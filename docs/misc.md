
 **Note:** Please review [general.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/general.md) before this.

# Table of Contents

* [Section 0x01 — Player Position, Appearance, Uses etc](#section-0x01--player-position-appearance-medallium-data-uses-etc)
* [Section 0x09 — Currency and Points (misc9)](#section-0x09--currency-and-points-misc9)

---

## Key Section IDs

- Section `0x01` — Player Position, Appearance, Medallium Data, Uses etc

| Field   | Notes                        | Section ID | Offset  |
| ------- | ---------------------------- | ---------- | ------- |
| float32 | X Coordinate                 | `0x01`     | `0x04`  |
| float32 | Y Coordinate                 | `0x01`     | `0x08`  |
| float32 | Z Coordinate                 | `0x01`     | `0x0C`  |
| uint32  | Location                     | `0x01`     | `0x18`  |
| uint32  | Equipped Wallpaper           | `0x01`     | `0x27`  |
| uint8   | Crank-a-kai Cranks Remaining | `0x01`     | `0x170` |
| uint8   | Bicycle Skin Equipped        | `0x01`     | `0x186` |
| uint8   | Bicycle Bell Equipped        | `0x01`     | `0x187` |

---

- Section `0x09` — Currency and Points (`misc9`)

| Field  | Notes                                 | Section ID | Offset |
| ------ | ------------------------------------- | ---------- | ------ |
| uint32 | Money (100 = 100 yen / \$1 / £1 / €1) | `0x09`     | `0x34` |
| uint32 | JP (Jungle Points)                    | `0x09`     | `0x38` |
| uint32 | GP (Gym Points)                       | `0x09`     | `0x3C` |

---


