# WorldEditorWrapper Reference

`WorldEditorWrapper` is a Python class that wraps every `WorldEditor` native function with cleaner return types and adds a set of higher-level helpers. It lives in `lib38\WorldEditorRemixWrapper\main.py`.

Import it in your script:

```python
from WorldEditorRemixWrapper import WorldEditorWrapper
WE = WorldEditorWrapper()
```

`WorldEditorWrapper` proxies any attribute it doesn't explicitly define straight to `WorldEditor`, so `WE.SomeFunction()` is equivalent to `we.SomeFunction()` for anything not listed below.

---

## Differences from raw WorldEditor

| Raw API | Wrapper equivalent | Difference |
|---|---|---|
| `we.IsMapReady() -> int` | `WE.IsMapReady() -> bool` | Returns Python `bool` |
| `we.IsMapLoaded() -> int` | `WE.IsMapLoaded() -> bool` | Returns Python `bool` |
| `we.IsObjectSelected(i) -> int` | `WE.IsObjectSelected(i) -> bool` | Returns Python `bool` |
| `we.SelectObjectsByRect(...) -> int` | `WE.SelectObjectsByRect(...) -> bool` | Returns Python `bool` |
| `we.DrawAttrBrush(..., erase: int)` | `WE.DrawAttrBrush(..., erase: bool)` | Accepts Python `bool` |
| `we.DrawWaterBrush(..., erase: int)` | `WE.DrawWaterBrush(..., erase: bool)` | Accepts Python `bool` |
| `we.DrawTextureBrush(..., erase: int, drawOnlyOnBlank: int)` | `WE.DrawTextureBrush(..., erase: bool, drawOnlyOnBlankTile: bool)` | Accepts Python `bool` |
| `we.SaveMap(name) -> int` | `WE.SaveMap(name) -> bool` | Returns `bool`, `name` is optional |
| `we.InitBaseTexture(name) -> int` | `WE.InitBaseTexture(name) -> bool` | `name` is optional |

All other wrapper methods follow the same pattern: `int` returns become `bool`, and `int` enabled/disabled parameters accept Python `bool`.

---

## Static helpers

```python
WorldEditorWrapper.SceneName(v: int) -> str
```
Convert a `SCENE_*` constant to a human-readable string: `"Map"`, `"Object"`, `"Effect"`, `"Fly"`, `"Max"`.

```python
WorldEditorWrapper.PropertyName(v: int) -> str
```
Convert a `PROPERTY_TYPE_*` constant to a string: `"None"`, `"Tree"`, `"Building"`, `"Effect"`, `"Ambience"`, `"DungeonBlock"`.

```python
WorldEditorWrapper.PrefixEnv() -> str
```
Returns `"d:/ymir work/environment/"` — the canonical environment script prefix.

---

## Coordinate helpers

```python
WE.GetRealTargetPosition() -> (int, int)
```
Returns the camera target position converted from raw editor units to "real" map coordinates (divides by 100 and negates Y). Useful for logging or UI display.

```python
WE.MoveToRealTargetPosition(x: int, y: int) -> None
```
Move the camera target to "real" map coordinates `(x, y)`.

```python
WE.AdjustBaseCoordinates(x: int, y: int) -> (int, int)
```
Snap `(x, y)` to the nearest valid `MAPBASE` boundary. Pops a `LogBox` warning if the input was misaligned.

```python
WE.WorldToPixel(world_x: float, world_y: float) -> (int, int)
WE.PixelToWorld(pixel_x: int, pixel_y: int) -> (float, float)
```
Convert between world-space coordinates and height-pixel coordinates.

---

## Height helpers

```python
WE.RecalculateHeightPixel(x: int, y: int) -> None
```
Read the current height pixel at `(x, y)` and write it back (effectively a no-op that forces a redraw). Prints the value to the console.

```python
WE.AdvanceHeightPixel(x: int, y: int, advance: int) -> None
```
Add `advance` to the current height pixel at `(x, y)`.

---

## Terrain geometry helpers

```python
WE.GetTerrainBounds() -> (int, int, int, int) or None
```
Returns `(minX, minY, maxX, maxY)` world-space bounds of the full loaded map, or `None` if no map is ready.

