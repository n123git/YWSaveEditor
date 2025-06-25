[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-Web%20HTML5-green)](#)<br/>
[<img src="https://github.com/n123git/YWSaveEditor/blob/main/badge_2.svg"></img>]()



# Yo-kai Watch 2 Save Editor

A terrible, WIP **Save Editor for Yo-kai Watch 2**, which is made to support **desktop, mobile and the 3DS!<sup><a href="#footnote-1" style="text-decoration: none; color: inherit;">1</a></sup>** Built to be reliable yet feature-rich. Whether you're a competitive player, modder, or just a casual fan — this tool was made for you!

### Disclaimer
This tool is *severly* unfinished, I do not recommend it, find better ones [here](https://github.com/nobodyF34R/ykw-editors).</br>
This tool is provided "as is" without any warranties, express or implied. The creator of this tool is not responsible for any damage, data corruption, or data loss that may result from its use. By using this tool, you agree to assume full responsibility for any consequences and waive any claims against the creator or the tool itself for any impact it may cause.</br>
**TL;DR:** This tool *can* mess up your data. It is YOUR responsibility to back things up first and you use it at YOUR OWN risk.



## Table of Contents:
 - See the [/docs/](https://github.com/n123git/YWSaveEditor/tree/main/docs) folder for technical documentation (Don't worry, I saved you the effort of trying to go through my terrible code :D).
 - See the [/api-docs/](https://github.com/n123git/YWSaveEditor/tree/main/api-docs) folder for documentation on my API.
 - Visit [/releases/latest](https://github.com/n123git/YWSaveEditor/releases/latest) for downloads and source code.
 - Use [Issues](https://github.com/n123git/YWSaveEditor/issues) or [Pull Requests](https://github.com/n123git/YWSaveEditor/pulls) to report bugs and suggest features (or just ping @n123original on discord).

## Features

- **Game Support**: Fully compatible with Yo-kai Watch 2 save data.
-  **Cross-Platform**:  
   - **HTML Version** — runs locally in your browser, supports **Windows, macOS, Linux, Android, iOS, iPadOS and *more***.
       - If the OS can run a modern web broswer, it can run the web version.
           - The main releases currently requires ES13, although on request a patched version can be shared.
   - **3DS Version** — run directly on a homebrewed 3DS<sup><a href="#footnote-2" style="text-decoration: none; color: inherit;">2</a></sup> for on-the-go editing. Requires a modern installation of Luma. Has direct save file and Checkpoint integration via a simple, effective UI.
-  **Feature-Rich**: Includes all features from Togenyan’s editor — and more.
   - Modify Yo-kai, inventory, key items, currencies (money, JP, BP/KP, GP), team, medals, and much, much more.
   - Built-in hex editor via `Dev Mode`. (If you don't know what this is, you don't need it)
   - Illegal value detection and suggestion.
   - Config file
   - An API to programatically interact with it :>
-  **Modding Support**: Built with modularity in mind — ideal for custom Yo-kai, future expansion etc.
   - Easily customize parsing logic, field definitions, or UI components.
-  **Fast & Efficient**: Optimized for laziness, and no speed — even on 3DS hardware.
- **Open Source**: Transparent, editable, and developer-friendly.
- **Actively Maintained**: New features and fixes added regularly.

---

## Downloading

### ➤ HTML Version
- [Download the Latest Release](https://github.com/n123git/YWSaveEditor/releases/latest)
- Open the `.html` file in ANY modern web browser, whether it be Chrome, Firefox or ~~Microsoft Edge~~. This works on every up-to-date web browser (ES13+)
- No installation, shady links or internet connection* required!
<br/>
* After the download (for obvious reasons).

### ➤ 3DS Version
- [Download `.cia`](#)  <!-- I will release the 3DS version *once* i'm satisfied with the web ver -->

---

## Getting Started

1. **Backup your save file!**  
   Mistakes can corrupt saves — always keep a copy.
2. **Load your save** via the file picker (HTML) or via the save selection panel for the 3DS (Has Checkpoint-integration).
3. Use the intuitive interface or hex editor to make your changes.
4. Press **Save** and reinsert the save into your game.

---

## Q / A

**Q: How do I add support for custom Yo-kai?**  
A: Simply Copy-Paste your custom Yo-kai data into the `yokai.js` file in the `data` folder. The editor will then recognize and display your custom entries instead of leaving them blank.

**Q: Do you plan to add support to modify things such as a Yo-kai's Rank?**  
A: No. This is a save editor, not a game editor or modding tool. To moding the game, please use tools such as [Albatross](https://github.com/Tiniifan/Albatross), [CfgBin Editor](https://github.com/onepiecefreak3/CfgBinEditor/) and join the [YW Modding Zone](https://discord.gg/TbmKf6Ujq3) (Note that the afformentioned discord IS against save editing and any kind of cheating in general, do not save edit there).

**Q: Does the editor support all versions of Yo-kai Watch 2?**  
A: This editor is tested on EUR and USA v2.0, but should work for all 2.0 versions unless something goes horribly wrong, and may work for 1.0 and JP versions, albeit with limited support.

**Q: Does the editor support Yo-kai Watch 1 or any other games?**  
A: Currently, the editor is designed exclusively for Yo-kai Watch 2 saves. Support for other games may be added in the future.

**Q: Is it safe to edit my save with this save editor?**  
A: While the editor is tested and reliable, **always backup your save files** before making changes. Corruption can occur, and as previously mentioned I (and my software) are not responsible for any negative/unwelcome affects caused by using this tool. Use it at your own risk.

**Q: How do I report bugs or request features?**  
A: Open an issue on the project’s GitHub repository or contact me (@n123original on discord).

**Q: Can you help me get a Yo-kai with Infinite Stats?**
A: No. That's not possible, this is a save editor and therefore follows the restrictions of save files. For those things, see mods or cheats (Some things like level 255 Yo-kai ARE technically possible, but the game will detect it and lock it to a lower level).

**Q: What happens if I set something to an illegal value?**
A: It depends, but it could (from best to worst):
* Work perfectly fine,
* Be extremely buggy,
* Have the game detect the cheating and ignore the value,
* Overflow and mess up other things (You don't want a team of pandles with -999HP, do you?)

---

## Developer Info

- Written in **vanilla HTML/JS/CSS** (desktop) and **Lua** via **LovePotion** (3DS).
   - The `HTML` version uses no external libraries - I couldn't find any that would actually help me much anyway.
- Save structure parsing is fully modular and documented.
- External condition and ID mappings use plain JSON (imported via `.js` scripts due to annoying restrictions) for easy editing.
- Uses SectionID's to dynamically infer offsets for maximum compatibility.

### Contributing
Pull requests, suggestions, and issue reports are welcome!  
See `docs` for the save format documentation.

### Current Progress
- Save Data Decryption: 80%
- Yo-kai Editing: 97%
 - Items Editing: 90%
 - Trophy Editing: 99%
 - Misc Editing: 30%
- SectionID Management: 100%
- Save Data Encryption: 30%
---

## License

This project is licensed under the MIT License.  
You are free to use, modify, and distribute it as long as you credit the original authors (me :)).

## Credits

- [The LovePotion Team](https://lovebrew.org/) - for developing and maintaining the best 3DS game engine.
- [Nobody_F34R](https://github.com/nobodyF34R) - for his YW-ID project, general help and being a chill guy all around.
- [Togenyan](https://github.com/Togenyan) — inspiration and original editor framework.
- [Level-5](https://en.wikipedia.org/wiki/Level-5_(company)) — developers of Yo-kai Watch.
- The community — for being chill af.

## Footnotes
<a name="footnote-1"></a>1. By 3DS, I am referring to the 3DS and 2DS family of systems, including but not limited to the "New" remakes. See [this](https://www.nintendo.com/en-gb/Hardware/Nintendo-3DS-Family/Nintendo-3DS-Family-94560.html) for more information on the family of systems.<br/>
<a name="footnote-2"></a>2. A homebrewed 3DS refers to a 3DS which has had cfw (Custom Firmware) installed to remove limitations - namely signature checks, allowing for custom applications. You can mod ANY 3DS by following the instructions [here](https://3ds.hacks.guide).
