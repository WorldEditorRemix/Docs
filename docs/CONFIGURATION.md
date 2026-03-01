# Configuration — WorldEditorRemix.ini

`WorldEditorRemix.ini` lives next to the `.exe` and is read at startup. Changes take effect on the next launch (unless noted otherwise).

Lines starting with `;` are comments. All option names are **case-sensitive**.

---

## [config] — General options

### Output defaults

```ini
VIEW_CHAR_OUTPUT_BY_DEFAULT   = true
VIEW_SHADOW_OUTPUT_BY_DEFAULT = true
VIEW_WATER_OUTPUT_BY_DEFAULT  = true
```

Control which render layers are enabled when the editor first opens.

---

### Window

```ini
; WINDOW_HEIGHT_SIZE = 1080
; WINDOW_WIDTH_SIZE  = 1920
WINDOW_FOV_SIZE    = 45
```

`WINDOW_HEIGHT_SIZE` / `WINDOW_WIDTH_SIZE` — override the initial window dimensions. Commented out by default (uses system defaults).

`WINDOW_FOV_SIZE` — field of view in degrees. `45` is the default.

```ini
CENTERED_WINDOW = true
```

Centre the program window on the primary monitor at launch.

---

### Camera

```ini
WASD_MINIMAL_MOVE  = 100
```

Minimum movement in units per WASD keypress (`100` = 1 px at the terrain grid scale).

```ini
MOUSE_WHEEL_MIN = 200.0
MOUSE_WHEEL_MAX = 80000.0
```

Clamp the camera zoom range controlled by the mouse wheel.

---

### Insert / F6 behaviour

```ini
NO_GOTO_AFTER_INSERT = true
```

When `true`, pressing Insert (or F6) to save the shadow map / minimap does **not** move the camera to the origin — it stays where you were.

---

### Atlas / minimap output

```ini
NOMAI_ATLAS_DUMP = true
```

Suppress `.mai` monster-area-info dumps when saving the atlas or pressing Insert/F6.

```ini
NOMINIMAP_RAWALPHA = true
```

When saving `minimap.dds`: strip the alpha channel and enable DXT compression. Produces a smaller file compatible with most launchers.

---

### Terrain height collision

```ini
DETECT_MDATR_HEIGHT = true
```