```python
WE.GetTerrainHeightRange(x1, y1, x2, y2) -> (float, float)
```
Scan all height values in the rectangle and return `(min_height, max_height)`.

```python
WE.GetTerrainSlope(x: float, y: float) -> float
```
Approximate slope magnitude at world position `(x, y)` by sampling neighbouring heights.

```python
WE.FindFlatAreas(x1, y1, x2, y2, threshold=0.1) -> list
```
Return a list of `(x, y)` pixel coordinates within the rectangle where the 2×2 height variation is ≤ `threshold`.

```python
WE.GetHeightmapStatistics(x1, y1, x2, y2) -> dict or None
```
Returns a dict with keys `min`, `max`, `mean`, `median`, `std_dev`, `count` over the scanned region.

```python
WE.BatchSetHeightRegion(x1, y1, x2, y2, height_map: list) -> bool
```
Apply a 2-D list of height values across the rectangle. `height_map[row][col]` maps to `(min_x + col, min_y + row)`.

```python
WE.BatchSetAttrRegion(x1, y1, x2, y2, attr_flag: int) -> bool
```
Set `attr_flag` on every attribute cell in the rectangle.

```python
WE.BatchApplyTextureRegion(x1, y1, x2, y2, texture_numbers) -> bool
```
Paint `texture_numbers` across the rectangle using the texture brush.

---

## Object helpers

```python
WE.GetObjectBounds(object_index: int) -> (x, y, z, x, y, z) or None
```
Returns `(x, y, z, x, y, z)` (point bounds — same position repeated) for quick existence/position checks.

```python
WE.FindObjectsByType(property_type: int) -> list
```
Return indices of all objects matching the given `PROPERTY_TYPE_*`.

```python
WE.FindObjectsInRegion(x1, y1, x2, y2) -> list
```
Return indices of all objects whose position falls within the world-space rectangle.

```python
WE.GetObjectStatistics() -> dict
```
Returns `{type_name: count, ..., "total": n}` — a breakdown of all placed objects by property type.

```python
WE.GetObjectsByPortal(portal_id: int) -> list
```
Return indices of all objects that have `portal_id` assigned.

```python
WE.BatchMoveObjects(object_indices: list, delta_x: float, delta_y: float) -> bool
```
Move a list of objects by a delta, preserving the original selection afterwards.

---

## Validation helpers

```python
WE.ValidateMap() -> list
```
Returns a list of issue strings. Empty list = no problems detected.

```python
WE.ValidateObjectPlacement(object_index: int) -> list
```
Check whether an object's Z height is consistent with the terrain below it.

```python
WE.CheckTerrainConsistency() -> list
```
Sample corner heights of every terrain chunk and flag any that are outside a sane range.

```python
WE.ValidatePortals() -> list
```
Find portals that have fewer than two objects assigned (likely broken portals).

---

## Transaction / undo helpers

```python
WE.TransactionBegin() -> bool
WE.TransactionEnd() -> bool
```
Lightweight transaction depth tracking (no native undo grouping — call `BackupTerrain()` / `BackupObject()` before destructive operations).

```python
WE.SafeOperation(operation_func, *args, **kwargs)
```
Run `operation_func(*args, **kwargs)` inside a transaction. On exception, attempts to undo back to the transaction start and re-raises.

---

## Path / file helpers

```python
WE.NormalizePath(path: str) -> str
```
Normalise backslashes to forward slashes and collapse double slashes.

```python
WE.GetMapDirectory(map_name: str) -> str
```
Extract the directory component from a map name path.

```python
WE.EnsureDirectoryExists(path: str) -> bool
```
Create the directory (and all parents) if it does not already exist.

```python
WE.GetRelativePath(full_path: str, base_path: str) -> str
```
Return `full_path` relative to `base_path`.

---

## Performance helpers

```python
WE.TimeOperation(operation_func, *args, **kwargs) -> (result, elapsed_seconds)
```
Execute a function and return both its result and the wall-clock time taken.

```python
WE.ProgressCallback(callback_func) -> bool
```
Register a callback `callback_func(current, total, message)` to be called by operations that report progress.
