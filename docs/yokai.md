This *should* be obvious, but this is only for Yo-kai editing, for most other tasks consult [general.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/general.md) or [head.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/head.md).

### Disclaimer
- This documentation is OUTDATED, an update is coming.
- This documentation won't help you if you don't understand what bytes, endianness and hex is. This is for programmers who want to understand how the save file system works for Yo-kai.

- The first Yo-kai starts at the `SectionID` `0x07` (7). Check `general.md` for more. This can semi-reliably be approximated to offset `0x5108`.
- Each Yo-kai has a 92 byte long entry (a length of `0x5C`).
- The first 2 bytes refer to #0 (sometimes called `num1`)
- The next 2 bytes refer to #1 (sometimes called `num2`)
   - It is unknown what these two do, although they are also present in Items, and might be related to sorting.
- The next 4 bytes refer to the Yo-kai's ID stored as a `Uint32` (32-bit Unsiged Integer). They are stored in `Little Endian` format, so reverse the `Byte Array` to convert to `Big Endian` before interpreting it as decimal. This being `0` can be used to determine that there is not a Yo-kai present in the current entry.
- The next 23 bytes refer to the Yo-kai's Nickname, set it to `0x0` if it has none. This can be decoded by treating it as a `UTF-8` string.
- The next byte is unknown
- This next 20 bytes aren't fully known in detail yet but are related to factors such as Unlocked Soultimates (Jibanyan), and move levels. The 11th byte refers to the atack level, the 15th refer's to the technique level, and the 18th refers to the soultimate level i.e. `0A` means level 10, `0x01` means level 1 etc.
- These 4 bytes refer to the Current XP toward the next level (not total XP). It should be decoded as a Little-endian `Uint32` (32-bit unsigned integer).
- These next 4 bytes refer to 2 seperate but similar things,  the first 2 bytes refer to the remaining HP of the Yo-kai, treat it as a signed 16-bit integer (`int16`). The other 2 bytes is assumed to refer to the Soultimate Guage, although this hasn't been verified yet.
- The next 4 bytes refer to the `OwnerID`. Which is the Yo-kai's original owner. Stored as hex.
- The next 5 bytes are IV's. Handle these as normal hex i.e. `0A` = 10, `01` = 1 etc
- The next 5 bytes are EV's. Again, handle these as normal hex i.e. `0A` = 10, `01` = 1 etc
- The next byte is unknown
- The next 4 bytes are Sport Center buffs, they store what values to change i.e. buffing one stat might give increase it's value by 5, and decrease another stat's value by 2. Note that because of the potential for negatives they must be interpreted as an `Int8` (Signed 8-bit Integer).
- The next byte is the Yo-kai's Level, it is intended to go from (0–99), but can technically reach level `255`.
- The next 4 bytes include several things, such as Win Pose data; The first byte is the selected pose, while the rest forms a bitmask of unlocked poses. Refer to my `data` folder for pose lookup code, as it is too complicated to explain here.
- The next byte combines Loafing behavior (high 4 bits) and Attitude (low 4 bits). Check the `data` dir for more information regarding decoding.
- The next 7 bytes include several pieces of data, such as the current Soultimate for Jibanyan and Alliance (BS/FS). The first (high) nibble of the 1st out of the 7 bytes refer to Bony/Fleshy `1` = Fleshy/Wicked, `0` = Bony. This stays the same for Type Rare and Version Exclusive Yo-kai. Very simple.

Example:
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

