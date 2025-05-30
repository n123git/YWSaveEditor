[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-3DS%20Lua%20%7C%20Web%20HTML5-green)](#)



# Yo-kai Watch 2 Save Editor

A powerful, modern, and modular **Save Editor for Yo-kai Watch 2**, supporting **desktop, mobile and 3DS!** Built to be fast, flexible, and future-proof. Whether you're a competitive player, modder, or just a casual fan — this tool was made for you!

### Disclaimer
This tool is severly unfinished, I currently do not recommend it, find better ones [here](https://github.com/nobodyF34R/ykw-editors).
This tool is provided "as is" without any warranties, express or implied. The creator of this tool is not responsible for any damage, data corruption, or data loss that may result from its use. By using this tool, you agree to assume full responsibility for any consequences and waive any claims against the creator or the tool itself for any impact it may cause.
**TL;DR:** This tool *can* mess up your data. It is YOUR responsibility to back things up first and you use it at YOUR OWN risk.

---

## Table of Contents:
 - See the `docs` folder for technical documentation (Don't worry, I saved you the effort of trying to go through my terrible code :D).
 - Visit `Releases` for downloads and source code.
 - Use `Issues` or `Pull Requests` to report bugs and suggest features (or just ping @n123original on discord).

## Features

- **Game Support**: Fully compatible with Yo-kai Watch 2 save data.
-  **Cross-Platform**:  
   - **HTML Version** — runs locally in your browser, supports **Windows, macOS, Linux, Android, iOS, iPadOS and *more***.
       - If it can run a web broswer atleast a decade old (ES6), it can run the save editor :D  
   - **3DS Version** — run directly on a homebrewed 3DS for on-the-go editing.
-  **Feature-Rich**: Includes all features from Togenyan’s editor — and more.
   - Modify Yo-kai, inventory, key items, money, team, medals, and more.
   - Built-in hex editor via `Dev Mode`. (If you don't know what this is, you don't need it)
   - Illegal value detection and suggestion.
-  **Modding Support**: Built with modularity in mind — ideal for custom Yo-kai, fan translations etc.
   - Easily customize parsing logic, field definitions, or UI components.
-  **Fast & Efficient**: Optimized for speed, responsiveness, and low memory use — even on 3DS hardware.
- **Open Source**: Transparent, editable, and developer-friendly.
- **Actively Maintained**: New features and fixes added regularly.

---

## Downloading

### ➤ HTML Version
- [Download ZIP](#) (includes `ANCHOREDITv2.HTML`)
- Open the `.html` file in ANY modern web browser, whether it be Chrome, Firefox or ~~Microsoft Edge~~.
- No installation, shady links or internet connection required!

### ➤ 3DS Version
- [Download `.cia`](#)

---

## Getting Started

1. **Backup your save file!**  
   Mistakes can corrupt saves — always keep a copy.
2. **Load your save** via the file picker (HTML) or automatically (3DS).
3. Use the intuitive interface or hex editor to make your changes.
4. Press **Save** and reinsert the save into your game.

---

## ❓ Q / A

**Q: How do I add support for custom Yo-kai?**  
A: Simply Copy-Paste your custom Yo-kai data into the `yokai.js` file in the `data` folder. The editor will then recognize and display your custom entries instead of leaving them blank.

**Q: Can this editor change the rank of a Yo-kai?**  
A: No. This is a save editor, not a game editor or modder. For modding the game, please use other tools such as [Albatross](https://github.com/Tiniifan/Albatross).

**Q: Does the editor support all versions of Yo-kai Watch 2?**  
A: This editor is tested on EUR and USA v2.0, but will work for all 2.0 versions, and may work for 1.0 and JP versions, albeit with limited support.

**Q: Does the editor support Yo-kai Watch 1 or other games?**  
A: Currently, the editor is only designed for Yo-kai Watch 2 saves. Support for other versions may be added in the future.

**Q: Is it safe to edit my save with this editor?**  
A: While the editor is tested and reliable, always **backup your save files** before making changes. Corruption can occur if invalid data is introduced.

**Q: How do I report bugs or request features?**  
A: Open an issue on the project’s GitHub repository or contact the maintainers via the provided communication channels.

**Q: Can you help me get a Yo-kai with Infinite Stats?**
A: No. That would require a mod, the best I can do is the maximum *possible* stats.

**Q: What happens if I set something to an illegal value?**
A: It depends, but it could (from best to worst):
* Work perfectly fine,
* Be extremely buggy,
* Have the game detect the cheating and ignore the value,
* Overflow and mess up other things (You don't want a team of pandles with -999HP, do you?)

---

## Dev Info

- Written in **vanilla HTML/JS/CSS** (desktop) and **Lua** via **LovePotion** (3DS).
   - The `HTML` version uses no external libraries - I couldn't find any that'd actually help that much anyway.
- Save structure parsing is fully modular and documented.
- External condition and ID mappings use plain JSON for easy editing.

### Contributing
Pull requests, suggestions, and issue reports are welcome!  
See `docs/STRUCTURE.md` for the save format documentation.

### Current Progress
- Save Data Decryption: 80%
- Yo-kai Editing: 97%
- Save Data Encryption: 30%
---

## License

This project is licensed under the MIT License.  
You are free to use, modify, and distribute it as long as you credit the original authors.

---

## Credits

- The LovePotion Team - for the beautiful 3DS Game Engine
- Nobody_F34R - for his YW-ID project, general help and being a chill guy all around.
- Togenyan — inspiration and original editor framework.
- Level-5 — developers of Yo-kai Watch.
- The community — for reverse engineering efforts and feedback.
