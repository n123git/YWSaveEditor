# MiscApi Documentation

A JS API for managing my save editor programmatically for the misc/general portion (wow fancy words I totally didnt look up like a year ago :>).

## Quick Start

```javascript
const api = new MiscApi();
console.log(api.listFields()); // See all the available fields (WOW)
```

## Methods

### Core Data Methods

#### `get(fieldName)`
Retrieves a formatted value from the save file.

**Parameters:**
- `fieldName` (string): Name of the field to retrieve

**Returns:**
- For bit arrays (chest, weatherevent, trophy): Array of 0s and 1s
- For other fields: Formatted value (string, number, etc.)
- If the save file has been loaded but the processing failed i.e. the save data is just `0A`, this returns `Uncaught TypeError: node is null`

**Examples:**
```javascript
api.get('money');           // Returns: 50000
api.get('location');        // Returns: "Uptown Springdale"
api.get('weatherevent');    // Returns: [1, 0, 1, 0, 0, 0, 0, 0]
api.get('chest');           // Returns: [0, 1, 1, 0, ...] (144 elements)
```

#### `set(fieldName, value)`
Sets a formatted value in the save file.

**Parameters:**
- `fieldName` (string): Name of the field to set
- `value`: Value to set (type depends on field format)

**Returns:** `true` on success

**Examples:**
```javascript
// Numbers
api.set('money', 999999); // the max aka 999999yen/$9999.99/£9999.99/€9999.99

// Strings (enum values)
api.set('location', 'Downtown Springdale');
api.set('ficon', 'Pandle');  // Uses in-game name

// Bit arrays (0s and 1s)
api.set('weatherevent', [1, 0, 1, 0, 0, 0, 0, 0]);
api.set('chest', [0, 1, 1, 0, ...]);  // Can be shorter than full length but it causes the unspecified ones to be set to 0 (unchecked)

// Partial arrays (will warn but will work setting the unspecified ones to 0 aka unchecked)
api.set('weatherevent', [1, 0, 1]);  // Warns: only 3/8 elements provided
```

#### `getRaw(fieldName)`
Gets the raw hex bytes for a field.

**Parameters:**
- `fieldName` (string): Name of the field

**Returns:** Hex string (uppercase)

**Example:**
```javascript
api.getRaw('money');  // Returns: "7f96"
```

#### `setRaw(fieldName, hexValue)`
Sets raw hex bytes for a field.

**Parameters:**
- `fieldName` (string): Name of the field
- `hexValue` (string): Hex string (case insensitive, spaces/formatting ignored)

**Returns:** `true` on success

**Example:**
```javascript
api.setRaw('money', 'ff ff');  // Sets money to 65535
```

### Information Methods

#### `listFields()`
Lists all available fields in the save file.

**Returns:** Array of field objects with properties:
- `name` (string): Field name
- `format` (string): Data format
- `size` (number): Size in bytes
- `description` (string): Field description

**Example:**
```javascript
api.listFields();
// Returns: [
 {
    "name": "Location",
    "format": "location",
    "size": 4,
    "description": "Current location in the game"
  },
  {
    "name": "XPos",
    "format": "float32",
    "size": 4,
    "description": "Current X Coordinate in the game"
  },
  {
    "name": "YPos",
    "format": "float32",
    "size": 4,
    "description": "Current Y Coordinate in the game"
  },
  {
    "name": "ZPos",
    "format": "float32",
    "size": 4,
    "description": "Current Z Coordinate in the game"
  },  ... ]
```


#### `getAvailableValues(fieldName)`
Gets available values for enum-type fields.

**Parameters:**
- `fieldName` (string): Name of the field

**Returns:** Array of available values, or `null` for non-enum fields

**Examples:**
```javascript
api.getAvailableValues('Location');     // Returns: ["Springdale Hot Springs Lobby", "Old Springdale", "Cicada Canyon", ...]
api.getAvailableValues('Friends App - Icon');        // Returns: ["Not set.", "Grumples", "Eyeclone", "Chirpster", "Kelpacabana", ...]
api.getAvailableValues('Weather Events Active'); // Returns: ["<unknown>", "<unknown>", "Yo-kai Advisory", "Yo-kai Warning", "Tripping Advisory", "Bugs: Swarming!", "Fish: Good fishing!", "<unknown>"]
api.getAvailableValues('Money');        // Returns: null (not an enum)
api.getAvailableValues('thisdosentexist'); // Throws the error: Uncaught Error: Field 'thisdosentexist' not found. Available fields: Location, ...
```

