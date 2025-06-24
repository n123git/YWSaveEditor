# General

This documentation describes the Save API, which allows JS modification of save files using a simple yet powerful API via my save editor.

* [/yokai.md](https://github.com/n123git/YWSaveEditor/blob/main/api-docs/yokai.md) describes the API calls for the yokai editor (available through devtools console once the save is loaded)
* [/keyitem.md](https://github.com/n123git/YWSaveEditor/blob/main/api-docs/keyitem.md) describes the API calls for the key items editor  (again, available through devtools console once the save is loaded)
* coming soon describes the API calls for the general editor, again load the save file and then paste it in the devtools console available via Ctrl+Shift+I on most browsers.
LOREM IPSUM ACTUALLY WRITE THIS LATER

## Examples

Yo-kai Example #1: Fixes IVs of all Yo-kai:
```js
(function fixYokaiData() {
    const yokaiList = getAllYokai();
    console.log(`Fixing ${yokaiList.length} Yo-kai...`);

    let ivTotalTarget = 40;

    yokaiList.forEach((yokai, index) => {
        // --- Fix IVs ---
        let IV_HP = yokai.get("IV_HP");
        let IV_Str = yokai.get("IV_Str");
        let IV_Spr = yokai.get("IV_Spr");
        let IV_Def = yokai.get("IV_Def");
        let IV_Spd = yokai.get("IV_Spd");

        let IV_Sum = Math.floor(IV_HP / 2) + IV_Str + IV_Spr + IV_Def + IV_Spd;

        let needsFix = (IV_HP % 2 !== 0) || (IV_Sum !== ivTotalTarget);

        if (needsFix) {
            let ivs = [0, 0, 0, 0, 0];

            for (let i = 0; i < ivTotalTarget; ++i) {
                let r = Math.floor(Math.random() * 5);
                ivs[r]++;
            }

            yokai.set("IV_HP", ivs[0] * 2); // Even HP
            yokai.set("IV_Str", ivs[1]);
            yokai.set("IV_Spr", ivs[2]);
            yokai.set("IV_Def", ivs[3]);
            yokai.set("IV_Spd", ivs[4]);
        }
    });

    console.log("All done!");
})();
```

Key items Example #1 Shuffles Item Order
```js
// Simple Item Randomizer Example
// Swaps item IDs between all non-empty slots to randomize inventory order

function randomizeItems() {
    // Check if save is loaded
    if (!SaveAPI.isLoaded) {
        console.log("Please load a save first!");
        return;
    }
    
    // Get all non-empty items
    const items = SaveAPI.getAllItems().filter(item => item.itemId !== 0);
    
    if (items.length < 2) {
        console.log("Need at least 2 items to randomize!");
        return;
    }
    
    // Extract item IDs and shuffle them
    const itemIds = items.map(item => item.itemId);
    
    // Simple shuffle (Fisher-Yates)
    for (let i = itemIds.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [itemIds[i], itemIds[j]] = [itemIds[j], itemIds[i]];
    }
    
    // Apply shuffled IDs back to original slots (keeping quantities)
    for (let i = 0; i < items.length; i++) {
        SaveAPI.setItem(items[i].index, {
            num1: items[i].num1,    // Keep original quantity
            num2: items[i].num2,    // Keep original flags
            itemId: itemIds[i]      // Use shuffled ID
        });
    }
    
    // Update the UI
    SaveAPI.updateUI();
    
    console.log(`Randomized ${items.length} items!`);
}

console.log("Item randomizer loaded! Use: randomizeItems()");
```