Load `.mdatr` building height data for the collision manager. Required for the **Adjust Height** button to work correctly. See [Known Issues #3](KNOWN_ISSUES.md).

---

### Fog

```ini
NOFOG_ONMAPLOAD = true
```

Automatically disable fog when loading a map. Useful for editing visibility.

---

### UI refresh

```ini
REFRESHALL_ONUPDATEUI = false
```

When `true`, refreshes all checkbox states whenever `UpdateUI` is called. Can cause shadow flickering; keep `false` unless you need it.

---

### New map defaults

```ini
NEW_MAP_MAPNAME_PREFIX = "metin2_map_"
```

Pre-fill the map name field when creating new maps. Set to `""` to disable.

```ini
NEWMAP_TEXTURESETLOADPATH = "textureset\metin2_a1.txt"
```

Load this textureset automatically when creating a new map. If the file does not exist it is created as empty. Set to `""` to disable.

```ini
NEWMAP_TEXTURESETSAVEASMAPNAME = true
```

When `NEWMAP_TEXTURESETLOADPATH` is empty **and** the TextureSet path field is empty, auto-create `textureset/{mapname}.txt`. Ignored if `NEWMAP_TEXTURESETLOADPATH` is set or the field is filled in manually.

---

### server_attr generator

```ini
SERVERATTR_REMOVE_WEIRD_FLAGS = true
```

Strip attribute flags above bit 7 (bits 8–31) during server_attr generation so all 8 standard attribute bits (0–7) are preserved cleanly.

---

### Object display

```ini
VIEW_OBJECT_LIGHTING = true
```

Show diffuse lighting on placed objects.

```ini
OBJECT_HEIGHT_MAX = 5000
```

Maximum object height offset in units (`5000` = 50 m). Must be ≥ `OBJECT_HEIGHT_SLIDER_MAX`.

```ini
OBJECT_HEIGHT_SLIDER_MAX = 3000
```

Maximum value shown on the object height slider (`3000` = 30 m).

---

### Mob proto

```ini
MOB_PROTO_PATH = "locale/ymir/mob_proto"
```

Path to the `mob_proto` file used to populate monster lists for regen editing.

---

### Monster area info

```ini
VIEW_MONSTER_AREA_INFO = 0
```

Whether the **Monster Area Info** checkbox is checked at startup. `0` = unchecked, `1` = checked.

---

### Brush cursor colour

```ini
RENDER_CURSOR_COLOR_R = 0.0
RENDER_CURSOR_COLOR_G = 1.0
RENDER_CURSOR_COLOR_B = 0.0
```

RGB float values (0.0–1.0) for the brush cursor / object selection highlight. Default is green.

---

### Auto-save

```ini
AUTO_SAVE = false
AUTO_SAVE_TIME = 60
```

Enable periodic auto-save and set the interval in seconds. The auto-save checkbox in the UI respects this default.

---

### Auto-backup

```ini
AUTO_BACKUP = true
AUTO_BACKUP_FOLDER = "_backup"
```

Before every manual save the entire map folder is copied into `AUTO_BACKUP_FOLDER`. Each backup is stored in a dated sub-folder.

```ini
AUTO_BACKUP_SUBFOLDER_MODE = 0
```

Controls how the dated sub-folder is named inside `AUTO_BACKUP_FOLDER`:

| Mode | Layout |
|---|---|
| `0` | `{folder}/{mapName}_YYYYmmDD-HHMMSS` |
| `1` | `{folder}/{mapName}/YYYYmmDD-HHMMSS` |
| `2` | `{folder}/YYYYmmDD-HHMMSS_{mapName}` |
| `3` | `{folder}/YYYYmmDD-HHMMSS/{mapName}` |

---

### Map export

```ini
EXPORT_FOLDER = "_export"
EXPORT_SUBFOLDER_MODE = 0
```

Destination for the **Export Map** feature (copies all loaded map files for a clean release bundle). Same mode values as `AUTO_BACKUP_SUBFOLDER_MODE`.

---

### Property auto-generation

```ini
ENABLE_AUTOGEN_PROPERTY = true
```

Auto-generate missing `.property` files when the **Scan New Objects** button is pressed.

```ini
AUTOGEN_PROPERTY_EFFECT = true
```

Also auto-generate `.pre` effect property files for `.mse` files found in `d:/ymir work/effect/`.

---

### Object ID

```ini
NEW_OBJECT_ID_FROM_CRC = true
```

Calculate object IDs from a CRC of the normalised filename rather than using a random ID. Produces deterministic, reproducible IDs.

---

### Locale / language

```ini
LOCALE = "EN"
```

Set to `"EN"` to use the internal binary resources (default). Set to another code (e.g. `"DE"`) to load `WorldEditorRemix_{LOCALE}.dll`.

---

### Python scripting

```ini
ENABLE_PYTHON = true
```

Enable the embedded Python 3.8 interpreter. When `true`, pressing **F5** in any scene executes `lib38\WorldEditorRemix.py`. Set to `false` to disable scripting entirely.

---

### Minimap / MAI snapshot options

```ini
MAI_IMAGE_WITHOUT_MONSTER_AREA_INFO = false
```

When generating the MAI image, hide monster area info overlays.

```ini
MINIMAP_EFFECT_SNAPSHOT = false
```

Include effect particles in the minimap snapshot.

---

### Rendering options

```ini
ENABLE_GR2_ANIMATION = true
```

Play Granny2 animations in the viewport.

```ini
CHARACTER_SHADOW_MAP_SIZE = 512
```

Shadow map texture resolution for the main character shadow.

```ini
ENABLE_NEW_COMPASS = true
```

Show the updated compass overlay.

```ini
ENABLE_OBJECT_HIERARCHY = true
```

Show the object hierarchy list panel in the Map → Object page (supports Ctrl+click multi-select and right-click warp).

```ini
INTERSECT_OBJECT_PICKING = false
```

When `true`, use intersection-based object picking. When `false`, use the legacy bounding-box behaviour.
