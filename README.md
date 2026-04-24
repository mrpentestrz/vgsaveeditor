# Vanguard Galaxy Save Terminal

A standalone, offline, single-file save editor for **Vanguard Galaxy** (Steam, Early Access). Drop your `.save` file onto an HTML page and edit credits, faction reputation, ship hull, equipment stats, refined materials, mission objectives, and more. No installation, no server, no internet connection required after download.

> ⚠️ **Unofficial fan project.** Not affiliated with the developers of Vanguard Galaxy. Always back up your save before editing.

---

## Screenshots

> *Add screenshots here once you have them. Suggested shots: the boot splash, the Overview tab with a loaded save, the Faction reputation list, the Galaxy map stats, the Equipment tab.*

---

## Features

### Core
- **Single HTML file**, ~120 KB. Double-click to open in any modern browser.
- **100% offline** — gzip handling (pako library) is inlined, no CDN, no external fonts, no telemetry.
- **CRT terminal aesthetic** — scanlines, drifting starfield, amber-on-black readouts, boot sequence.
- **Live diff tracking** — every change increments the modified-fields counter; a single click resets to the original.
- **Two export modes** — repackaged `.save` (gzipped, drop-in ready) or pretty-printed `.json` for inspection.
- **Keyboard shortcut** — `Ctrl/Cmd + S` exports the edited save.
- **Mobile-friendly** — responsive layout works on phones too.

### 11 Editor Tabs

| # | Tab | What you can edit |
|---|-----|-------------------|
| 01 | **Overview** | Credits, level, XP, bonus skill points, ship name, elapsed time, all 3 ranks, inventory & cargo capacity. Includes 9 quick-action buttons (see below). |
| 02 | **Commander** | First/last name, callsign, gender, icon, level, XP, bonus skill points, title, title color. Plus all 8 skill trees with per-node point allocation, mastery level, mastery XP, locked state. |
| 03 | **Ship** | Ship name, class, faction, unit rank, hull/armor/shield (current + max), damage taken, travel speed. Combat flags: always hostile, always friendly, no rep loss, can be tracked, can flee. Crew: each crew member's level, XP, rarity. |
| 04 | **Equipment** | Direct hull/armor/shield stats. Every installed module (Reactor, Engine, Scanner, Tractor Beam, Hull Kit) and every hardpoint (turrets/weapons) with editable name, item level, rarity, base cost, favourite flag, individual stats, and dynamic fields (range, capacityCost, etc.). Boosters list with add/remove. |
| 05 | **Factions** | All 122 faction-pair reputation entries with live reputation bars (cyan = positive, red = negative). Filter by name, "Player only" toggle, sort by rep. |
| 06 | **Cargo** | Cargo capacity, global inventory capacity. Visual list of all cargo items and global inventory items color-coded by rarity. **Click any item to clone it** (creates a fresh x1 stack). |
| 07 | **Materials** | The 8 refined material slots in the global refinery (Titanium, Tungsten, Carbon, Platinum, Silicon, Iron, Cobalt, Nickel — best-guess labels). Plus a per-item count editor for everything in cargo. |
| 08 | **Galaxy** | Map exploration stats (sectors, systems, POIs visited, jumpgates open/closed). Autopilot settings (run missions, auto-sell, gameplay type, target layer, etc.). Sortable list of all systems with per-system POI exploration progress. |
| 09 | **Missions** | Active missions with editable per-objective progress (`currentAmount` vs `requiredAmount`). Archived mission ID list. |
| 10 | **Progress** | All progression unlock flags (map, autoplay, fast-lane travel, Salwor trade-in, skilltree tier 2, warp fuel, emergency jump, Umbral Transponder, etc.). All ranks and rank caps. Game register counters and flags. |
| 11 | **Raw JSON** | Full pretty-printed JSON editor with validation. For anything the UI doesn't expose. |

### Quick Actions

One-click bulk operations across the editor:

