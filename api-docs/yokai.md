# Yo-kai Save Editor API Documentation

A (hopefully) complete JS API for interacting with my save editor's Yo-kai branch

* How do you use the API?
* Follow this doc (XD), and paste the code into the JS console once your save file is loaded :>

## Quick Start

```javascript
console.log('=== Yo-kai Save Editor API ===');
console.log('Usage examples:');
console.log('  let yokai = getAllYokai();                    // Get all Yokai');
console.log('  yokai[0].set("level", 99);                    // Set level to 99');
console.log('  yokai[5].set("IV_Spd", 15);                   // Set speed IV');
console.log('  yokai[0].getRaw("energy");                    // Get raw hex bytes');
console.log('  yokai[0].setRaw("energy", "20 00 64 00");     // Set raw hex bytes');
console.log('  yokai[0].setHelper.energy.HP.set(500);        // Set HP using helper');
console.log('  yokai[0].setHelper.loafAndAi.loaf.set(3);     // Set loaf attitude');
console.log('  yokai[0].setHelper.specialEquip.alliance.set("Fleshy"); // Set alliance');
console.log('  let saveHex = exportSave();                   // Export as hex');
console.log('  setCP932(true);                               // Enable CP932 encoding');
console.log('  console.log(isCP932());                       // Check encoding mode');
```

## Core Utilities

### Basic Yokai Management

- **`getAllYokai()`** - Returns an array of all Yokai in the current save file.
- **`exportSave()`** - Exports the current save data as a hex string cuz why not :>
- **`setCP932(enabled)`** - Enable/disable CP932 text encoding for Japanese characters. Enable this for full support in JP save files (optional).
- **`isCP932()`** - Check if CP932 encoding is currently enabled. Again, enable this for full support in JP save files (optional).

### Yokai Object Methods

Each Yokai object returned by `getAllYokai()` has the following methods:

#### Standard Setters/Getters
- **`.set(field, value)`** - Set a field's value. DO NOT use this for complex fields.
- **`.get(field)`** - Get a field's value. DO NOT use this for complex fields.
- **`.getRaw(field)`** - Get raw hex bytes for a field. The complex field helper can help you with this so you don't need this for complex fields either lol.
- **`.setRaw(field, hexString)`** - Set raw hex bytes in the form of a string for a field i.e. `"60 00 A9 81"`. The complex field helper can help you with this so you don't need this for complex fields either lol.

Notes:
- Fields are **case-sensitive**.
- You cannot edit hex fields via `.set` it will throw an error. Use `.setRaw` and `.getRaw` instead.

#### Helper Methods

The `.setHelper` object provides (hopefully) convenient methods for complex fields to reder `setRaw` and `getRaw` mostly obsolete. Read the property list for use instructions.

## Usage Examples

### Basic Yokai Editing
```javascript
// Get all Yokai
let allYokai = getAllYokai();

// Level up first Yokai to max
allYokai[0].set("level", 99);

// set some garbage IVs for no reason whatsoever
allYokai[0].set("IV_Spd", 15);
allYokai[0].set("IV_Str", 15);
allYokai[0].set("IV_Spr", 15);
allYokai[0].set("IV_Def", 15);
```

### Advanced Editing with Helpers
```javascript
// Create a perfect Yokai by overwriting the first one (ALWAYS Jibanyan in legit saves)
let perfectYokai = allYokai[0];

// Max stats
perfectYokai.setHelper.energy.HP.set(999);
perfectYokai.setHelper.energy.Soul.set(999);

// Max abilities
perfectYokai.setHelper.specialUnlock.attackLevel.set(10);
perfectYokai.setHelper.specialUnlock.techniqueLevel.set(10);
perfectYokai.setHelper.specialUnlock.soultimateLevel.set(10);

// Set attitudes
perfectYokai.setHelper.loafAndAi.loaf.set(0);
perfectYokai.setHelper.loafAndAi.ai.set(2);  
```

### Raw Hex Editing
```javascript
// For devs who want direct hex manipulation
let energyHex = allYokai[0].getRaw("energy");
console.log("Current energy hex:", energyHex);

// Set raw energy data
allYokai[0].setRaw("energy", "FF FF FF FF 00 00 00 00");
```

### Property List & Editing Guide
This is a list on everything that *can* and *can't* be edited and how to do so.

num1:
- Edited via the default set and get methods: `yokai.set("num1", "5")`
   - Can also be edited via `setRaw()`, and `getRaw()`.
   - The complete purpose is unknown.

num2:
- Edited via the default set and get methods: `yokai.set("num2", "7")`
   - Can also be edited via `setRaw()`, and `getRaw()`.
   - The complete purpose is unknown.
  
youkaiId:
- Note: Also known as the yokai itself, type rares are considered seperate yokai. Also, beware of capitalization. Can be edited via the default set method: `yokai.set("youkaiId", ID)`.
   - Can also be edited via `setRaw()`, and `getRaw()`.
 
nickname:
- Edited via the default set and get methods: `yokai.set("nickname", "hi!")`
   - Can also be edited via `setRaw()`, and `getRaw()`. Renders as a black-box in game for invalid values and `""` or `00` represents no nickname used.
   - Uses cp932 (an extension of SHIFT_JIS) in JP versions, and UTF-8 in international versions, although this is automatically handled.

unused1:
- Edited via the hex set and get methods: `yokai.setRaw("unused1", "00")`. Also this currently has no known use.
   - Can NOT be edited via `set()`, and `get()`.
 
