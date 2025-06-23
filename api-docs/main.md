# Yo-kai Save Editor API Documentation

A comprehensive JavaScript API for interacting with the Yo-kai editor for Yo-kai Watch 2 save files.

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
- You cannot edit hex fields via `.set` it will throw an error. Use `.setRaw` instead. The same applies to `get` and `getRaw` respectively.

#### Helper Methods

The `.setHelper` object provides (hopefully) convenient methods for complex fields to reder `setRaw` and `getRaw` mostly obsolete:

##### Energy Management
```javascript
yokai[0].setHelper.energy.HP.set(yokai[0].setHelper.energy.HP.get() - 50);      // Set HP
yokai[0].setHelper.energy.Soul.set(250);    // Set Soul bar
```

##### Special Abilities
```javascript
yokai[0].setHelper.specialUnlock.attackLevel.set(5);     // Attack level
yokai[0].setHelper.specialUnlock.techniqueLevel.set(3);  // Technique level  
yokai[0].setHelper.specialUnlock.soultimateLevel.set(7); // Soultimate level
```

##### Poses & Animations
```javascript
yokai[0].setHelper.special6.currentPose.set(2);           // Current pose
yokai[0].setHelper.special6.unlockedPoses.set([0,1,2,3]); // Unlocked poses array
```

##### Personality & AI
```javascript
yokai[0].setHelper.loafAndAi.loaf.set(3);  // Loaf attitude (0-7)
yokai[0].setHelper.loafAndAi.ai.set(1);    // AI behavior pattern
```

##### Equipment & Alliance
```javascript
yokai[0].setHelper.specialEquip.equipment.set("equipName");  // Ignore equipment for now
yokai[0].setHelper.specialEquip.alliance.set("Fleshy");      // Alliance: "Fleshy" or "Wicked" ALSO IGNORE THIS FOR NOW
```

## Usage Examples

### Basic Yokai Editing
```javascript
// Get all Yokai
let allYokai = getAllYokai();

// Level up first Yokai to max
allYokai[0].set("level", 99);

// Max out IVs for competitive play
allYokai[0].set("IV_Spd", 15);
allYokai[0].set("IV_Str", 15);
allYokai[0].set("IV_Spr", 15);
allYokai[0].set("IV_Def", 15);
```

### Advanced Editing with Helpers
```javascript
// Create a perfect Yokai
let perfectYokai = allYokai[0];

// Max stats
perfectYokai.setHelper.energy.HP.set(999);
perfectYokai.setHelper.energy.Soul.set(999);

// Max abilities
perfectYokai.setHelper.specialUnlock.attackLevel.set(10);
perfectYokai.setHelper.specialUnlock.techniqueLevel.set(10);
perfectYokai.setHelper.specialUnlock.soultimateLevel.set(10);

// Set personality
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

num2:
- Edited via the default set and get methods: `yokai.set("num2", "7")`
   - Can also be edited via `setRaw()`, and `getRaw()`.

youkaiId:
- Note: Also known as the yokai itself, also beware of capitalization. Can be edited via the default set method: `yokai.set("youkaiId", ID)`.
   - Can also be edited via `setRaw()`, and `getRaw()`.

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

- This API currently has bad input validation so don't put the wrong value :P
- Always call `getAllYokai()` first to load Yokai data
- Use helper methods for complex multi-byte fields
- Raw hex editing requires understanding of the save file format, so USE THE HELPERS!!!!
- CP932 encoding is needed for proper Japanese character support, there is no way around this for now.
- Changes are applied immediately to the in-memory save data so yeah :<
- Use `exportSave()` to get the final modified save file




DEV NOTES:
FIX SOME ADVANCED FIELDS
ADD REFRESH VIEW FOR USERS
ADD WAYS TO EXPOSE LOADING SAVE FILES BEFORE INIT WITHOUT USING DOM SHENANIGANS
