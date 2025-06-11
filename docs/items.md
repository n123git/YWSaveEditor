**Note**: this should be quite obvious, but this is only for items, for most features consult [general.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/general.md).

# General

Items can be split into:
* Consumables(Everything that isn't the below)
    * Critters (Insects and Fish) are also in this category.
* Equipment (Equipment.... obvious enough)
* Souls (Souls... pretty obvious)
* Key Items (Note: Internally Key Items that display an image on click such as maps, are considered a different category, but this dosen't apply to save files)

Tip: ItemIDs use a CRC-32 Checksum - all IDs do this (including Map and Yokai IDs).

These categories define what `SectionID` they are relative to, this defines where they are placed. Consult [general.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/general.md) for more info.

* Consumables: + Critters `0x04`
* Equipment: `0x05`
* Key Items: `0x06`
* Souls: `0x3000` from `0x13`

---

## Souls

Each Soul is `0xC` long, and begins at offset `0x3000` relative to its `SectionID`. Also note that there are `100` Soul Slots in a save file, meaning that it ends at `0x4B0` (13488) from its `SectionID`.

Each Soul contains:

<!-- you wont believe how long it took me to realise each row can have a different length -->
<!-- I also didnt know that comments malformed a table but whatever -->
| Offset | Type    | Name             | Description           | 
|--------|---------|------------------|-----------------------|
| 0x00   | uint16  | num1             | #0's complete purpose is unknown, but it is known to be used for sorting/reference. |
| 0x02   | uint16  | num2             | #1's complete purpose is unknown, but it is known to be used for sorting/reference. |
| 0x04   | uint32  | ItemID           | The item's ItemID, check my `data` folder for a list of all ItemID's.|
| 0x08   | uint16  | XPUntilLevelUp   | The amount of XP obtained *towards* the next level, not in total. |
| 0x0A   | uint16  | Level            | The Souls level. |

---

## Key Items

Each Key Item is `0x8` long, and begins at offset `0x2000` relative to its `SectionID`. Also note that there are `180` Key Item Slots in a save file.

Each Key Item contains:

| Offset | Type    | Name             | Description           | 
|--------|---------|------------------|-----------------------|
| 0x00   | uint16  | num1             | #0's complete purpose is unknown, but it is known to be used for sorting/reference. |
| 0x02   | uint16  | num2             | #1's complete purpose is unknown, but it is known to be used for sorting/reference. |
| 0x04   | uint32  | ItemID           | The item's ItemID, check my `data` folder for a list of all ItemIDs.|

--

## Equipment

Each Equipment is `0x10` long, and begins at offset `0x1000` relative to its `SectionID`. Also note that there are `90` Equipment Slots in a save file.

Each Equipment contains:

| Offset | Type    | Name             | Description           | 
|--------|---------|------------------|-----------------------|
| 0x00   | uint16  | num1             | #0's complete purpose is unknown, but it is known to be used for sorting/reference. |
| 0x02   | uint16  | num2             | #1's complete purpose is unknown, but it is known to be used for sorting/reference. |
| 0x04   | uint32  | ItemID           | The item's ItemID, check my `data` folder for a list of all ItemIDs.|
| 0x08   | uint8   | ItemCountA       | The amount of (unequipped?) equipment. |
| 0x04   | uint8   | ItemCountB        | The amount of (equipped?) equipment.|

## Consumables+Critters

Each Consumable/Critter is `0x0C` long, and begins at offset `0x0` relative to its `SectionID`. Also note that there are `XX` Consumable/Critter Slots in a save file.

Each Slot contains: 

| Offset | Type    | Name             | Description           | 
|--------|---------|------------------|-----------------------|
| 0x00   | uint16  | num1             | #0's complete purpose is unknown, but it is known to be used for sorting/reference. |
| 0x02   | uint16  | num2             | #1's complete purpose is unknown, but it is known to be used for sorting/reference. |
| 0x04   | uint32  | ItemID           | The item's ItemID, check my `data` folder for a list of all ItemIDs.|
| 0x08   | uint8   | ItemCount       | The quantity of said item. |

--

[TODO: FINSIH THIS PAGE]