specialUnlock:
- Edited via the helpers OR hex set and get methods: `yokai.setRaw("specialUnlock", "0A 00...")`. This holds attack, technique and soultimate levels.
   - Can NOT be edited via `set()`, and `get()`.
   - Edit via `yokai.setHelper.specialUnlock.attackLevel.set()`, `yokai.setHelper.specialUnlock.soultimateLevel.set()` and `yokai.setHelper.specialUnlock.techniqueLevel.set()` respectively (and their get methods).
   - The highest valid value is 10 (`0x0A`)

expPoint:
- Edited via the default set and get methods: `yokai.set("expPoint", "32")`. Note: this represents the XP *TOWARDS* the next level, not the total XP :>
   - Can also be edited via `setRaw()`, and `getRaw()`.
 
energy:
- Edited via the helpers OR hex set and get methods: `yokai.setRaw("energy", "0A 00 01 0B")`. This holds the HP remaining and Soul guage.
   - Can NOT be edited via `set()`, and `get()`.
   - Edit via `yokai.setHelper.energy.HP.set()`, and `yokai.setHelper.energy.Soul.set(2)` along with their respective get methods. (Im too lazy to add them to this).
 
ownerId:
- Edited via the default set and get methods: `yokai.set("ownerId", "87f16038")`.
   - Can also be edited via `setRaw()`, and `getRaw()`.
 
IV_HP, IV_Str, IV_Spr, IV_Def, IV_Spd, EV_HP, EV_Str, EV_Spr, EV_Def, EV_Spd, SC_Str, SC_Spr, SC_Def, and SC_Spd:
- Edited via the default set and get methods: `yokai.set("SC_Str", "-3")` or `yokai.set("IV_HP", 2)`. Note: SC stats are signed, others are **not**.
   - Can also be edited via `setRaw()`, and `getRaw()`.
   - IVs can technically reach 127, but HP_IV's legal max is 80, and the other IVs cap out at 40. EVs cap out at 127. Legal SCs can't be smaller than -10 or larger than 25.

unknown:
- Edited via the hex set and get methods: `yokai.setRaw("unknown", "a")`. Also this currently has no known use.
   - Can NOT be edited via `set()`, but can be gotten as a string from `get()`.
 
level:
- Edited via the default set and get methods: `yokai.set("level", 21)`. Note: this can theoretically go up to 255, but the game automatically sets it to 99 if it's higher.
   - Can also be edited via `setRaw()`, and `getRaw()`.
   - Values > 99 automatically get set to 99 when the game is loaded.
 
special6:
- Edited via the helpers OR hex set and get methods: `yokai.setRaw("special6", "0A 00 01 0B...")`. This holds pose data.
   - Can NOT be edited via `set()`, and `get()`.
   - Edit via `yokai.setHelper.special6.unlockedPoses.set()`, and `yokai.setHelper.special6.currentPose.set()` along with their respective get methods.

loafAndAi:
- Edited via the helpers OR hex set and get methods: `yokai.setRaw("loafAndAi", "0A 00 01 0B...")`. This holds attitude data. Loaf is loafing attitude, AI is move attitude/frequency/moveAI
   - Can NOT be edited via `set()`, and `get()`.
   - Edit via `yokai.setHelper.loafAndAi.ai.set()`, and `yokai.setHelper.loafAndAi.loaf.set()` along with their corresponding get methods.

specialEquip:
- Edited via the helpers OR hex set and get methods: `yokai.setRaw("specialEquip", "0A 00 01 0B...")`. This holds Jibanyan's soultimate and Alliance (also known as Bony/Fleshy) data. Jibanyan's Alliance (and perhaps other story-befriends, and wicked yokai) *CANNOT* be changed due to how the game handles this in save files.
   - Can NOT be edited via `set()`, and `get()`.
   - Edit via `yokai.setHelper.specialEquip.alliance.set()`, and `yokai.setHelper.specialEquip.equipment.set()` (I totally didn't call it equipment because im brain-dead and too lazy to change it) along with their corresponding get methods. Note: for `alliance.set` any value that isnt "Bony" (case-insensitive) is considered fleshy/wicked/other. JIBANYAN SOULTIMATE IS CURRENTLY BROKEN USE HEX EDITING VIA SET AND GETRAW


### Save Management
```javascript
// Enable Japanese text support
setCP932(true);

// Make edits...
allYokai[0].set("level", 99);

// Export modified save
let modifiedSave = exportSave();
console.log("Modified save hex:", modifiedSave);
```

## Notes

- This API currently has input validation lazier than me so don't put the wrong value :P
- Always call `getAllYokai()` first to load Yokai data
- USE THE HELPER METHODS FOR COMPLEX FIELDS, IF YOU USE THE HEX EDITOR WHY??????
- Raw hex editing requires understanding of the save file format, so AGAIN, USE THE HELPERS!!!!
- CP932 encoding is needed for proper Japanese character support, there is no way around this for now.
- Changes are applied immediately to the in-memory save data so yeah :<
- Use `exportSave()` to get the final modified save file

DEV NOTES:
FIX SOME ADVANCED FIELDS
ADD REFRESH VIEW FOR USERS
ADD WAYS TO EXPOSE LOADING SAVE FILES BEFORE INIT WITHOUT USING DOM SHENANIGANS
ADD API SUPPORT FOR THE ITEMS EDITOR, GENERAL EDITOR ETC