**Overview**
- `+ 1M CREDITS`
- `LEVEL → 60`
- `+ 50 SKILL PTS`
- `ALL FACTIONS → +30000` / `→ -30000` / `→ 0`
- `FULL REPAIR SHIP`
- `UNLOCK ALL FLAGS`
- `RESET ALL EDITS`

**Equipment**
- `DOUBLE STATS` / `10× STATS`
- `ALL → LEGENDARY`
- `FAVOURITE ALL`
- `+ 2 ASPECT SLOTS`
- `100K HULL/ARMOR/SHIELD`

**Cargo**
- `CLONE INSTALLED EQUIPMENT TO CARGO`
- `FAVOURITE ALL ITEMS`

**Materials**
- `ALL → 1,000` / `10,000` / `99,999` / `0`

**Galaxy**
- `REVEAL ALL POIs`
- `OPEN ALL JUMPGATES`
- `MARK ALL SYSTEMS VISITED`

**Missions**
- `AUTO-COMPLETE OBJECTIVES`

---

## How it works

Vanguard Galaxy save files are gzip-compressed JSON. The editor:

1. Detects gzip magic bytes (`1f 8b`) and decompresses with [pako](https://github.com/nodeca/pako) (MIT-licensed, inlined).
2. Parses the JSON into a live in-memory model.
3. Renders editable fields against the model. Every change mutates the model directly.
4. On export, re-serializes the model (minified, like the original) and re-gzips it.

Round-tripping a vanilla save through the editor produces a re-gzipped file that's typically *smaller* than the original (different compression levels) but functionally identical.

---

## Installation

1. Go to [Releases](../../releases) (or just download `vanguard_save_editor.html` from this repo).
2. Save the HTML file anywhere — Desktop, Downloads, wherever.
3. Double-click to open in your default browser. That's it.

There's no installer because there's nothing to install. The whole thing is one HTML file.

---

## Usage

### Locating your save file

Save files are typically here:

| OS | Path |
|----|------|
| Windows | `%USERPROFILE%\AppData\LocalLow\Maverick Games\Vanguard Galaxy\` |
| Linux (Proton) | `~/.steam/steam/steamapps/compatdata/3471800/pfx/drive_c/users/steamuser/AppData/LocalLow/Maverick Games/Vanguard Galaxy/` |
| Steam Cloud | `<Steam>/userdata/<your-id>/3471800/remote/` |

The file you want is named something like `<character>.save`. If you're not sure where it is, in Windows run:
```
dir /s /b "%USERPROFILE%\AppData\*vanguard*"
```

### Editing

1. **Back up your save first.** Copy `your_save.save` → `your_save.save.bak`.
2. Open `vanguard_save_editor.html` in your browser.
3. Drag the `.save` file onto the drop zone (or click to browse).
4. Edit whatever you want across the 11 tabs.
5. Click `EXPORT EDITED SAVE` (or press `Ctrl/Cmd+S`).
6. The browser downloads `<original>.edited.save`.
7. Rename to replace your original (after backing up!) and load the game.

### Tips

- Quit the game before editing the save. The game writes back on exit and will overwrite your changes.
- If you want to move materials between stations, dump cargo first and use the Materials tab — refinery storage is global.
- The credits field is stored as a **string** in the save, not a number. The editor preserves that.
- Mass faction edits (`ALL FACTIONS → +30000`) affect all 122 faction-pair entries, including non-player ones. Usually fine, but if you want surgical changes, use the Factions tab and filter by "Player only."
- The Cargo tab lets you click any item to clone it. Combined with the Equipment tab's stat editor, the workflow for "modded" gear is: acquire base item → buff stats → set rarity Legendary → clone several times.

---

## What this tool can NOT do

This is a **save editor**, not a game mod. It can only change values that already exist in your save file.

- ❌ **Cannot add new ship classes** that the game doesn't already have. To unlock more hulls in shipyards, use the [VG All Ships For Gold](https://www.nexusmods.com/vanguardgalaxy/mods/7) BepInEx mod.
- ❌ **Cannot spawn entirely new item types** that the game's item registry doesn't know about. You can clone existing items and rename/buff them, but the game's internal item factory is what creates novel items.
- ❌ **Cannot rebalance the game globally** — no enemy buffs, no global drop-rate changes, no economy rebalancing. Those need C# code injection via BepInEx + HarmonyX.

For real game modification (adding ships, changing combat formulas, new UI elements), see [Nexus Mods for Vanguard Galaxy](https://www.nexusmods.com/games/vanguardgalaxy/mods).

---

## Compatibility

Tested against **Vanguard Galaxy v0.8.0.13** (Early Access, 2026). The editor reads `data.Version` from the save and shows it in the status bar.

Future game updates may add or rename fields. The Raw JSON tab is the safety net — anything the UI doesn't expose can still be edited directly. If a field's name changes and the UI breaks, the underlying data is still preserved through round-trip.

The editor is built defensively:
- Null entries in arrays are filtered out (some saves contain them — we hit this in cargo and global inventory).
- Missing optional fields don't crash the renderer.
- Render errors are caught per-tab and toasted, not fatal.
- All faction/crew/skill-tree iteration handles malformed entries gracefully.

---

## Browser support

Anything modern. No specific minimums but used:
- `FileReader` + `Blob` + `File` API
- ES2017+ (`async/await`, optional chaining)
- CSS Grid + CSS variables

Verified working in Chromium, Firefox, Safari. Works fine over `file://`.

---

## Development

There's no build step. The whole thing is one HTML file with inlined CSS and JS. Edit `vanguard_save_editor.html` directly.

If you want to run automated tests against a real save, the project has been tested using `jsdom` to simulate the full user flow:

```bash
npm install jsdom
node test.js   # write your own — see history for examples
```

The end-to-end test loads the save through the file input event, clicks each tab, fires every quick action, and verifies the JS produces no console errors.

### Project structure

```
vanguard_save_editor.html   # The entire editor (~120 KB, includes pako)
README.md                   # This file
LICENSE                     # MIT
```

That's it. One file. Resist the urge to add a build pipeline.

---

## Contributing

Pull requests welcome. Some ideas:

- More accurate refined material slot labels (the order is currently a best-guess).
- Better guesses for the `MATERIAL_LABELS` constant if the game canonicalizes them somewhere.
- Aspect editor (the `aspectSlots` array contains structured aspect objects we don't fully expose).
- Multi-ship support if/when the player owns more than one ship.
- Item template database for safer item spawning (would require reverse-engineering the game's item factory).
- i18n.

Please don't add external runtime dependencies. The "single offline file" property is a feature, not a constraint to work around.

---

## Credits & Acknowledgements

- **[pako](https://github.com/nodeca/pako)** by Vitaly Puzrin and Andrey Tupitsin — MIT + Zlib licensed gzip implementation, inlined into the HTML file.
- **Vanguard Galaxy** by Maverick Games — go [buy it on Steam](https://store.steampowered.com/app/3471800/Vanguard_Galaxy/) and join the Discord.
- The [Vanguard Galaxy Wiki](https://vanguard-galaxy.fandom.com/) and [FearLess Cheat Engine forum thread](https://fearlessrevolution.com/viewtopic.php?t=37749) for documenting the faction/material structures.

---

## License

[MIT](LICENSE) — do whatever you want with it.

The pako library is bundled under its own MIT/Zlib license; the original copyright header is preserved in the inlined code.

---

## Disclaimer

This tool reads, modifies, and writes save files on your local machine only. It does not connect to any server, does not collect data, and does not communicate with the game while it is running. Use at your own risk; always back up your save. The author is not responsible for corrupted saves, lost progress, or violation of any future Terms of Service. If save format support breaks after a game update, file an issue.

If a save edit causes the game to crash or behave strangely, restore your backup. If you don't have a backup, that's a great lesson and you'll never forget it again.
