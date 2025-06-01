# Introduction

The Save System is composed of 2 core file types:
 * `game.yw` files, these hold the main save file data and are named as such, where `game1.yw` is the 1st save file, `game2.yw` is the 2nd, and `game3.yw` is the 3rd and final save file. These store everything not in the `head.yw`'s and use a `SectionID` format.
 * `head.yw` files, these contain the player's name, encryption keys and preview data (everything seen before you click on a save file). Note that editing the preview data is usually pointless as saving will restore it to it's correct value for obvious reasons (It is updated real-time? real-save? idk what to call it XD). More precise documentation can be found in `data/head.md`. These are encrypted identically to `YW1` save files.

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

---

## Unconfirmed
- After a h2 there is always 32 bits (`0x20`) to skip past. This is correct for Key Items and Yo-kai.
