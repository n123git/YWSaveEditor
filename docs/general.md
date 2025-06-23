# Introduction

The Save System is composed of 2 little-endian file types:
 * `game.yw` files, these hold the main save file data and are named as such, where `game1.yw` is the 1st save file, `game2.yw` is the 2nd, and `game3.yw` is the 3rd and final save file. These store everything not in the `head.yw`'s and use a `SectionID` format.
 * `head.yw` files, these contain the player's name, encryption keys along with their preview data (all the data seen before you click on a save file). Note that editing most of the preview data is usually pointless as saving will restore it to it's correct value, although this dosen't apply to all of them. More precise documentation can be found in `data/head.md`. These are encrypted identically to `YW1` save files.

# SectionID Format

These save files are mostly nested sequences of Sections. The `SectionID` structure uses nested sections where each section starts with two 4-byte header words. There are two forms of Header Words:

* `h1` - an `int16` enum which determines whether it's a section start or section end, specifically `0xFFFE` is a Section Start Marker, and `0xFEFF` is a Section End Marker.
* `h2` - a 4-byte integer (`int16`) with the following structure:

* **Bits 0–7** (LSB) → `Section ID`
* **Bits 8–31** → `Section Size` in bytes (excluding the 8 bytes of headers, i.e., h1 and h2)

---

## **Section ID (ID)**

This is a `Uint8` (0-255) used to uniquely identify the section, kind of like a `UUID`. For instance, `0x06` is the SectionID for Key Items, which is constant, and NEVER varies between save files, despite the exact offset being inconsistent, and therefore cannot directly be used.

### Example tree:

```
Root (ID: 1, size: 64)
├── Child (ID: 5, size: 32)
│   └── Grandchild (ID: 9, size: 8)
└── Child (ID: 6, size: 12)
```

Here is a basic example of an entry in that tree (values are in hex because ye):

| Offset | Bytes                                                   | Meaning                                |
| ------ | ------------------------------------------------------- | -------------------------------------- |
| 0x00   | `FE FF 00 00`                                           | `h1` = Section Start Marker (`0xFFFE`) |
| 0x04   | `F1 34 15 01`                                           | `h2` = Section Header Data             |
|        | → Reversed: `01 15 34 F1` (little-endian to big-endian) |                                        |
|        | → Full value: `0x011534F1`                              |                                        |
|        | → ID = **`0xF1`** (last byte)                           |                                        |
|        | → Size = **`0x011534`** (upper 3 bytes = 70964 bytes)   |                                        |
| 0x08   | (data)                                                  | Payload of section ID 1                |
| ...    | ...                                                     | Data. Can be nested.                   |
| 0x1153C| `FF FE`                                                 | End Marker (0xFEFF)                    |

