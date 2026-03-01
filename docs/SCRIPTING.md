# Python Scripting

WorldEditorRemix embeds **Python 3.8** (standalone, no system Python required). Scripting lets you automate terrain edits, object manipulation, attribute painting, water cleanup, and much more — all from within the running editor.

---

## Enabling scripting

In `WorldEditorRemix.ini`:

```ini
ENABLE_PYTHON = true
```

Make sure the `lib38\` folder is present next to the `.exe`. That folder contains the entire Python runtime.

---

## Entry point

```
lib38\WorldEditorRemix.py
```

This is the **only file** executed directly by the editor. Edit it freely. Press **F5** while any scene tab (Map / Object / Effect / Fly) is active to run it.

The file is re-read from disk on every F5 press, so there is no need to restart the editor between changes.

---

## Available modules

Inside `WorldEditorRemix.py` (and any module it imports) the following are pre-imported by the host:

| Module | Description |
|---|---|
| `WorldEditor` (aliased as `we`) | Native C++ bindings — the full editor API |
| `dbg` | Logging helpers (`dbg.Tracen()`, `dbg.LogBox()`) |

Standard Python 3.8 library modules are available as normal (`os`, `sys`, `json`, `math`, `time`, etc.).

The `WorldEditorRemixWrapper` package lives at `lib38\WorldEditorRemixWrapper\` and is importable as:

```python
from WorldEditorRemixWrapper import WorldEditorWrapper, CONSTANTS, terrain_operations
from WorldEditorRemixWrapper import show_ui
```

---

## Minimal script example

```python
import WorldEditor as we
import dbg

if we.IsMapReady():
    name = we.GetMapName()
    dbg.LogBox("Current map: {}".format(name))
else:
    dbg.LogBox("No map loaded.")
```

---

## Using the wrapper

`WorldEditorWrapper` is a convenience class that wraps every `WorldEditor` function with Pythonic return types (booleans instead of ints, etc.) and adds higher-level helpers:

```python
from WorldEditorRemixWrapper import WorldEditorWrapper
WE = WorldEditorWrapper()

if WE.IsMapReady():
    cx, cy = WE.GetTerrainCount()
    dbg.Tracen("Map size: {}x{} terrains".format(cx, cy))

    real_x, real_y = WE.GetRealTargetPosition()
    dbg.Tracen("Camera at real coords: {}, {}".format(real_x, real_y))
```

See [WRAPPER_REFERENCE.md](WRAPPER_REFERENCE.md) for the full method list.

---

## Terrain operations

High-level operations (attribute painting, water cleanup, grass placement) are in `terrain_operations`:

```python
from WorldEditorRemixWrapper import terrain_operations

# Paint ATTRIBUTE_BLOCK on all pixels covered by "stone" textures
terrain_operations.ApplyAttributesByTexturePattern()

# Remove water pixels that lack ATTRIBUTE_WATER
terrain_operations.CleanupWaterWithoutAttr()

# Scatter grass props on grass-textured tiles
inserted, skipped = terrain_operations.PlaceGrassOnGrassTextures(min_distance_meters=10.0)
dbg.LogBox("Inserted: {}, Skipped: {}".format(inserted, skipped))
```

See [TERRAIN_OPERATIONS.md](TERRAIN_OPERATIONS.md) for the full reference.

---

## Showing the UI panel

The wrapper ships a floating Win32 overlay panel that shows live map info and buttons for the terrain operations:

```python
from WorldEditorRemixWrapper import show_ui
show_ui()
```

The panel stays on top of the editor window and updates every 2 seconds. See [UI.md](UI.md) for details.

---

## Logging

```python
import dbg

dbg.Tracen("This goes to the debug console / log file")
dbg.LogBox("This pops up a message box")
```

`dbg.Tracen` appends a newline automatically. `dbg.LogBox` is useful for one-off confirmations.

---

## CONSTANTS dictionary

`WorldEditorRemixWrapper.CONSTANTS` exposes all numeric constants from `WorldEditor` as a plain dict, useful for scripts that need to pass values without importing `WorldEditor` directly:

```python
from WorldEditorRemixWrapper import CONSTANTS

brush_circle = CONSTANTS["BRUSH_SHAPE_CIRCLE"]
terrain_size = CONSTANTS["TERRAIN_SIZE"]
```

---

## Workflow tips

- The script runs **synchronously** in the editor's main thread. Long loops will freeze the UI. For heavy operations, log progress with `dbg.Tracen` and let the editor repaint between terrain chunks if possible.
- Always check `we.IsMapReady()` (or `WE.IsMapReady()`) before touching terrain or object APIs. Many calls crash or silently fail when no map is loaded.
- Use `try / except` around your code during development — unhandled exceptions print to the console but won't crash the editor.
- The script file is re-executed top-to-bottom every F5 press. Module-level state (globals, class instances) does **not** persist between runs unless you store it somewhere the GC won't collect (e.g. `sys.modules`).

---

## Example: move camera to world coordinate

```python
from WorldEditorRemixWrapper import WorldEditorWrapper
WE = WorldEditorWrapper()

# Move camera to world position (1000, 2000)
WE.MoveToRealTargetPosition(1000, 2000)
```

## Example: flatten a region of terrain

```python
import WorldEditor as we

TARGET_HEIGHT = 1000  # editor units

x1, y1 = 100, 100
x2, y2 = 200, 200

we.DrawHeightRegion(x1, y1, x2, y2, TARGET_HEIGHT)
we.RefreshTerrain()
```

## Example: paint an attribute flag manually

```python
import WorldEditor as we

FLAG = we.ATTRIBUTE_BLOCK

for y in range(50, 100):
    for x in range(50, 100):
        we.SetAttrAt(float(x * 200), float(y * 200), FLAG)

we.RefreshAllTerrainAttrs()
```
