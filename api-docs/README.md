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
