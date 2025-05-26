- Each Yo-kai is `0x5C` bytes long
- The first 2 bytes refer to #0 (sometimes called `num1`)
- The next 2 bytes refer to #1 (sometimes called `num2`)
   - It is unknown what these 2 do, although they are also present in Items, and might be related to sorting.
- The next 4 bytes refer to the Yo-kai's ID, to decode this you first reverse the array of bytes and join them (to make it big endian) and then convert it to decimal.
- The next 23 bytes Refer to the Yo-kai's Nickname, make it all 0's if it has none. This can be decoded by treating it as a UTF-8 string.
- The next byte is unknown
- This next 20 bytes aren't fully known in detail yet.
- These 4 bytes refer to the XP for this level and does not count the XP used to reach the previous levels i.e. if it takes 500 XP to level up to the next level, and they are half way there they have 250XP. Decode it as a little-endian 32-bit unsigned integer (`Uint32`)
- These next 4 bytes refer to several things, but the first 2 bytes refer to the remaining HP of the Yo-kai, treat it as a signed 16-bit integer (`int16`).
- The next 4 bytes refer to the OwnerID
- The next 5 bytes are IV's
- The next 5 bytes are EV's
- The next byte is unknown
- The next 4 bytes are Sport Center buffs, although there are some problems interpereting these right now.
- The next byte is the level
- The next 4 bytes include several things, including the unlocked Win Poses and the selected one. To get the selected winpose you get the first part, which corresponds to a Pose, and can therfore be looked up via an array stored in my `data` dir. The unlocked winposes are simply a bitmask, read my code in the `data` dir for more information.
- The next byte handles both attitude's, where the 2 hex characters correspond to the 2 attitude's. Check the `data` dir for more information
- The next 7 bytes include several pieces of data, such as the current Soultimate for Jibanyan.

For example:
| Offset | Size | Field         | Description                                                        | Raw Hex Bytes                                              | Decoded                                                                 |
|--------|------|---------------|--------------------------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------------------|
| 0x00   | 2    | num1          | Index #0                                                           | 00 00                                                      | 0                                                                       |
| 0x02   | 2    | num2          | Index #1                                                           | 01 00                                                      | 1                                                                       |
| 0x04   | 4    | youkaiId      | Actual Yo-kai species ID                                           | A8 8A A6 A3                                                | a3a68aa8 (2745600680 / Jibanyan, 135)                                   |
| 0x08   | 23   | nickname      | UTF-8 string, max 22 chars + null                                  | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 |                                                                         |
| 0x1f   | 1    | unused1       | (unused)                                                           | 00                                                         | 00                                                                      |
| 0x20   | 20   | SpecialUnlock | Dictates whether special features are unlocked i.e. Nyaight        | 29 10 2A 00 00 00 00 00 00 00 0A 08 00 00 0A 08 00 00 0A 08 | Unknown: (41)                                                           |
| 0x34   | 4    | expPoint      | Current experience points                                           | 00 00 00 00                                                | 0                                                                       |
| 0x38   | 4    | Energy        | Contains HP Remaining among other features                          | 7F 01 4B 00                                                | HP Remaining: (127)                                                     |
| 0x3c   | 4    | ownerId       | Owner ID (hex displayed)                                           | BF 84 4C 22                                                | 575440063                                                               |
| 0x40   | 1    | IV_HP         | Individual stat                                                    | 10                                                         | 16                                                                      |
| 0x41   | 1    | IV_Str        | Individual stat                                                    | 08                                                         | 8                                                                       |
| 0x42   | 1    | IV_Spr        | Individual stat                                                    | 08                                                         | 8                                                                       |
| 0x43   | 1    | IV_Def        | Individual stat                                                    | 08                                                         | 8                                                                       |
| 0x44   | 1    | IV_Spd        | Individual stat                                                    | 08                                                         | 8                                                                       |
| 0x45   | 1    | EV_HP         | EV                                                                 | 14                                                         | 20                                                                      |
| 0x46   | 1    | EV_Str        | EV                                                                 | 0A                                                         | 10                                                                      |
| 0x47   | 1    | EV_Spr        | EV                                                                 | 00                                                         | 0                                                                       |
| 0x48   | 1    | EV_Def        | EV                                                                 | 00                                                         | 0                                                                       |
| 0x49   | 1    | EV_Spd        | EV                                                                 | 00                                                         | 0                                                                       |
| 0x4a   | 1    | unknown       | Unknown                                                            | 00                                                         | 00                                                                      |
| 0x4b   | 1    | SC_Str        | Sports Center stat modifier (signed)                               | 00                                                         | 0                                                                       |
| 0x4c   | 1    | SC_Spr        | Sports Center stat modifier (signed)                               | 00                                                         | 0                                                                       |
| 0x4d   | 1    | SC_Def        | Sports Center stat modifier (signed)                               | 05                                                         | 5                                                                       |
| 0x4e   | 1    | SC_Spd        | Sports Center stat modifier (signed)                               | FE                                                         | 254                                                                     |
| 0x4f   | 1    | level         | Level (0–99)                                                       | 63                                                         | 99                                                                      |
| 0x50   | 4    | Special6      | Win Pose Data and More                                             | 04 03 01 00                                                | Selected Win Pose: ("Random Win Pose"), Unlocked Win Poses: ("Pose 1") |
| 0x54   | 1    | loafAndAi     | High 4 bits = loaf, low 4 bits = AI (Attitude)                     | 01                                                         | Loaf: 0 (Serious), Attitude: 1 (Grouchy)                                |
| 0x55   | 7    | SpecialEquip  | Includes special equip data i.e. Soultimate for Jibanyan          | 11 C4 0F 05 01 DC 09                                       | (Default Soultimate)                                                   |
