# UI Panel

WorldEditorRemix ships a floating Win32 overlay panel that shows live map information and provides one-click buttons for common terrain operations. It is created by the Python scripting system.

---

## Opening the panel

The panel is launched at the end of `lib38\WorldEditorRemix.py`:

```python
from WorldEditorRemixWrapper import show_ui
show_ui()
```

Press **F5** with a scene active to execute the script and open the panel. If the panel is already open, it is brought to the foreground instead of creating a new one.

---

## Panel layout

The panel is a 400×600 px always-on-top window titled **"World Editor Remix UI SCRIPT"**.

### Live info labels (top section)

Updated every **2 seconds** automatically:

| Label | What it shows |
|---|---|
| **Map Name** | Current map name, or status (`Map not ready`, `No map`, etc.) |
| **Terrain Count** | `WxH` terrain chunk grid size |
| **Base XY** | World-space origin coordinates of the map |
| **Camera** | Raw editor camera target `(X, Y)` |
| **Real Camera** | Camera target converted to map coordinates |
| **Selected** | Number of currently selected objects |
| **Scene** | Active scene index and name |
| **Env** | Full path of the loaded environment script |

### Action buttons (bottom section)

| Button | Script call | See |
|---|---|---|
| Apply Attributes By Texture Pattern | `terrain_operations.ApplyAttributesByTexturePattern()` | [TERRAIN_OPERATIONS.md](TERRAIN_OPERATIONS.md) |
| Cleanup Water Without Attr | `terrain_operations.CleanupWaterWithoutAttr()` | [TERRAIN_OPERATIONS.md](TERRAIN_OPERATIONS.md) |
| Place Grass On Grass Textures | `terrain_operations.PlaceGrassOnGrassTextures()` | [TERRAIN_OPERATIONS.md](TERRAIN_OPERATIONS.md) |
| Delete Grass Objects By CRC List | `terrain_operations.DeleteGrassObjectsByCRCList(1)` | [TERRAIN_OPERATIONS.md](TERRAIN_OPERATIONS.md) |
| Replace Objects By CRC | replaces CRC `1602256688` → `832768113` | [TERRAIN_OPERATIONS.md](TERRAIN_OPERATIONS.md) |

Each button shows a `dbg.LogBox` confirmation (or error message) after its operation completes, then refreshes the labels.

> **Note:** The hardcoded CRCs in the **Replace Objects By CRC** button are examples from the default script. Edit `ui.py` → `on_replace_objects` to change them to your own CRC values.

---

## Customising the panel

The UI lives in `lib38\WorldEditorRemixWrapper\ui.py`. It is plain Python and Win32 API — no external UI framework required.

### Adding a button

1. Add an entry to `BUTTON_IDS`:
   ```python
   BUTTON_IDS = {
       ...
       'btn_my_tool': 1006,
   }
   ```
2. Add it to the `buttons` list inside `create_controls`:
   ```python
   (BUTTON_IDS['btn_my_tool'], "Run My Tool"),
   ```
3. Handle it in `wnd_proc` under `WM_COMMAND`:
   ```python
   elif cmd_id == BUTTON_IDS['btn_my_tool']:
       self.on_my_tool()
   ```
4. Implement the handler:
   ```python
   def on_my_tool(self):
       try:
           # your code here
           dbg.LogBox("My tool completed")
           self.update_labels()
       except Exception as e:
           dbg.LogBox("Error: {}".format(str(e)))
   ```

### Changing the refresh interval

```python
TIMER_INTERVAL = 2000  # milliseconds
```

Set to a smaller value for faster updates or a larger value if performance matters.

---

## Managing the panel lifetime

```python
from WorldEditorRemixWrapper import get_or_create_ui, cleanup_ui

# Get the existing panel (or create one)
ui = get_or_create_ui()

# Destroy all panels and clean up
cleanup_ui()
```

The panel is also destroyed automatically via `atexit` when the Python interpreter shuts down.

### Pumping messages manually

`show_ui()` calls `ui.pump_messages()` once after creating the window. The Win32 timer handles periodic updates, so you do not need to keep calling `pump_messages()` yourself. If you need to process pending messages from a script loop:

```python
ui = get_or_create_ui()
if ui:
    ui.pump_messages()
```
