# Gather Map

Gather Map is a self-learning resource map overlay for World of Warcraft
1.12. It remembers successfully gathered herbs, minerals, and Survival
trees, plus skinning results, lockpicking practice chests, and ordinary treasure
chests. Saved locations appear as resource icons on both the minimap and the
world map, without corner dots. Minimap icon scale is adjustable in the GUI.

Locations are saved account-wide in `GatherMapDB`, so every character can use
the same database and the locations remain available after logout or `/reload`.

![Saved herbs on the Tanaris world map](Screenshots/world-map-tanaris.png)

## How it works

- Successfully gather, mine, cut a tree, skin, or open a chest to teach Gather
  Map the resource and position.
- Normal interaction and the
  [Interact](https://github.com/lookino/Interact) hotkey are supported.
- Each location records its main map zone and the smaller minimap subzone name.
- Difficulty is recalculated from current skills or character level. Gray
  (low-value) nodes retain 85% opacity; icons keep their original artwork.
- Recorded profession markers are only displayed when the current character has
  the corresponding profession or skill; preloaded database markers are an
  exception (also shown when untrained). Account-wide
  saved locations remain available on all characters.
- Custom resources can learn their requirement when the tooltip includes a
  `Requires Herbalism (N)`, `Requires Mining (N)`, or similar line.

## Supported resources

- **Herbalism:** saves the herb node and compares it with Herbalism.
- **Mining:** saves ore veins and deposits and compares them with Mining.
- **Survival:** saves Simple, Bright, Shade, Tropical, Dead, and Star Wood Trees
  after successful Woodcutting and compares them with Survival. Markers only
  appear on characters who know Survival; Find Trees does not need to remain
  active after a location has been learned.
- **Skinning:** saves the leather, hide, scale, or other item actually obtained
  and compares the skinned creature's requirement with Skinning. These markers
  are hunting spots rather than exact spawn points because creatures move. The
  requirement follows Vanilla's low-level and high-level creature formulas, so
  the difficulty changes as the character's Skinning skill increases. Known
  Vanilla leathers and hides always use their exact inventory icons, even when
  the client has not cached the looted item's texture yet.
- **Lockpicking:** saves world chests that expose a Lockpicking requirement and
  compares them with the rogue's Lockpicking skill. These markers are only
  displayed while playing a rogue.
- **Treasure chests:** saves ordinary open-world chests. Known Vanilla chest
  tiers compare with character level; custom tiers may have unknown difficulty.

## Map markers

Both maps display the complete resource texture without cropped edges or
colored corner dots. Hover over an icon to see the resource name.

The minimap uses 16-unit square icons by default. Open `/gmap config` and
adjust **Minimap icon scale** from 50% to 200% (8 to 32 units). Changes apply
immediately and are saved account-wide. Icons near the circular rim are hidden
before their corners would extend beyond the minimap.

World-map icons default to 16-unit squares. The separate **World map icon scale**
slider adjusts them from 50% to 200%, independently of the minimap. Nearby nodes of the same resource
still merge, but their combined bounds only determine placement: the icon and
its hover area stay square and never grow with the group. Different resources
remain separate and can overlap. Use the world map's **Gather Map** checkbox to
show or hide the overlay.

Low-difficulty (gray) markers retain 85% opacity. All other icons use full
opacity and their original texture colors.

## Settings GUI and minimap button

Click the new Gather Map button around the minimap or type `/gmap config` to
open the settings window. Drag the minimap button to reposition it.

The **Record resources** checkboxes independently enable/disable learning
Herbalism, Mining, Skinning, Woodcutting, Lockpicking, or Treasure Chests.
All six are enabled by default, including for existing saved databases.
Disabling a category prevents **new** locations from being added or existing
ones updated; it does **not** erase previously collected locations or hide their
markers. The separate minimap and world-map display checkboxes control marker
visibility without affecting gathering data collection. Preferences and the
minimap button's angle are saved account-wide in `GatherMapDB`.

## Commands

- `/gmap config` or `/gathermap options` — open/close settings.
- `/gathermap` or `/gathermap count` — show counts by resource and current skills.
- `/gathermap on` — show minimap markers.
- `/gathermap off` — hide minimap markers while continuing to learn locations.

## Installation

1. Extract or copy the `GatherMap` folder into `World of
   Warcraft\Interface\AddOns`.
2. Restart WoW or run `/reload` if the add-on was already installed.
3. Enable **Load out of date AddOns** on the character screen if the client asks
   for it.

No other add-on is required. Interact integration is enabled automatically when
Interact is installed.

## Upgrade from a previous add-on name

If you used this add-on under a previous folder name, migrate your saved data
before playing with the new installation. A normal file replacement is not
enough when the folder and saved-variable names change.

1. Fully close the game. Keep the previous add-on installed for this step.
2. Install the new `GatherMap` folder beside the previous folder.
3. Open PowerShell in `GatherMap` and run the following, replacing both example values:

   ```powershell
   .\Upgrade-SavedData.ps1 -ClientPath "C:\Games\World of Warcraft" -PreviousAddonName "PreviousAddonFolder"
   ```

The helper reads the previous TOC, copies account-wide and per-character saved
data to the new names, and saves backups under `AddonUpgradeBackups` in the
game folder. It keeps the original files intact and refuses to overwrite
existing saved data for the new add-on. It does not execute saved Lua code.

4. Disable the previous add-on and enable **GatherMap** before entering the world.
5. Check your settings and saved data. Keep the backups until you have verified them.

Further updates using the same add-on name keep your saved data normally.
On other operating systems, back up the files, then copy the previous add-on's
`.lua` file in each `WTF/Account/**/SavedVariables` directory to `GatherMap.lua`.
Change only the top-level variable name declared in the old TOC to the matching
name in the new TOC; leave its table contents unchanged. Do this with the game closed.

## Important limitations

Gather Map markers represent saved possible spawn locations or, for skinning,
places where a resource was previously obtained. Blizzard's yellow tracking
dot still indicates whether a tracked herb or mineral is currently spawned;
the Vanilla 1.12 API does not reveal the identity of an individual live
tracking dot.

World-map markers appear on a specifically selected zone map, not on the full
continent view. Gather Map includes minimap dimensions for supported custom outdoor and event maps: Hyjal, Scarlet Enclave, Lapidis Isle,
Gillijim's Isle, Tel'Abim, Alah'Thalas, Gilneas, Icepoint Rock, Blackstone
Island, Thalassian Highlands, Winter Veil Vale, Grim Reaches, Balor, Northwind,
and Moonwhisper Coast. Custom areas inside an existing Vanilla zone, such as
Tirisfal Uplands, automatically use their parent map's dimensions.

![Herbalism markers in the custom Grim Reaches zone](Screenshots/world-map-grim-reaches.png)

## Credits

- Minimap coordinate scaling and bundled herb artwork are based on the original
  [Gatherer](https://github.com/jsb/Gatherer) project for Vanilla WoW.
- Grim Reaches dimensions use map data from
  [MetaHunt](https://github.com/DuvelCorp/MetaHunt).

## Version 0.6.0: built-in pfQuest gathering locations (Turtle 1.18)

An independent, preinstalled location dataset now ships in `PresetDatabase.lua`.
There are **4,445 approximate resource spawn locations from 41 Vanilla areas**,
selected as a lightweight spatial sample from 16,957 ore/herb location records
in the [`The-Kludge-Bureau/pfQuest`](https://github.com/The-Kludge-Bureau/pfQuest)
Vanilla database. Unlike a full dump, this deliberately does **not** include
all known spawn points. The positions are rounded to 0.2 map-percentage-point
increments. Some source locations are rare/empty and not represented; Turtle
custom zones are not yet covered by the preinstalled Vanilla database.

Click the Gather Map minimap button or type `/gmap config`. The new **Map
database (pfQuest)** options let you independently toggle:

- **Preloaded herbs** (enabled initially)
- **Preloaded mining nodes** (enabled initially)
- **My recorded locations** (enabled initially)

These affect **display only**, separately from the existing **Record resources**
options. All personal discoveries remain in account-wide `GatherMapDB.nodes`.
The imported locations stay in the add-on's read-only preset database, are never
copied into SavedVariables, and never overwrite the player's observed nodes.
Preset tooltips include `(pfQuest DB)` to distinguish them from discoveries.

Presets are decoded only for the map being viewed and the minimap accesses only
nearby nodes. When the character does not have the relevant profession, preset
markers appear in the unknown-skill color instead of disappearing. The world-map
view is periodically refreshed, with an upper limit on visible marker frames to
avoid flooding the 1.12 UI. This is a lightweight add-on sample, not the full
pfQuest module, and does not require pfQuest to be installed.

**Data credit/license:** Sampling uses the pfQuest resource classification,
object names and coordinate data by Eric Mauser (Shagu), licensed MIT; see
`PFQUEST-LICENSE.txt` and the upstream project above. The database source is
`The-Kludge-Bureau/pfQuest`, branch `main`, tree
`6b7085f0d8c49f833c2ff54fc97ae07dccc83379`.

## Version 0.6.1: resource icons and minimap scale

- Replaced minimap corner dots with resource icons and removed world-map dots.
- Added a saved 50%-200% minimap icon scale slider to the settings GUI.
- Fixed stretched world-map icons caused by using merged-node bounds as size.
- Removed texture cropping; icons use full UV coordinates on both maps.
- Existing recorded nodes, presets and recording preferences are preserved.

Validation: Lua 5.1 execution with mocked game UI APIs covered merged groups,
uniform dimensions, full textures, slider updates, saved setting initialization,
pool reuse and visibility toggles. Final appearance needs verification in-game.

## Version 0.6.2

- Added a saved 50%-200% world-map icon scale slider.
- Removed the (pfQuest DB) suffix from node tooltips on both maps.

## Version 0.6.3

- Cursor tooltips follow every frame with a 3-unit offset.
- Minimap button artwork fits within its circular rim.
- GUI database options are named Herbs and Mining Nodes.
- With matching resource tracking active, minimap icons fade to 8% at an
  estimated 100 yards or nearer; 100-120 yards blends back to normal opacity.
  This estimates detection distance, not actual spawn presence. World-map
  icons are unchanged. If the tracking API is unavailable, both resource kinds
  use distance fading; tracking disabled or another tracking type means no fade.

## Version 0.6.4

- Expanded herb/mining presets from the installed pfQuest-octo database.
- Verified mining icons against Octo's item database, including Silver Ore.
- Known veins override stale recorded loot textures.
- Removed the world map's 300-group sampling limit so dense zones retain nodes.
- Saved personal nodes and preferences are unchanged. See DATABASE-SOURCES.md.

## Version 0.6.5

- Node Tracking tab: Mining above Herbs, sorted by required skill, all enabled
  initially. Filters affect recorded and preset nodes on both maps, not learning.
- Matching tracking fully hides minimap icons and their hover targets within
  the estimated 100-yard detection radius (smooth transition at 100-120 yards).
- Minimap nodes use individual coordinates, without group-center displacement.
- Zone dimensions from pfQuest-octo drive minimap scaling where available.
- Source coordinates retain 0.01 map percentage point precision; previous
  rounded locations are retained only when no source point occupies that cell.

## Version 0.6.6

- Matching tracking fully hides addon nodes within 200 yards; transition to
  normal opacity occurs between 200 and 220 yards.
- Hold Ctrl with the pointer inside the circular minimap to temporarily hide
  all addon minimap nodes and their tooltips. Release Ctrl or move outside to
  restore them, subject to normal tracking and resource filters.
- World-map markers and native tracking markers are unaffected.
