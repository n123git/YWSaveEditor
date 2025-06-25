# Yo-kai Save Editor API Documentation

A (hopefully) complete JS API for interacting with my save editor's Key Items branch.

* How do you use the API?
* Follow this doc (XD), and paste the code into the JS console once your save file is loaded :>
* Unlike the Yo-kai editor, this is *not* fully class based.

## (Incomplete) Table of Contents
- [Quick Start](#quick-start)
- [API Reference](#api-reference)
- [Data Structures](#data-structures)
- [Examples](#examples)
- [Error Handling](#error-handling)
- [Remember These](#remember-these)

## Quick Start

```javascript
// 1. (Optional) Load your save data (automatically done when clicking "Analyze", so unless you want to load a custom save or manually edit some bytes, this isnt needed)
SaveAPI.loadSave(hexString);

// 2. Get all items
let items = SaveAPI.getAllItems();

// 3. Modify an item
SaveAPI.setItem(0, { num1: 99, itemId: 12345 });

// 4. Export modified save
let modifiedSave = SaveAPI.exportSave();
```

Note: this doc uses `SaveAPI`, `SaveAPI` is an instance of `SaveEditorAPI` that is automatically synced to the current save file via a hook in the analyze button. You can use other instances if you want, but that will over complicate things :D
## API Reference

### Core Methods (Yay)

#### `SaveAPI.loadSave(hexData)`
Loads save data from a hex string. Kinda self-explanatory.

**Parameters:**
- `hexData` (string): The hex string of the save file (spaces and newlines allowed)

**Returns:**
- `boolean`: `true` if successful, `false` if it failed (note:fact check false if failed, I dont remember if this is true).

**Example:**
```javascript
let success = SaveAPI.loadSave("FEFF01AB0A7C48656C6C6F20576F726C64...");
if (success) {
    console.log("Save loaded successfully!");
}
```

#### `SaveAPI.getAllItems()`
Retrieves all items from the loaded save data.

**Returns:**
- `Array<ItemObject>`: Array of all item objects

**Example:**
```javascript
let items = SaveAPI.getAllItems();
console.log(`Found ${items.length} item slots, very coolz`);
```

#### `SaveAPI.getItem(index)`
Gets a specific item by its slot index.

**Parameters:**
- `index` (number): Item slot index (0-indexed like an array)

**Returns:**
- `ItemObject|null`: Item object or null if not found (oh noes).

**Example:**
```javascript
let firstItem = SaveAPI.getItem(0); // gets the first item
if (firstItem) {
    console.log(`Item: ${firstItem.name} (ID: ${firstItem.itemId})`);
}
```

#### `SaveAPI.setItem(index, itemData)`
Modifies an item at a specific index.

**Parameters:**
- `index` (number): Item slot index, again 0-indexed
- `itemData` (object): Object containing properties to update

**Returns:**
- `boolean`: `true` if successful, `false` if failed (again, check false, dosen't my code throw??? i dont remember)

**Example:**
```javascript
// Update num1 and item ID
SaveAPI.setItem(0, { num1: 99, itemId: 12345 });

// Only update num1
SaveAPI.setItem(0, { num1: 50 });
```

#### `SaveAPI.findItems(search)`
Searches for items by name or ID.

**Parameters:**
- `search` (string|number): Item name (partial match) or exact item ID

**Returns:**
- `Array<ItemObject>`: Array of matching items

**Example:**
```javascript
// Find by name
let watches = SaveAPI.findItems("Watch");

// Find by ID
let specificItems = SaveAPI.findItems(1332445335);
```

#### `SaveAPI.addOrUpdateItem(itemId, data)`
Adds a new item or updates an existing one. Automatically finds the best slot. (The long name is intentional :>>>)

**Parameters:**
- `itemId` (number): The item ID to add/update
- `data` (object): Additional data (num1, num2) - defaults to `{num1: 1, num2: 0}`

**Returns:**
- `number|null`: Index where item was placed, or null if failed

**Example:**
```javascript
// Add new item
let index = SaveAPI.addOrUpdateItem(1332445335);

// Add with specific data
let index2 = SaveAPI.addOrUpdateItem(1332445335, { num1: 5, num2: 1 });
```

#### `SaveAPI.removeItem(index)`
Removes an item by setting it to empty (all zeros).

**Parameters:**
- `index` (number): Item slot index to clear

**Returns:**
- `boolean`: `true` if successful, `false` if failed

**Example:**
```javascript
SaveAPI.removeItem(0); // Clear first slot
```

#### `SaveAPI.exportSave()`
Exports the current save data as a hex string.

**Returns:**
- `string`: Hex string representation of the modified save data

**Example:**
```javascript
let modifiedSave = SaveAPI.exportSave();
// do something with it
```

#### `SaveAPI.getStats()`
Provides statistics about the current save data.

**Returns:**
- `StatsObject`: Object containing save statistics

**Example:**
```javascript
let stats = SaveAPI.getStats();
console.log(`Used: ${stats.usedSlots}/${stats.totalSlots} slots`);
console.log(`Unique items: ${stats.uniqueItems}`);
```

### Utility Methods

#### `SaveAPI.updateUI()`
Edits the save editor's save to match the API's save

**Example:**
```javascript
SaveAPI.setItem(0, { num1: 99 });
SaveAPI.updateUI(); // Updates the hex display
```

### Convenience Functions

Global shortcut functions are available for common operations:

- `loadSave(hexData)` → `SaveAPI.loadSave(hexData)`
- `getAllItems()` → `SaveAPI.getAllItems()`
- `getItem(index)` → `SaveAPI.getItem(index)`
- `setItem(index, data)` → `SaveAPI.setItem(index, data)`
- `findItems(search)` → `SaveAPI.findItems(search)`
- `addItem(itemId, data)` → `SaveAPI.addOrUpdateItem(itemId, data)`
- `removeItem(index)` → `SaveAPI.removeItem(index)`
- `exportSave()` → `SaveAPI.exportSave()`
- `getSaveStats()` → `SaveAPI.getStats()`

## Data Structures

### ItemObject
```javascript
{
    index: 0,           // Slot index (0-based)
    num1: 1,           // First number
    num2: 0,           // Second number
    itemId: 12345,     // Item ID
    name: "Yo-kai Watch", // Human-readable name
    offset: 14056      // Byte offset in save file
}
```

### StatsObject
```javascript
{
    totalSlots: 100,        // Total available slots
    usedSlots: 45,          // Slots with items
    emptySlots: 55,         // Empty slots
    uniqueItems: 30,        // Number of unique item types
    items: [                // Array of non-empty items
        {
            name: "Yo-kai Watch",
            id: 12345,
            count: 1
        }
        // ... more items
    ]
}
```

## Examples!!!!!!

### Basic Item Management
```javascript
// Load and inspect save
SaveAPI.loadSave(hexString);
let stats = SaveAPI.getStats();
console.log(`Save has ${stats.usedSlots} items`);

// Find all watches
let watches = SaveAPI.findItems("Watch");
watches.forEach(watch => {
    console.log(`${watch.name} at slot ${watch.index}`);
});

// Give player 99 of first item
if (stats.usedSlots > 0) {
    SaveAPI.setItem(0, { num1: 99 });
}
```

### Batch Operations
```javascript
// Give player multiple items
const itemsToAdd = [
    { id: 12345, num1: 10 },
    { id: 12346, num1: 5 },
    { id: 12347, num1: 1 }
];

itemsToAdd.forEach(item => {
    SaveAPI.addOrUpdateItem(item.id, { num1: item.num1 });
});

console.log("Added", itemsToAdd.length, "items");
```

### Save Backup and Restore
```javascript
// Create backup
let backup = SaveAPI.exportSave();

// Make changes
SaveAPI.setItem(0, { num1: 999 });
SaveAPI.addOrUpdateItem(99999, { num1: 1 });

// Restore if needed
SaveAPI.loadSave(backup);
```

### Inventory Analysis
```javascript
let items = SaveAPI.getAllItems();
let nonEmpty = items.filter(item => item.itemId !== 0);

// Group by item type
let itemCounts = {};
nonEmpty.forEach(item => {
    if (itemCounts[item.name]) {
        itemCounts[item.name] += item.num1;
    } else {
        itemCounts[item.name] = item.num1;
    }
});

console.log("Inventory summary:", itemCounts);
```

## Error Handling

The API includes comprehensive error handling:

```javascript
try {
    if (!SaveAPI.loadSave(hexData)) {
        console.error("Failed to load save data");
        return;
    }
    
    let item = SaveAPI.getItem(999); // Invalid index
    if (!item) {
        console.log("Item not found");
    }
    
    let success = SaveAPI.setItem(0, { num1: "invalid" });
    if (!success) {
        console.error("Failed to set item");
    }
    
} catch (error) {
    console.error("API Error:", error.message);
}
```

### Common Errors
- **Save not loaded**: Most methods will throw "Save not loaded. Call loadSave() first, if youre using the default instance (`SaveAPI`), then click Analyze on the UI."
- **Invalid indices**: Methods return `null` or `false` for out-of-range indices
- **Invalid data**: Numeric values are clamped to valid ranges
- **Malformed hex**: `loadSave()` returns `false` for invalid hex data :<>>><<>><

## Remember These

### 1. Always Check Load Status
```javascript
if (!SaveAPI.loadSave(hexData)) {
    alert("Failed to load save file!");
    return;
}
```

### 2. Validate Indices
```javascript
let item = SaveAPI.getItem(index);
if (!item) {
    console.log("Invalid item index");
    return;
}
```

### 3. Use Stats for Validation
```javascript
let stats = SaveAPI.getStats();
if (stats.usedSlots >= stats.totalSlots) {
    console.log("Inventory full!");
}
```

### 4. Backup Before Major Changes
```javascript
let backup = SaveAPI.exportSave();
// Make changes...
// If something goes wrong: SaveAPI.loadSave(backup);
```

### 5. Update UI After Changes
```javascript
SaveAPI.setItem(0, { num1: 99 });
SaveAPI.updateUI(); // Refresh the hex display
```

### 6. Use Batch Operations Efficiently
```javascript
// Good: Multiple changes then one export
SaveAPI.setItem(0, { num1: 99 });
SaveAPI.setItem(1, { num1: 50 });
SaveAPI.setItem(2, { num1: 25 });
let result = SaveAPI.exportSave();

// Avoid: Exporting after each change
```

### 7. Handle Weird Cases
```javascript
// Check for empty slots before adding
let stats = SaveAPI.getStats();
if (stats.emptySlots === 0) {
    console.log("No space for new items");
} else {
    SaveAPI.addOrUpdateItem(newItemId);
}
```

## Integration Notes

- The API automatically integrates with the UI
- Clicking "Analyze" automatically loads data into the API
- The API respects the same data format and constraints as the original editor
- All changes are immediately reflected in the internal buffer
- Use `updateUI()` to sync changes back to the textarea display

## Compatibility Notes (lol)

This API is designed to work with the Key Items Editor and therefore requires:
- The HTML page for the editor
- The existing JavaScript dependencies in the editor's data folder (gameconfig.js, important.js, etc.)
- Modern browser with ES13 support
- Save Data