#### `getSaveHex()`
Exports the entire save file as a hex string.

**Returns:** Uppercase hex string of the complete save file

**Example:**
```javascript
api.getSaveHex();  // Returns: "4A6F686E00000000FF7F..."
```

#### `getVersion() `
Returns the current API version as a number (NOT the save editer version. I considered making this a BigInt or even a Symbol to troll)

**Returns:** Number corresponding to API Version

**Example:**
```javascript
api.getVersion() // Returns: [Version]

function code() {
if(api.getVersion() !== 1.0) {
  throw Error("Plugin/extension/whateveryoucallthis is out of date :<<")
}

// do stuff here like actual code
}
code()
```

#### `syncSave(hex = getSaveHex())`
Changes the save file to match the input - by default the normal save fetched from the API (wow the API uses the API)

**Returns:** `undefined`

**Example:**
```javascript
d = api.getSaveHex()
d = doStuff(d); // im lazy ok
api.syncSave(d)
```

**Notes:**
* The input is hex, whitespace is ignored, and it is case-insensitive. It automatically strips whitespace, adds a space every byte, and capitises letters
   * TL;DR requirements are lax, it internally rearranges it to my preferences :>>>>

## Data Types

### Numbers
- `uint8`: 0-255
- `uint24`: 0-16,777,215  
- `uint32`: 0-4,294,967,295
- `float32`: 32-bit floating point number

### Enums
String values from predefined lists:
- `location`: Game locations
- `wallpaper`: Available wallpapers
- `bicycle`: Bicycle types
- `bell`: Bell sounds
- `ficon`: Friend icons (uses in-game names)
- `hq`: Headquarters
- `job`: Jobs
- `hobby`: Hobbies
- `ambit`: Ambitions

### Bit Arrays
Arrays of 0s and 1s representing boolean flags:
- `chest`: 144 elements (treasure chests collected)
- `weatherevent`: 8 elements (weather event flags)
- `trophy`: 96 elements (trophies collected)

### Raw Data
- `hex`: Raw hex string data

## Error Handling

The API throws descriptive errors for common issues:

```javascript
try {
    api.set('invalidField', 'value');
} catch (error) {
    console.error(error.message); // "Field 'invalidField' not found..."
}

try {
    api.set('ficon', 'Invalid Icon');
} catch (error) {
    console.error(error.message); // "Friends icon 'Invalid Icon' not found..."
}
```

## Bit Array Behavior

For bit array fields (`chest`, `weatherevent`, `trophy`):

### Getting Values
```javascript
api.get('weatherevent');  // Returns: [1, 0, 1, 0, 0, 0, 0, 0]
```

### Setting Values
```javascript
// Full array
api.set('weatherevent', [1, 0, 1, 0, 0, 0, 0, 0]);

// Partial array (warns but works)
api.set('weatherevent', [1, 0, 1]);  // Missing elements become 0
// Console: "Weather event array has 3 elements, expected 8. Missing elements will be set to 0."

// Boolean values also work
api.set('weatherevent', [true, false, true, false]);
```

## Field Format Reference

| Format | Description | Example Input | Example Output |
|--------|-------------|---------------|----------------|
| `uint8` | 8-bit integer | `255` | `255` |
| `uint24` | 24-bit integer | `1000000` | `1000000` |
| `uint32` | 32-bit integer | `4000000` | `4000000` |
| `float32` | 32-bit float | `123.45` | `123.45` |
| `location` | Location name | `"Uptown Springdale"` | `"Uptown Springdale"` |
| `ficon` | Friend icon name | `"Pandle"` | `"Pandle"` |
| `weatherevent` | Weather flags | `[1,0,1,0,0,0,0,0]` | `[1,0,1,0,0,0,0,0]` |
| `chest` | Chest collection | `[0,1,1,0,...]` | `[0,1,1,0,...]` |
| `trophy` | Trophy collection | `[1,1,0,1,...]` | `[1,1,0,1,...]` |
| `hex` | Raw hex data | `"deadbeef"` | `"deadbeef"` | <!-- for those who know :> -->

## Integration

The API automatically updates the UI when values change:
- Calls `updateOutput()` to refresh the save file
- Calls `renderFields()` to update the display (unless in dev mode)
