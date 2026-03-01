# Known Issues

This page documents all known limitations in WorldEditorRemix. Items marked **FIXED** have been resolved in a previous version; they are retained here for reference.

---

| # | Status | Issue | Workaround |
|---|---|---|---|
| 1 | **FIXED v11** | Shadow output option: when `UpdateUI` is called, shadows are hidden even though the checkbox is pressed | Use the **Shadow Recalculate** button |
| 2 | Open | The BGM player requires `/miles` folder; fadein/fadeout doesn't work until a map is loaded | Load a map before playing BGM |
| 3 | Open | **Adjust Height** button only works when `.mdatr` height data is detected (requires `DETECT_MDATR_HEIGHT = true` and the culling manager to be active) | Enable `DETECT_MDATR_HEIGHT` in `.ini` |
| 4 | **FIXED v44** | Debug build is very laggy on maps like `n_flame_dungeon` / `n_ice_dungeon` due to intensive `SphereRadius` checks | Use the Release build for those maps |
| 5 | **FIXED v47** | Loading a map corrupts the script panel camera perspective (shows `0z` instead of correct Z) | Press **Y** to reset to default perspective |
| 6 | **FIXED v46** | Some tree objects become immovable / non-selectable after placement | Select a normal building and the trees together; moving the building will move all |
| 7 | Open | The server_attr generator cleans flags above bit 7 (bits 8–31) to preserve the standard 8 attribute bits | By design; `SERVERATTR_REMOVE_WEIRD_FLAGS = false` disables this |
| 8 | **FIXED v15** | Files can be read from `pack/Index` but Property entries in packs are not considered | Fixed since v15 |
| 9 | Partial (v46) | Monster Area Info features are laggy and contain residual bugs | Less buggy since v46 but still laggy; avoid heavy use |
| 10 | Open | Multi-texture selection (Ctrl+click in textureset) works for brushing / init, but DELETE only removes one texture at a time | Delete textures one by one |
| 11 | Open | `.mdatr` height is not refreshed when a building is moved — requires a new placement event to trigger the update | Press **H** to manually refresh MDATR heights after moving objects |
| 12 | Open | By default, only the first 8 terrain textures of a 32×32 px region are rendered | Press `LShift+0` to raise the limit to 255; `LShift+8` to restore the 8-texture cap |
| 13 | Open | Minimap rendering misses buildings/trees in the first 2×2 terrain regions due to a Ymir cache issue | Press **T** to set the camera so those objects are visible before capturing |
| 14 | Open | When a textureset, environment, etc. fails to load, the previously loaded filename remains shown in the UI | Reload manually |
| 15 | Open | Attribute flag **3** has no launcher implementation; do not use it | Some server owners repurpose it as a grass flag with a custom launcher |
| 16 | Open | Pack loading does not load texturesets from files first (if they are already in pack/); the object placer list stays empty because it reads from `property/` not `pack/property/` | Copy property data out of the pack or use a file-based workflow |
| 17 | Open | Saving `regen.txt` requires **Ctrl+S** (same as save map) | By design |
| 18 | **FIXED v46** | Enabling wireframe (F11 / F4) on the Attr Tab rendered the terrain all white | Fixed since v46 |
| 19 | Open | The water brush disappears when the camera renders the waterwheel small/big effect | No known workaround |
| 20 | Open | Monster Area Info markers go underground if you are outside the relative sectree | Stay within the sectree bounds when editing |
| 21 | Open | The full skybox only displays correctly after the **top** picture is added (if other textures are already inserted) | Add textures in order: top first, then the rest |
| 22 | Open | The Attr Tab slider represents "16 Photoshop-style layers" for splitting attributes; counterintuitive and confusing | Prefer the new attr checkboxes introduced in a later version |
| 23 | Open | Object attachment in the Model tab attaches static objects only; hair skeletons will not mirror the playing animation | By design limitation of the attachment system |
| 24 | **FIXED v30 / v47** | Inserting lower gradients in the Environment tab could cause an out-of-range crash | Fixed |
| 25 | Open | Brushes applied outside the screen / map bounds may affect random terrain positions | Avoid brushing near or outside map edges |

---

## TODO items (planned)

| ID | Status | Task |
|---|---|---|
| A | ✅ DONE | Support more than 8 textures per region |
| B | ✅ DONE | Shortcut to fix note #5 camera perspective |
| C | ❌ Rejected | Disable the `radius <= GetRadius()+0.0001f` assert (see note #4) — ignoring it in the Debug build would stop the lag, but the Release build is unaffected |
| D | ❌ Rejected | Translation into languages other than English |
| E | ❌ Rejected | Alternative path for `d:` drive — use `subst` instead |
| F | 🔲 TODO | Fix notes #19 and #25 (water brush / out-of-bounds brush) |
