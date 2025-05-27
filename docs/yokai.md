### Disclaimer
- This documentation won't help you if you don't understand what bytes, endianness and hex is. This is for programmers who want to understand how the save file system works for Yo-kai.

- Each Yo-kai has a 92 byte long entry (a maximum offset of `0x5C`).
- The first 2 bytes refer to #0 (sometimes called `num1`)
- The next 2 bytes refer to #1 (sometimes called `num2`)
   - It is unknown what these two do, although they are also present in Items, and might be related to sorting.
- The next 4 bytes refer to the Yo-kai's ID stored as a `Uint32` (32-bit Unsiged Integer). They are stored in `Little Endian` format, so reverse the `Byte Array` to convert to `Big Endian` before interpreting it as decimal.
- The next 23 bytes refer to the Yo-kai's Nickname, set it to `0x0` if it has none. This can be decoded by treating it as a `UTF-8` string.
- The next byte is unknown
- This next 20 bytes aren't fully known in detail yet but are related to factors such as Unlocked Soultimates (Jibanyan) and move levels. The 11th byte refers to the atack level, the 15th refer's to the technique level, and the 18th refers to the soultimate level i.e. `0A` means level 10, `0x01` means level 1 etc.
- These 4 bytes refer to the Current XP toward the next level (not total XP). It should be decoded as a Little-endian `Uint32` (32-bit unsigned integer).
- These next 4 bytes refer to several things, but the first 2 bytes refer to the remaining HP of the Yo-kai, treat it as a signed 16-bit integer (`int16`). The other 2 bytes is assumed to refer to the Soultimate Guage, although this hasn't been verified yet.
- The next 4 bytes refer to the `OwnerID`. Which is the Yo-kai's original owner. Stored as hex.
- The next 5 bytes are IV's. Handle these as normal hex i.e. `0A` = 10, `01` = 1 etc
- The next 5 bytes are EV's. Again, handle these as normal hex i.e. `0A` = 10, `01` = 1 etc
- The next byte is unknown
- The next 4 bytes are Sport Center buffs, they store what values to change i.e. buffing one stat might give increase it's value by 5, and decrease another stat's value by 2. Note that because of the potential for negatives they must be interpreted as an `Int8` (Signed 8-bit Integer).
- The next byte is the Yo-kai's Level, it is intended to go from (0–99), but can technically reach level `255`.
- The next 4 bytes include several things, such as Win Pose data; The first byte is the selected pose, while the rest forms a bitmask of unlocked poses. Refer to my `data` folder for pose lookup code, as it is too complicated to explain here.
- The next byte combines Loafing behavior (high 4 bits) and Attitude (low 4 bits). Check the `data` dir for more information regarding decoding.
- The next 7 bytes include several pieces of data, such as the current Soultimate for Jibanyan.
     
