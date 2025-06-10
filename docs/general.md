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

This is a `Uint8` (0-255) useed to uniquely identify the section, similar to a `UUID`. For example, `0x06` is the SectionID for Key Items, which is ALWAYS correct, despite the exact offset being extremely inconsistent, and therefore cannot directly be used.

### Example tree structure (abstract):

```
Root (ID: 1, size: 64)
├── Child (ID: 5, size: 32)
│   └── Grandchild (ID: 9, size: 8)
└── Child (ID: 6, size: 12)
```

Here is a basic example of an entry in that tree (values in hex):

| Offset | Bytes         | Meaning                  |
| ------ | ------------- | ------------------------ |
| 0x00   | `FE FF 00 00` | Start Marker (0xFFFE)    |
| 0x04   | `40 00 00 01` | Size=0x00000040, ID=0x01 |
| 0x08   | (data)        | Payload of section ID 1  |
| ...    | ...           | Data. Can be nested.     |
| 0x48   | `FF FE 00 00` | End Marker (0xFEFF)      |


## Misc Notes

`game*.yw` contains some data **before** and **after** the first top-level `Section`. This data is accessed via **absolute** offsets, and is therefore not relative to any `Section`. In *Yo-kai Watch 2*, there are exactly **32 bytes (`0x20`) before** the first `Section` start marker.

# Common Data Types
Data is usually stored as either a `uint8`, `uint32` or ocasionally a `uint16` and `uint64`, signed integers are ocasionally used. For a large series of binary data, bitmasks are used. Examples include Trophies and Unlocked Win Poses. A bitmask is a series of binary data used to represent a high amount of binary data i.e.
(Arbitrary binary length, DO NOT USE THE LENGTH FOR REFERENCE)

00000000000000000000 → No trophies<br/>
11111111111111111111 → All trophies<br/>
10000000000000000001 → First and last trophy only

Also note that all IDs are stored as a CRC-32 Checksum, for example your Location is stored as the CRC-32 Checksum of the Location's file name. A list of all the file names can be found [here](https://tcrf.net/Notes:Yo-kai_Watch_2)


### Examples
The data found in the `game*.yw` and `head.yw` files are, in detail documented in their individual pages:
* [docs/head.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/head.md) for `head.yw`, including Preview Data, Player Name and more.
* [docs/yokai.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/yokai.md) for Yo-kai
* [docs/items.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/items.md) for all the different types of Items
* [docs/general.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/general.md) for a TON of generic ones such as Trophies, Crank-a-kai's, Daily Events, and more!


## Unconfirmed
- After a h2 there is always 32 bytes (`0x20`) to skip past. This is correct for Key Items and Yo-kai.
