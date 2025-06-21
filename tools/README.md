Hi! This directory contains:
* Tips to develop YW save editors
* Tools I personally developed to aid in the understanding/developement of the system in question.

# Step 1. "Know your stuff"
You will ***seriously*** struggle if you don't know what you're doing. Brush up on endianness, bitmasks, checksums (specifically CRC-32) and common hex structures such as `uint32`, `UTF-8` and maybe even specialised ones like `cp932`.

# Step 2. General Tools
You'll want some general tools such as:
* Save De/encryptor
* Hex Editor - I personally use `HxD`, but others are fine too.
And a tool to compare differences, usually between batches/groups to weed out factors such as time.

I've personally made a `SectionID` parsing tool, which is an unused HTML placed in the `data` folder of releases. I have also made a batch difference checker via a label/ID system, but I lost the newer version which uses `SectionID` parsing. Both are out of date do NOT use them. For archival purposes the tools can be found....
Currently, i'd recommend 