Here is a basic JS tool to parse SectionIDs:
```js

(function (global) {
  class Section {
    constructor(id, size, pos) {
      this.id = id;
      this.size = size;
      this.pos = pos;
      this.children = [];
    }
    addChild(child) {
      this.children.push(child);
    }
  }

  function parseSavedata(fullBytes) {
    if (!(fullBytes instanceof Uint8Array)) {
      throw new Error("Input must be a Uint8Array.");
    }

    if (fullBytes.length <= 0x20) {
      console.error("Input too short to skip 0x20 offset.");
      return null;
    }

    const bytes = fullBytes.slice(0x20); // skip the part outside of the first/main SectionID (0x01)
    const view = new DataView(bytes.buffer, bytes.byteOffset, bytes.byteLength);
    const stack = [];
    let root = null;
    let pos = 0;

    function atEnd() { // self-explanatory
      return pos >= bytes.length;
    }

    if (pos + 4 > bytes.length) return null;
    let h1 = view.getUint32(pos, true);
    pos += 4;

    if ((h1 & 0xFFFF) !== 0xFFFE) return null; // if the low 16-bits == 0xFFFE, then continue, also gotta love magic numbers

    if (pos + 4 > bytes.length) return null; // no data
    let h2 = view.getUint32(pos, true);
    pos += 4;

    let id = h2 & 0xFF; // get the low 8-bits of h2 as id
    let size = h2 >>> 8; // shift right by 8 bits — you get the remaining 24 bits as size

    root = new Section(id, size, pos); // init the new sectionID
    stack.push(root);

    while (!atEnd()) {
      if (pos + 4 > bytes.length) break; // too short/no data left
      h1 = view.getUint32(pos, true);
      pos += 4;

      while ((h1 & 0xFFFF) === 0xFFFE) { // same check as earlier
        if (pos + 4 > bytes.length) return null;

        h2 = view.getUint32(pos, true);
        pos += 4;
        id = h2 & 0xFF; // again grab the ID
        size = h2 >>> 8; // and the size

        if (stack.length === 0 || pos + size > bytes.length) return null; // very sad oh noes

        const sec = new Section(id, size, pos); // init a section
        stack[stack.length - 1].addChild(sec); // now we
        stack.push(sec); // add it to stack

        if (pos + 4 > bytes.length) break; // too short/no data left
        h1 = view.getUint32(pos, true);
        pos += 4;
      }

      if ((h1 & 0xFFFF) === 0xFEFF) {
        if (stack.length === 0) return null;
        stack.pop();
      } else {
        if (stack.length === 0) {
          console.warn("Data outside sections, stopping parse but exporting tree anyway.");
          break; // Don't return null — just stop parsing.
        }

        const current = stack[stack.length - 1];
        const dataToSkip = current.size - 4;
        if (pos + dataToSkip > bytes.length) return null;
        pos += dataToSkip;
      }
    }

    if (!root) return null;

    // Final check: if the stack is unbalanced, log a warning, but still return the result
    if (stack.length !== 0) {
      console.warn("Unbalanced sections: some sections were not closed properly.");
    }

    return sectionToObject(root);
  }

  function sectionToObject(section) {
    return {
      id: section.id,
      size: section.size,
      pos: section.pos,
      children: section.children.map(sectionToObject),
    };
  }

function grabOffset(node, targetId) {
  console.log(`Checking node id: ${node.id}`);
  if (node.id === targetId) {
    console.log(`Found id ${targetId}, pos: ${node.pos}`);
    return node.pos;
  }

  for (const child of node.children) {
    const pos = grabOffset(child, targetId);
    if (pos !== null) {
      return pos;
    }
  }

  return null;
}

  global.SaveDataParser = {
    parseSavedata,
  };

global.grabOffset = grabOffset;

global.IDObj = { /* The IDs for different data sections */
  "Yokai": "0x07",
  "Item": "0x04",
  "Equipment": "0x05",
  "Important": "0x06",
  "misc1": "0x01",
  "misc9": "0x09",
  "Soul": "0x13",
  "Medallium": "0x01",
  "misc8": "0x14"
}
global.genOffset = {
  "Yokai": 0x20,
  "Important": 0x20
} /* The general offset constants i.e. for Yokai it starts at the ID 0x07 but the first 0x20 bytes aren't for any Yo-kai in particular, and I havent figured out what they do, as of me writing this (I probably have, and just haven't updated this */
})(window);

```
## Misc Notes

`game*.yw` contains some data **before** and **after** the first top-level `Section`. This data is accessed via **absolute** offsets, and is therefore not relative to any `Section`. In *Yo-kai Watch 2*, there are exactly **32 bytes (`0x20`) before** the first `Section` start marker.

# Common Data Types
Data is usually stored as either a `uint8`, `uint32` or ocasionally a `uint16` and `uint64`, signed integers are ocasionally used. For a large series of binary data, bitmasks are used. Examples include Trophies and Unlocked Win Poses. A bitmask is a series of binary data used to represent a high amount of binary data i.e.
(Arbitrary binary length, DO NOT USE THE LENGTH FOR REFERENCE)

00000000000000000000 → No trophies<br/>
11111111111111111111 → All trophies<br/>
10000000000000000001 → First and last trophy only

Also note that all IDs are saved as their CRC-32 Checksum. An example of this is location; your Location is stored as the CRC-32 Checksum of it's file name. A list of all the file names can be found [here](https://tcrf.net/Notes:Yo-kai_Watch_2). And other examples include Items and Yo-kai. Also note that (THIS IS VERY IMPORTANT). JP copies use cp932, whereas international save files use UTF-8

### Examples
The data found in the `game*.yw` and `head.yw` files are, in detail documented in their individual pages:
* [docs/head.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/head.md) for `head.yw`, including Preview Data, Player Name and more.
* [docs/decrypt.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/decrypt.md) for encryption/decryption documentation.
* [docs/yokai.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/yokai.md) for Yo-kai
* [docs/items.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/items.md) for all the different types of Items
* [docs/misc.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/misc.md) for a TON of generic ones such as Trophies, Crank-a-kai's, Daily Events, and more!


## Unconfirmed
- After a h2 there is always 32 bytes (`0x20`) to skip past. This is correct for Key Items and Yo-kai.
