# Terrain Operations Reference

`terrain_operations` is a high-level module that bundles common map editing workflows. It lives at `lib38\WorldEditorRemixWrapper\terrain_operations.py` and is importable from your script:

```python
from WorldEditorRemixWrapper import terrain_operations
```

All functions guard against a not-ready map at the start and log progress via `dbg.Tracen`.

---

## Texture ID lookup

```python
terrain_operations.GetTextureIdsByPattern(pattern: str) -> set
```

Scan all textures registered in the current map's textureset and return the set of texture indices whose filenames contain `pattern` (case-insensitive).

**Example:**
```python
stone_ids = terrain_operations.GetTextureIdsByPattern("stone")
# → {3, 7, 12}
```

Iterates from index 1 upward until `GetTerrainTextureFilename` raises an exception (end of list).

---

## Apply attributes by texture pattern

```python
terrain_operations.ApplyAttributesByTexturePattern() -> None
```

A two-pass automated attribute painter:

1. Finds all texture IDs whose filename contains `"stone"` → marks those tiles with `ATTRIBUTE_BLOCK`
2. Finds all texture IDs whose filename contains `"tile"` → marks those tiles with `ATTRIBUTE_BANPK`

Uses `we.ApplyAttrToTerrainByTile` per terrain chunk for efficiency (native bulk operation). Calls `we.RefreshAllTerrainAttrs()` at the end.

**When to use:** After setting up your textureset, run this once to auto-paint the standard attribute flags based on texture naming conventions. You will typically need to touch up edge cases manually.

**Logs:**
- Number of stone / tile texture IDs found
- Per-terrain pixel counts
- Total pixels painted
- Number of terrain chunks refreshed

---

## Clean up water without attribute

```python
terrain_operations.CleanupWaterWithoutAttr() -> None
```

Delegates to the native `we.CleanupWaterWithoutAttr()` and logs detailed results:

| Metric | Description |
|---|---|
| `terrainsChecked` | Total terrain chunks examined |
| `terrainsWithWater` | Chunks containing any water pixels |
| `terrainsWithWaterAttr` | Chunks with `ATTRIBUTE_WATER` set |
| `terrainsWithWaterButNoAttr` | Chunks where water pixels exist but the attr flag is missing |
| `totalWaterPixels` | Total water pixel count across all chunks |
| `waterPixelsWithAttr` | Water pixels that also carry the attribute |
| `terrainsCleaned` | Chunks where orphaned water was removed |

**When to use:** After painting or importing water, run this to remove any water pixels that lack the matching `ATTRIBUTE_WATER` flag — a common source of visual glitches.

---

## Place grass on grass textures

```python
terrain_operations.PlaceGrassOnGrassTextures(min_distance_meters: float = 10.0) -> (int, int)
```

Scatter grass/plant props over all tiles that use a texture whose name contains `"grass"`.

- Uses `GetTextureIdsByPattern("grass")` to find matching textures.
- Calls `we.PlaceGrassPropsByTexture` with `CRCLIST_PLANTS` (see below) and `min_distance_meters` spacing.
- Returns `(inserted, skipped)`.

**`min_distance_meters`**: Minimum spacing between placed objects in metres. Increase to thin out density.

**Example:**
```python
inserted, skipped = terrain_operations.PlaceGrassOnGrassTextures(min_distance_meters=15.0)
```

---

## Place objects on specific textures

```python
terrain_operations.PlaceObjectsOnSpecificTextures(
    objectList: list,
    texturePrefix: str,
    min_distance_meters: float = 10.0
) -> (int, int)
```

Generic version of `PlaceGrassOnGrassTextures`. Scatter objects with CRCs in `objectList` over tiles matching `texturePrefix`. Returns `(inserted, skipped)`.

**Example:**
```python
MY_CRCS = [123456789, 987654321]
terrain_operations.PlaceObjectsOnSpecificTextures(MY_CRCS, "sand", 20.0)
```

---

## Delete grass objects by CRC list

```python
terrain_operations.DeleteGrassObjectsByCRCList(scope: int = 1) -> int
```

Delete all objects whose CRC is in `CRCLIST_PLANTS` (runs the deletion twice to handle any that were skipped on the first pass). Returns number deleted on the first pass.

`scope`: `1` = current loaded map area.

---

## Delete objects by CRC list

```python
terrain_operations.DeleteObjectsByCRCList(objectList: list, scope: int = 1) -> int
```

Generic version. Delete all objects whose CRC appears in `objectList`. Runs deletion twice (first pass result is returned). Returns number deleted.

---

## Replace objects by CRC

```python
terrain_operations.ReplaceObjectsByCRC(source_crc: int, target_crc: int, scope: int = 1) -> int
```

Replace every object with `source_crc` with an object using `target_crc`, preserving position, rotation, and scale. Returns number replaced.

**Example:**
```python
old_crc = 1602256688   # warpgate02.pre
new_crc  = 832768113   # warpgate01_otherworld_01.pre
terrain_operations.ReplaceObjectsByCRC(old_crc, new_crc, 1)
```

---

## CRCLIST_PLANTS

The built-in CRC list used by grass functions:

```python
CRCLIST_PLANTS = [
    802175187,   # aloevera_rt_flowers_02.prt
    1030411250,  # aloevera_rt_flowers_01.prt
]
```

Commented-out entries in the source include various `secretdungeon/flower/` CRCs. Uncomment or extend the list in `terrain_operations.py` to include your own props.

---

## Attribute constants

The module re-exports the two most common attribute flags for convenience:

```python
from WorldEditorRemixWrapper.terrain_operations import ATTRIBUTE_BLOCK, ATTRIBUTE_BANPK
```

These are identical to `we.ATTRIBUTE_BLOCK` and `we.ATTRIBUTE_BANPK`.
