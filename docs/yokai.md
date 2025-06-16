This *should* be obvious, but this is only for Yo-kai editing, for most other tasks consult [general.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/general.md) or [head.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/head.md).


This documentation won't help you if you don't understand basic hex constructs and endianness.

---

**Note**: assume the data is little-endian unless stated otherwise and the "Offsets" are in bytes, NOT bits.
Yo-kai begin at the `SectionID` `0x07` (7). Check `general.md` for more. 
  - This can semi-reliably be approximated to the absolute offset `0x5108` 
   
Each Yo-kai entry is has a length of `0x5C` (92) bytes.

| Offset | Type    | Name             | Description           | 
|--------|---------|------------------|-----------------------|
| 0x00   | uint16  | num1             | #0's complete purpose is unknown, but it is known to be used for sorting/reference. |
| 0x02   | uint16  | num2             | #1's complete purpose is unknown, but it is known to be used for sorting/reference. |
| 0x04   | uint32  | YokaiID          | The Yo-kai's ID, note that Type Rares have different IDs, and so do boss vs befriendable Yo-kai. Check my `data` folder for a list of all Yo-kai ID's. 0 = Empty slot.|
| 0x08   | uint24  | Nickname         | The Yo-kai's Nickname in UTF-8. `0x0` = No Nickname. Unused characters render as a filled black box.|
| 0x20   | uint20  | Misc             | The 11th byte refers to the atack level, the 15th refer's to the technique level, and the 18th refers to the soultimate level i.e. `0A`= level 10.|
| 0x34   | uint32  | XP               | The Current XP *towards* the next level, NOT the total XP. |
| 0x38   | int16   | HP Remaining     | The Current amount of HP Remaining. |
| 0x3A   | int16   | Soul Remaining   | The Current amount of Soul Remaining. |
| 0x3C   | uint32  | OwnerID          | The Owner of the Yo-kai, used for trading. |
| 0x40   | uint8   | IV_HP            | IV for HP. | 
| 0x41   | uint8   | IV_Str           | IV for STR (Strength).| 
| 0x42   | uint8   | IV_Spr           | IV for SPR (Spirit). | 
| 0x43   | uint8   | IV_Def           | IV for DEF (Defense).| 
| 0x44   | uint8   | IV_Spd           | IV for SPD (Speed).|
| 0x45   | uint8   | EV_HP            | EV for HP. | 
| 0x46   | uint8   | EV_Str           | EV for STR (Strength).| 
| 0x47   | uint8   | EV_Spr           | EV for SPR (Spirit). | 
| 0x48   | uint8   | EV_Def           | EV for DEF (Defense).| 
| 0x49   | uint8   | EV_Spd           | EV for SPD (Speed).|
| 0x4A   | uint8   | unknown          | Unknown. |
| 0x4B   | int8    | SC_Str           | SC buff/nerf for STR (Strength). |
| 0x4C   | int8    | SC_Spr           | SC buff/nerf for SPR (Spirit). |
| 0x4D   | int8    | SC_Def           | SC buff/nerf for DEF (Defense). |
| 0x4E   | int8    | SC_Spd           | SC buff/nerf for SPD (Speed).|
| 0x4F   | uint8   | Level            | The Yo-kai's level usually 0-99 (`0x0`-`0x63`). Levels higher than `0x63` will automatically be changed to 99 (`0x63`) by the game. |
| 0x50   | uint32  | Win Pose Data    | The first byte is the selected pose, while the rest forms a bitmask of unlocked poses. Refer to my `data` folder for pose lookup code, as it is too complicated to explain here. |
| 0x54   | uint8   | Loaf Data        | Loafing behavior (high 4 bits) and Attitude (low 4 bits). Check the `data` dir for more information regarding decoding. |

- The next 7 bytes include several pieces of data, such as the current Soultimate for Jibanyan and Alliance (BS/FS). The first (high) nibble of the 1st out of the 7 bytes refer to Bony/Fleshy `1` = Fleshy/Wicked, `0` = Bony. This stays the same for Type Rare and Version Exclusive Yo-kai, but cannot be used for Jibanyan (and potentially other Auto-befriend Yo-kai). Very simple.



