# General

This documentation describes the Save API, which allows programmatic modification of save files using my save editor.

* [/yokai.md](https://github.com/n123git/YWSaveEditor/blob/main/api-docs/yokai.md) describes the API calls for the yokai editor (available through devtools console once the save is loaded)

LOREM IPSUM ACTUALLY WRITE THIS LATER

## Examples

Yo-kai Example #1: Auto numbers and fixes IVs of all Yo-kai:
```js
(function fixYokaiData() {
    const yokaiList = getAllYokai();
    console.log(`Fixing ${yokaiList.length} Yo-kai...`);

    let ivTotalTarget = 40;

    yokaiList.forEach((yokai, index) => {
        // --- Fix num1 and num2 ---
        let value = (index +1)* 2;
        yokai.set("num1", index);
        yokai.set("num2", value);

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
