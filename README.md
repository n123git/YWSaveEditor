# Yo-kai Watch 2 Save Editor

A powerful, modern, and modular **Save Editor for Yo-kai Watch 2**, supporting **desktop, mobile and 3DS!** Built to be fast, flexible, and future-proof. Whether you're a competitive player, modder, or just a casual fan — this tool was made for you!

---

## Table of Contents:
 - See the `docs` folder for technical documentation.
 - Visit `Releases` for downloads and source code.
 - Use `Issues` or `Pull Requests` to report bugs and suggest features.

## Features

- **Game Support**: Works with Yo-kai Watch 2 save data.
-  **Cross-Platform**:  
   - **HTML Version** — runs locally in your browser, supports **Windows, macOS, Linux, Android, and iOS**.  
   - **3DS Version** — run directly on a homebrewed 3DS for on-the-go editing.
-  **Feature-Rich**: Includes all features from Togenyan’s editor — and more:
   - Modify Yo-kai, inventory, key items, money, team, medals, and more.
   - Built-in hex and table views.
   - Illegal value detection and suggestion.
-  **Modding Support**: Built with modularity in mind — ideal for custom Yo-kai, fan translations etc.
   - Easy to add or tweak parsing logic, field definitions, or UI components.
-  **Fast & Efficient**: Optimized for speed, responsiveness, and low memory use — even on 3DS hardware.
- **Open Source**: Transparent, editable, and developer-friendly.
- **Actively Maintained**: New features and fixes added regularly.

---

## Downloading

### ➤ HTML Version
- [Download ZIP](#) (includes `ANCHOREDITv2.HTML`)
- Open in any modern web browser.
- No installation, no internet required.

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
A: Simply copy-paste the custom Yo-kai data into the `yokai.js` file located in the `data` folder of the editor. This will let the editor recognize and display your custom entries instead of leaving it blank.

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

---

## Dev Info

- Written in **vanilla HTML/JS/CSS** (desktop) and **Lua** via **LovePotion** (3DS).
- Save structure parsing is fully modular and documented.
- External condition and ID mappings use plain JSON for easy editing.

### Contributing
Pull requests, suggestions, and issue reports are welcome!  
See `docs/STRUCTURE.md` for the save format documentation.

### Current Progress
- Save Data Decryption: 80%
- Yo-kai Editing: 95%
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
