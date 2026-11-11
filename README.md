# Hooligans Types Editor

A Windows desktop editor for **DayZ server missions**. Point it at your mission
folder and it loads everything the Central Economy uses: types, events, spawn
points, territories and the map. If you run **DayZ Expansion**, it also loads
the market, traders, AI, quests and loadouts. You edit in one place, see the
changes on the map, and save back to the original files.

**[Download the latest installer](https://github.com/bchavezmedia/hooligan-types-editor-releases/releases/latest)**:
`Hooligans-Types-Editor-Setup-<version>.exe`

---

## Getting started

1. Download and run the installer. It is code-signed by **BChavez Media**.
   For a new version, Edge may say the file "isn't commonly downloaded".
   That's Microsoft's download-reputation check for files few people have
   downloaded yet. It isn't a problem with the file. Choose **…** >
   **Keep** > **Keep anyway**. To check the file, right-click it >
   **Properties** > **Digital Signatures**: the signer should be
   **BChavez Media**.
2. Start the app and open a folder (see [What to open](#what-to-open)).
3. Edit, then save. Every save keeps a backup of the original file next to it.

To edit a single file, such as one `types.xml`, use **Open a single file…** or
drag the file onto the window.

## What to open

The app reads a DayZ server's standard folder layout. You can open any of
these three folders:

| You open | What you get |
|---|---|
| The **server folder** (the one that contains `mpmissions\`) | Everything. Pick between missions if the server has more than one. |
| The server's **`mpmissions\`** folder | Same as above. |
| A **mission folder** on its own | The mission's tabs only: no **Mods** or **Profiles** tab. Expansion files kept in a profiles folder may not be found. |

For the full set of tabs, open the server folder, or a mission folder that is
still inside `<server>\mpmissions\`.

### The expected layout

```text
DayZServer\                           <- server folder
├── serverDZ.cfg                      template="..." = the mission opened first
├── start.bat / *.cmd                 -config= and -profiles= link each mission to its profiles folder
├── @SomeMod\                         server mods (Mods tab)
├── profiles\                         the server's profiles folder (Profiles tab)
│   └── ExpansionMod\                 Expansion: Market\, Traders\, Quests\, Loadouts\, AI\LootDrops\
└── mpmissions\
    └── dayzOffline.chernarusplus\    <- mission folder; ".chernarusplus" picks the map
        ├── cfgeconomycore.xml
        ├── cfglimitsdefinition.xml   (+ cfglimitsdefinitionuser.xml)
        ├── cfgspawnabletypes.xml
        ├── cfgeventspawns.xml
        ├── cfgplayerspawnpoints.xml
        ├── cfgenvironment.xml
        ├── areaflags.map
        ├── mapgroupproto.xml
        ├── mapgrouppos.xml
        ├── db\
        │   ├── types.xml
        │   └── events.xml
        ├── env\                      territory files (*.xml)
        ├── <custom CE folders>\      any folder listed in cfgeconomycore.xml
        └── expansion\                Expansion only
            ├── settings\             AIPatrolSettings.json, AILocationSettings.json,
            │                         HardlineSettings.json, MarketSettings.json
            ├── traderzones\
            ├── traders\
            └── objects\
```

File and folder names are matched without regard to case.

### What a mission folder needs

A folder counts as a mission if it has a **`cfgeconomycore.xml`** or a
**`db\`** folder. It must also contain at least one types file: `db\types.xml`,
or a types file listed in `cfgeconomycore.xml`. Every other file is optional;
when one is missing, only the feature that uses it is unavailable.

| File | Used for |
|---|---|
| `db\types.xml` | Types tab: the vanilla types, loaded first |
| `cfgeconomycore.xml` | Your custom CE folders and their `types`, `spawnabletypes` and `events` files, loaded in the order listed. Files on disk that are not listed are flagged, since the server ignores them. |
| `cfglimitsdefinition.xml`, `cfglimitsdefinitionuser.xml` | Valid category, usage, value and tag names for the problem checks, and the names of the loot tiers |
| `areaflags.map` | Loot tier and usage zones on the map (also needs `cfglimitsdefinition.xml`) |
| `mapgroupproto.xml`, `mapgrouppos.xml` | Loot positions and the spawn finder (which buildings can spawn an item), the spawn check, and the AI generator |
| `db\events.xml` | Dynamic events and what they spawn |
| `cfgeventspawns.xml` | Event positions on the map (editable) |
| `cfgplayerspawnpoints.xml` | Player spawn points on the map (editable) |
| `cfgenvironment.xml`, `env\*.xml` | Territories and infected zones. `env\` files not listed in `cfgenvironment.xml` are flagged, since the server ignores them. |
| `cfgspawnabletypes.xml` | Checked for duplicate classes across files |
| `cfggameplay.json` | Gameplay tab: stamina, base building, map, temperatures and other gameplay settings |

### Which map is shown

The map is picked from the text after the **last dot** in the mission folder's
name, and must match exactly:

| Mission folder ends in | Map |
|---|---|
| `.chernarusplus` | Chernarus |
| `.enoch` | Livonia |
| `.sakhal` | Sakhal |
| `.deerisle` | Deer Isle |
| `.nasdara` | Nasdara |

For any other map, add a **custom map** in the Map tab: choose a map image,
then enter the map's width and height in metres.

### The server's profiles folder

The **Profiles** tab, and Expansion's Market, Traders, Quests and Loadouts,
read from the server's profiles folder. The app looks for it in this order:

1. The folder you picked for this mission in the app.
2. The `-profiles=` folder of a startup script (`.bat` / `.cmd`) in the server
   folder whose `-config=` runs this mission.
3. A folder in the server folder named after the map, such as
   `profiles_chernarus`, or `profiles\chernarusplus`.
4. A folder named `profiles` in the server folder (shared by every mission).

This means one server folder can run several maps, each with its own
profiles folder.

### DayZ Expansion

Expansion is optional. Each tab appears only when its files are found:

| Tab | Looks for |
|---|---|
| Market | `<profiles>\ExpansionMod\Market\` or `Traders\`, or the mission's `expansion\traderzones\` |
| Hardline | `expansion\settings\HardlineSettings.json` |
| Quests | `<profiles>\ExpansionMod\Quests\Quests\` |
| Loadouts | `<profiles>\ExpansionMod\`, or `expansion\settings\AIPatrolSettings.json` |
| AI on the map | `expansion\settings\AIPatrolSettings.json` or `AILocationSettings.json` |

## What it does

### Types

- **Browse every type** across all the types files your mission's
  `cfgeconomycore.xml` loads. Each type is shown as a card, coloured by nominal.
- **Search** by name, category, tag, usage or value. Separate terms with commas
  to require all of them. Filter by loot tier and sort by any attribute.
- **Bulk edit** a selection or the whole filtered list: add or remove tiers,
  usages, tags or the category, set numbers, switch flags, or change values by
  a percentage.
- **Problem checks** compare names against your `cfglimitsdefinition.xml` and
  flag anything the server would reject.
- **Spawn check** lists loot types that can't spawn anywhere in the mission.
- **Spawn finder** shows where a type can spawn on the map, and which events
  spawn it.

### Map

Built-in satellite maps for **Chernarus, Livonia, Sakhal, Deer Isle and
Nasdara**, plus the in-game paper map for all of them except Deer Isle. You can
also add a custom map image (see [Which map is shown](#which-map-is-shown)).

- **Layers:** loot tier zones, loot positions, dynamic events, territories,
  player spawn points, infected zones, Expansion AI, traders and quests.
- **Edit on the map:** event positions, player spawn points (with undo),
  territory and infected zones, and Expansion AI patrols. An AI generator
  places patrols from your map's buildings.
- **Tools:** go to coordinates, pick a point (copy it in several formats),
  measure a distance, draw a radius.
- **Ground heights:** for maps without built-in height data, a capture script
  run once on your server records the real ground height for each position.

### Gameplay

The mission's `cfggameplay.json` as a settings page: stamina and movement,
shock and drowning, base building checks, the map, the hit indicator, vehicles,
and the monthly temperatures (with a chart).

- Each setting says what it does, and shows the **vanilla value** with a reset
  link when yours differs. **Only non-vanilla** lists just the settings you've
  changed.
- Older files are missing settings that newer DayZ versions added. The tab
  lists them and can add them with vanilla values.
- Shows whether the server actually reads the file. The server ignores it
  unless its config has `enableCfgGameplayFile = 1`.

### DayZ Expansion (optional)

Each of these tabs appears when the mission has the matching files.

- **Market:** categories, traders and trader zones, with prices, stock and
  colours. Changes to buy prices can be applied to a whole category at once.
  Each trader and zone can be shown on the map.
- **Hardline:** reputation and item rarity settings.
- **Quests:** quests, NPCs and objectives, with objective locations on a map.
- **Loadouts:** AI loadouts: clothing slots, weapon attachments, cargo, item
  sets and chances, quantity and health ranges.
- **AI:** patrols and roaming locations, edited on the map.

### Server

- **Mods:** the mods your server loads. Shows whether each one matches the
  Steam Workshop copy, whether it ships a keys folder, and which items it adds.
  You can add a mod's items to the mission's loot economy.
- **Server config:** the `serverDZ*.cfg` that runs the open mission (the one
  whose `template=` names it): server name, passwords (masked), max players,
  time speed, 3rd person, voice, login queue, logs and more. Settings that
  aren't in Bohemia's standard list, such as `dogtagsPlaytimeRequired`, are
  listed too. Only the lines you change are rewritten.
- **Profiles:** browse and edit the server's profile folder. Files are checked
  for valid syntax before saving.
- **Files:** open any file in the mission folder in a text editor.

## Saving and backups

- Files are saved in place, and only what you edited changes. Comments,
  element order, indentation and line endings are kept, so a save gives a
  clean diff.
- A `.bak` backup is written before each save. You choose how many backups to
  keep per file in **Settings**.
- Tabs with unsaved changes are marked with **•**, and the app asks before
  discarding them.

## Requirements

- Windows 10 or 11 (64-bit)
- A DayZ server, or a mission folder, laid out as described in
  [What to open](#what-to-open). Expansion is optional.

---

Questions or bugs: contact BChavez Media.
