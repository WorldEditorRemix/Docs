# Shortcuts

## Keyboard shortcuts

| Key | Action |
|---|---|
| `ESC` | Clear active cursor / cancel |
| `Delete` | Delete selected buildings / objects |
| `Ctrl+S` | Save map (also saves `regen.txt`) |
| `Insert` or `F6` | Save shadow map + `minimap.dds` |
| `F3` | Toggle BoundGrid display |
| `F4` | Toggle render UI visibility |
| `F11` | Toggle wireframe |
| `F5` | Execute `lib38\WorldEditorRemix.py` (Python scripting) |
| `R` | Reload textures |
| `Z` | Decrease texture splat by 0.1 |
| `X` | Increase texture splat by 0.1 |
| `CapsLock` | Show Gaussian/Cubic shadow effect (when shadows are displayed) |
| `H` | Refresh MDATR heights for moved objects (workaround for [Known Issue #11](KNOWN_ISSUES.md)) |
| `Y` | Reset camera perspective to default (workaround for [Known Issue #5](KNOWN_ISSUES.md)) |
| `T` | Set camera to capture all on-screen objects (use before Insert/F6 for minimap — see [Known Issue #13](KNOWN_ISSUES.md)) |
| `Space` | Start move/play animation in Object / Effect / Fly scene |
| `ESC` | Stop animation in Effect / Fly scene |

### Texture flag visualisation (Left Shift + number)

| Key | Effect |
|---|---|
| `LShift+1` to `LShift+6` | Display TextureCountThreshold flags (&2–7) as colour overlays on the ground |
| `LShift+8` | Set maximum showable textures per region to **8** (default limit — re-applies [Known Issue #12](KNOWN_ISSUES.md)) |
| `LShift+0` | Set maximum showable textures per region to **255** (removes the 8-texture cap) |

---

## Camera movement

| Key(s) | Movement |
|---|---|
| `W` `A` `S` `D` | Move camera (minimal step, async diagonal support) |
| `↑` `←` `↓` `→` | Move camera × 10 step (async diagonal support) |
| `Right Mouse` | Rotate / pan camera (may require `Ctrl` when editing daylight/attr — removed in Remix) |

---

## Mouse wheel shortcuts

> **Important:** Do **not** have an object selected when using MW+4 through MW+7.

| Modifier held | Mouse Wheel action |
|---|---|
| `1` | Rotate selected object around **X** axis |
| `2` | Rotate selected object around **Y** axis |
| `3` | Rotate selected object around **Z** axis |
| `4` | Move cursor height base (×1) |
| `5` | Move cursor height base (×0.5) |
| `6` | Move cursor height base (×0.05) |
| `7` | Move cursor ambience scale (×1) |
| `8` | Rotate selected objects around a **common pivot** on Z |
| `Q` | Move selected object height (×1) |
| `9` | Move selected object **X** position (×1, async) |
| `0` | Move selected object **Y** position (×1, async) |
| `RShift+9` / `RShift+0` | Move selected object X / Y (×10, async) |
| `RCtrl+9` / `RCtrl+0` | Move selected object X / Y (×100, async) |

---

## Object clipboard

WorldEditorRemix has a cross-instance clipboard for objects. The clipboard data is plain text and can be saved as `.txt`, shared between users, or pasted across multiple running instances.

| Key | Action |
|---|---|
| `Ctrl+C` | Copy selected objects (stores ID, rotation, local position, portal ID) |
| `Ctrl+V` *(hold 1 s)* | Paste objects from clipboard — **keep Ctrl+V held for ~1 second** to be recognised |

---

## Mouse

| Action | Effect |
|---|---|
| `Left click` | Insert object at cursor position |
| `Right click` | Move camera |