Outdated Example:
     | Offset | Size | Field         | Description                                         | Raw Hex Bytes                                      | Decoded                                                                 |
|--------|------|---------------|-----------------------------------------------------|----------------------------------------------------|-------------------------------------------------------------------------|
| 0x00   | 2    | num1          | Index #0                                            | 7B 00                                              | 123                                                                     |
| 0x02   | 2    | num2          | Index #1                                            | 7C 00                                              | 124                                                                     |
| 0x04   | 4    | youkaiId      | Actual Yo-kai species ID                            | BA E6 F9 FC                                        | fcf9e6ba (4244235962/Unfairy, 355)                                     |
| 0x08   | 23   | nickname      | UTF-8 string, max 22 chars + null                   | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 |                                                                         |
| 0x1f   | 1    | unused1       | (unused)                                            | 00                                                 | 00                                                                      |
| 0x20   | 20   | SpecialUnlock | Dictates whether special features are unlocked      | 00 00 00 00 00 00 00 00 00 00 0A 00 00 00 0A 00 00 00 0A 00           | Unknown: (0), Attack Level: 10, Technique Level: 10, Soultimate Level: 10 |
| 0x34   | 4    | expPoint      | Current experience points                           | 00 00 00 00                                        | 0                                                                       |
| 0x38   | 4    | Energy        | Contains HP Remaining among other features          | 14 02 A2 00                                        | HP Remaining: (20), Soul Bar Remaining (Unverified): (2)               |
| 0x3c   | 4    | ownerId       | Owner ID (hex displayed)                            | BF 84 4C 22                                        | 575440063                                                               |
| 0x40   | 1    | IV_HP         | Individual stat                                     | 12                                                 | 18                                                                      |
| 0x41   | 1    | IV_Str        | Individual stat                                     | 07                                                 | 7                                                                       |
| 0x42   | 1    | IV_Spr        | Individual stat                                     | 06                                                 | 6                                                                       |
| 0x43   | 1    | IV_Def        | Individual stat                                     | 0B                                                 | 11                                                                      |
| 0x44   | 1    | IV_Spd        | Individual stat                                     | 07                                                 | 7                                                                       |
| 0x45   | 1    | EV_HP         | EV                                                  | 00                                                 | 0                                                                       |
| 0x46   | 1    | EV_Str        | EV                                                  | 00                                                 | 0                                                                       |
| 0x47   | 1    | EV_Spr        | EV                                                  | 00                                                 | 0                                                                       |
| 0x48   | 1    | EV_Def        | EV                                                  | 00                                                 | 0                                                                       |
| 0x49   | 1    | EV_Spd        | EV                                                  | 00                                                 | 0                                                                       |
| 0x4a   | 1    | unknown       | Unknown                                             | 00                                                 | 00                                                                      |
| 0x4b   | 1    | SC_Str        | Sports Center stat modifier (signed)               | 00                                                 | 0                                                                       |
| 0x4c   | 1    | SC_Spr        | Sports Center stat modifier (signed)               | 00                                                 | 0                                                                       |
| 0x4d   | 1    | SC_Def        | Sports Center stat modifier (signed)               | 05                                                 | 5                                                                       |
| 0x4e   | 1    | SC_Spd        | Sports Center stat modifier (signed)               | FE                                                 | -2                                                                      |
| 0x4f   | 1    | level         | Level (0–99)                                        | 63                                                 | 99                                                                      |
| 0x50   | 4    | Special6      | Win Pose Data and More                              | 00 00 00 00                                        | Selected Win Pose: ("Win Pose 1"), Unlocked Win Poses: ("Pose 1")      |
| 0x54   | 1    | loafAndAi     | High 4 bits = loaf, low 4 bits = AI (Attitude)      | 20                                                 | Loaf: 2 (Casual), Attitude: 0 (N/A)                                    |
| 0x55   | 7    | SpecialEquip  | Includes special equip data                         | 19 00 00 36 00 30 0B                               | (Default Soultimate)                                                   |

