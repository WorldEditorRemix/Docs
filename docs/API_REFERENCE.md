# WorldEditor API Reference

The `WorldEditor` module is exposed to Python scripts as a native C++ extension. Import it as:

```python
import WorldEditor as we
```

All functions return Python-native types. Integer return values of `0` / `1` typically represent `false` / `true`; the [wrapper](WRAPPER_REFERENCE.md) converts these to actual booleans for you.

---

## Map — General

```python
WorldEditor.IsMapReady() -> int
```
Returns `1` if a map is currently loaded and ready for editing.

```python
WorldEditor.IsMapLoaded() -> int
```
Returns `1` if a map file has been opened (may differ from "ready").

```python
WorldEditor.GetMapName() -> str
```
Returns the current map's name string.

```python
WorldEditor.GetMapType() -> int
```
Returns `MAPTYPE_OUTDOOR`, `MAPTYPE_INDOOR`, or `MAPTYPE_INVALID`.

```python
WorldEditor.GetMapID() -> int
WorldEditor.SetMapID(id: int) -> None
```
Get/set the numeric map ID.

```python
WorldEditor.GetMapBounds() -> (int, int, int, int)
```
Returns `(minX, minY, maxX, maxY)` world-space bounds.

```python
WorldEditor.GetBaseXY() -> (int, int)
```
Returns the base world coordinate `(X, Y)` of the map's origin.

```python
WorldEditor.GetTerrainCount() -> (int, int)
```
Returns `(countX, countY)` — number of terrain chunks along each axis.

```python
WorldEditor.GetSceneType() -> int
```
Returns one of `SCENE_MAP`, `SCENE_OBJECT`, `SCENE_EFFECT`, `SCENE_FLY`.

```python
WorldEditor.GetEnvironmentDataName() -> str
```
Returns the filename of the currently loaded `.msenv` environment script.

```python
WorldEditor.CloseMap() -> None
```
Closes the current map.

```python
WorldEditor.ReloadMap() -> int
```
Reload the current map from disk. Returns `1` on success.

```python
WorldEditor.PreloadAllTerrainsAndAreas() -> int
```
Force-load all terrain chunks and areas. Returns number loaded.

```python
WorldEditor.PreloadTerrainAndArea(terrainCoordX: int, terrainCoordY: int) -> None
```
Preload a specific terrain chunk at grid coordinate `(X, Y)`.

---

## Map — Save

```python
WorldEditor.SaveMap(mapName: str = None) -> int
WorldEditor.SaveMapProperty() -> int
WorldEditor.SaveMapSetting() -> int
WorldEditor.SaveTerrains() -> int
WorldEditor.SaveAreas() -> int
```
Save all or individual parts of the map. Each returns `1` on success.

---

## Map — Creation

```python
WorldEditor.NewMap(mapName: str) -> int
WorldEditor.CreateNewOutdoorMap() -> int
WorldEditor.SetNewMapName(name: str) -> None
WorldEditor.SetNewMapSizeX(size: int) -> None
WorldEditor.SetNewMapSizeY(size: int) -> None
```

---

## Camera

```python
WorldEditor.GetTargetPosition() -> (float, float)
WorldEditor.UpdateTargetPosition(x: int, y: int) -> None
```
Get/update the 2-D camera target in raw editor units.

```python
WorldEditor.GetCameraPosition() -> (float, float, float)
WorldEditor.GetCameraTarget() -> (float, float, float)
WorldEditor.SetCameraPosition(x: float, y: float, z: float) -> None
WorldEditor.SetCameraTarget(x: float, y: float, z: float) -> None
```
Full 3-D camera control.

---

## Terrain — Height (pixel-level)

```python
WorldEditor.GetHeightPixel(x: int, y: int) -> int
WorldEditor.DrawHeightPixel(x: int, y: int, height: int) -> None
```
Read/write a single height pixel by pixel coordinate.

```python
WorldEditor.DrawHeightRegion(x1: int, y1: int, x2: int, y2: int, height: int) -> None
WorldEditor.GetHeightRegion(x1: int, y1: int, x2: int, y2: int) -> list
```
Batch read/write an entire rectangular region of height pixels.

---

## Terrain — Height (world-space)

```python
WorldEditor.GetHeightAt(x: float, y: float) -> int
WorldEditor.SetHeightAt(x: float, y: float, height: int) -> None
```
Read/write terrain height at a world-space coordinate.

```python
WorldEditor.GetTerrainSize() -> int
```
Returns the size (in world units) of one terrain chunk.

---

## Terrain — Coordinate conversion

```python
WorldEditor.WorldToTerrainCoords(x: float, y: float) -> (int, int, int, int)
```
Convert world `(x, y)` → `(terrainNumX, terrainNumY, cellX, cellY)`.

```python
WorldEditor.TerrainToWorldCoords(terrainNumX: int, terrainNumY: int, cellX: int, cellY: int) -> (int, int)
```
Inverse of the above.

```python
WorldEditor.GetTerrainNum(x: float, y: float) -> int
WorldEditor.GetTerrainNumFromCoord(terrainNumX: int, terrainNumY: int) -> int
WorldEditor.GetEditTerrainNum() -> int
WorldEditor.GetEditTerrain() -> int
```

```python
WorldEditor.GetTerrainWorldBounds(terrainNumX: int, terrainNumY: int) -> (int, int, int, int)
```
World-space bounds `(x1, y1, x2, y2)` for a terrain chunk.

---

## Terrain — Textures

```python
WorldEditor.GetTerrainTextureFilename(texIndex: int) -> str
WorldEditor.GetTerrainUsedTextures(terrainNumX: int, terrainNumY: int) -> list
```

```python
WorldEditor.AddTerrainTexture(filename: str) -> int
WorldEditor.RemoveTerrainTexture(texNum: int) -> int
WorldEditor.ResetTerrainTexture() -> None
WorldEditor.RAW_ResetTextures() -> None
WorldEditor.ReloadTerrainTextures() -> None
WorldEditor.ReloadBuildingTexture() -> None
```

```python
WorldEditor.InitBaseTexture(mapName: str = None) -> int
```
Initialise the base texture for the current (or named) map.

```python
WorldEditor.GetTileValueAt(x: float, y: float) -> int
```
Returns the raw tile value at a world coordinate.

---

## Terrain — Texture brush

```python
WorldEditor.DrawTextureBrush(x: float, y: float, textureList: list, brushSize: int, erase: int, drawOnlyOnBlankTile: int) -> None
WorldEditor.SetTextureBrushVector(textureList: list) -> None
WorldEditor.SetInitTextureBrushVector(textureList: list) -> None
WorldEditor.SetEraseTexture(enabled: int) -> None
WorldEditor.SetDrawOnlyOnBlankTile(enabled: int) -> None
```

---

## Terrain — Height brush

```python
WorldEditor.DrawHeightBrush(x: float, y: float, brushShape: int, brushType: int, brushSize: int, brushStrength: int) -> None
WorldEditor.SetBrushShape(shape: int) -> None
WorldEditor.GetBrushShape() -> int
WorldEditor.SetBrushType(type: int) -> None
WorldEditor.GetBrushType() -> int
WorldEditor.SetBrushSize(size: int) -> None
WorldEditor.GetBrushSize() -> int
WorldEditor.SetBrushSizeY(size: int) -> None
WorldEditor.GetBrushSizeY() -> int
WorldEditor.SetBrushStrength(strength: int) -> None
WorldEditor.GetBrushStrength() -> int
WorldEditor.SetMaxBrushSize(size: int) -> None
WorldEditor.SetMaxBrushStrength(strength: int) -> None
```

---

## Terrain — Attributes

```python
WorldEditor.GetAttrAt(x: float, y: float) -> int
WorldEditor.SetAttrAt(x: float, y: float, attrFlag: int) -> None
WorldEditor.SetAttrAtWorld(x: float, y: float, attrFlag: int) -> None
WorldEditor.IsAttrOn(x: float, y: float, attrFlag: int) -> int
WorldEditor.DrawAttrBrush(x: float, y: float, attrFlag: int, brushSize: int, erase: int) -> None
WorldEditor.GetSelectedAttrFlag() -> int
WorldEditor.SetSelectedAttrFlag(attrFlag: int) -> None
WorldEditor.ResetToDefaultAttr() -> int
WorldEditor.SetEraseAttr(enabled: int) -> None
```

```python
WorldEditor.ApplyAttrToTerrainByTile(terrainNumX: int, terrainNumY: int, textureIds: list, attrFlag: int) -> int
```
Apply `attrFlag` to all attribute map pixels in the given terrain chunk whose tile uses one of `textureIds`. Returns number of pixels set.

```python
WorldEditor.RefreshAllTerrainAttrs() -> int
```
Refresh the attribute render for all loaded terrain chunks. Returns number refreshed.

---

## Water

```python
WorldEditor.GetWaterHeight(x: float, y: float) -> int
WorldEditor.DrawWaterBrush(x: float, y: float, waterHeight: int, brushSize: int, erase: int) -> None
WorldEditor.SetAllWaterHeight(height: int) -> None
WorldEditor.GetBrushWaterHeight() -> int
WorldEditor.SetBrushWaterHeight(height: int) -> None
WorldEditor.SetEraseWater(enabled: int) -> None
WorldEditor.PreviewEditWater() -> None
WorldEditor.GetNumWater() -> int
WorldEditor.SetNumWater(num: int) -> None
WorldEditor.GetWaterHeightArray() -> list
WorldEditor.SetWaterHeightArray(waterList: list) -> None
```

```python
WorldEditor.CleanupWaterWithoutAttr() -> (int, int, int, int, int, int, int)
```
Returns `(terrainsCleaned, terrainsChecked, terrainsWithWater, terrainsWithWaterAttr, terrainsWithWaterButNoAttr, totalWaterPixels, waterPixelsWithAttr)`.

---

## Objects — Query

```python
WorldEditor.GetObjectCount() -> int
WorldEditor.GetObjectDataCount() -> int
WorldEditor.GetObjectList() -> tuple
WorldEditor.GetObjectData(index: int) -> (float, float, float, int, float, float, float)
```
`GetObjectData` returns `(x, y, z, crc, yaw, pitch, roll)`.

```python
WorldEditor.GetLastSelectedObjectData() -> dict
```

---

## Objects — Selection

```python
WorldEditor.GetSelectedObjectCount() -> int
WorldEditor.GetSelectedObjectName() -> str
WorldEditor.SelectObject(index: int) -> None
WorldEditor.SelectObjectsByRect(x1: float, y1: float, x2: float, y2: float) -> int
WorldEditor.CancelSelect() -> None
WorldEditor.IsObjectSelected(index: int) -> int
WorldEditor.GetPickedObjectIndex() -> int
WorldEditor.IsSelected() -> int
WorldEditor.GetLastPickedAreaIndex() -> int
WorldEditor.SetLastPickedAreaIndex(index: int) -> None
WorldEditor.IsIntersectingSelection(index: int) -> int
WorldEditor.GetSelectionPivot() -> (float, float, float)
WorldEditor.AccumulateSelectionPivot() -> (float, float, float)
```

---

## Objects — Manipulation

```python
WorldEditor.InsertObject(x: float, y: float, z: float, yaw: float, pitch: float, roll: float, scale: int, crc: int) -> None
WorldEditor.MoveSelectedObject(x: float, y: float) -> None
WorldEditor.MoveSelectedObjectHeight(height: float) -> None
WorldEditor.ResetSelectedObjectHeightToTerrain() -> None
WorldEditor.RotateSelectedObject(yaw: float, pitch: float, roll: float) -> None
WorldEditor.SetSelectedObjectRotation(yaw: float, pitch: float, roll: float) -> None
WorldEditor.RotateSelectedObjectAroundPivot(rollDeg: float) -> None
WorldEditor.DeleteSelectedObject() -> None
WorldEditor.SetSelectedObjectName(name: str) -> None
```

```python
WorldEditor.DeleteObjectsByCRCList(crcList: list, scope: int = 1) -> int
```
Delete all objects whose CRC is in `crcList`. `scope`: `1` = current map. Returns number deleted.

```python
WorldEditor.ReplaceObjectsByCRC(sourceCrc: int, targetCrc: int, scope: int = 1) -> int
```
Replace all objects with `sourceCrc` with `targetCrc`. Returns number replaced.

```python
WorldEditor.PlaceGrassPropsByTexture(textureIds: list, grassCrcs: list, minDistanceMeters: float = 10.0) -> (int, int)
```
Scatter objects (by CRC list) over tiles matching `textureIds`. Returns `(inserted, skipped)`.

---

## Objects — Copy/Paste

```python
WorldEditor.CopySelectedObjects() -> None
WorldEditor.PasteObjects(x: float, y: float, z: float) -> None
WorldEditor.CopySelectedObjectsToClipboard() -> None
WorldEditor.PasteObjectsFromClipboard(x: float, y: float, z: float) -> None
```

---

## Objects — Portals

```python
WorldEditor.SetSelectedObjectPortalNumber(portalID: int) -> None
WorldEditor.DelSelectedObjectPortalNumber(portalID: int) -> None
WorldEditor.GetSelectedObjectPortalNumbers() -> list
WorldEditor.GetSelectedObjectPortalVectorRef() -> list
WorldEditor.EnablePortal(enabled: int) -> None
WorldEditor.CollectPortalNumber() -> list
WorldEditor.ClearSelectedPortalNumber() -> None
```

---

## Property

```python
WorldEditor.GetPropertyType(ext: int) -> str
WorldEditor.GetPropertyExtension(type: str) -> int
```

---

## Editing modes

```python
WorldEditor.SetHeightEditing(enabled: int) -> None
WorldEditor.IsHeightEditing() -> int
WorldEditor.SetTextureEditing(enabled: int) -> None
WorldEditor.IsTextureEditing() -> int
WorldEditor.SetWaterEditing(enabled: int) -> None
WorldEditor.IsWaterEditing() -> int
WorldEditor.SetAttrEditing(enabled: int) -> None
WorldEditor.IsAttrEditing() -> int
WorldEditor.SetMonsterAreaInfoEditing(enabled: int) -> None
WorldEditor.IsMonsterAreaInfoEditing() -> int
WorldEditor.EditingStart() -> None
WorldEditor.EditingEnd() -> None
WorldEditor.UpdateEditing() -> None
```

---

## Undo / Redo

```python
WorldEditor.BackupObject() -> None
WorldEditor.BackupTerrain() -> None
WorldEditor.BackupObjectCurrent() -> None
WorldEditor.BackupTerrainCurrent() -> None
WorldEditor.Undo() -> None
WorldEditor.Redo() -> None
WorldEditor.CanUndo() -> int
WorldEditor.CanRedo() -> int
WorldEditor.GetUndoCount() -> int
WorldEditor.GetRedoCount() -> int
WorldEditor.ClearUndoBuffer() -> None
```

---

## Refresh

```python
WorldEditor.RefreshArea() -> None
WorldEditor.RefreshSelectedInfo() -> None
WorldEditor.RefreshObjectHeight(x: float, y: float, halfSize: float) -> None
WorldEditor.RefreshTerrain() -> None
WorldEditor.RefreshEnvironment() -> None
WorldEditor.ArrangeTerrainHeight() -> None
WorldEditor.SetTerrainModified() -> None
WorldEditor.RefreshAllTerrainAttrs() -> int
```

---

## Shadow

```python
WorldEditor.UpdateTerrainShadowMap() -> None
WorldEditor.ReloadTerrainShadowTexture() -> None
WorldEditor.DestroyShadowTexture() -> None
WorldEditor.RecreateShadowTexture() -> None
WorldEditor.RenderToShadowMap() -> None
WorldEditor.RenderShadow() -> None
```

---

## Minimap / Atlas

```python
WorldEditor.SaveMiniMap() -> None
WorldEditor.SaveAtlas() -> None
WorldEditor.RenderMiniMap() -> None
```

---

## Environment / Lighting

```python
WorldEditor.LoadEnvironmentScript(filename: str) -> None
WorldEditor.SaveEnvironmentScript(filename: str) -> None
WorldEditor.InitializeEnvironmentData() -> None
WorldEditor.SetLightDirection(x: float, y: float, z: float) -> None
WorldEditor.SetLightDiffuseColor(r: float, g: float, b: float) -> None
WorldEditor.SetLightAmbientColor(r: float, g: float, b: float) -> None
WorldEditor.EnableLight(enabled: int) -> None
WorldEditor.SetFogColor(r: float, g: float, b: float) -> None
WorldEditor.SetFogNearDistance(distance: float) -> None
WorldEditor.SetFogFarDistance(distance: float) -> None
WorldEditor.EnableFog(enabled: int) -> None
WorldEditor.SetWindStrength(strength: float) -> None
WorldEditor.SetWindRandom(random: float) -> None
WorldEditor.EnableWind(enabled: int) -> None
WorldEditor.RefreshScreenFilter() -> None
WorldEditor.RefreshSkyBox() -> None
WorldEditor.RefreshLensFlare() -> None
```

---

## Skybox

```python
WorldEditor.SetSkyBoxTextureRenderMode(enabled: int) -> None
WorldEditor.IsSkyBoxTextureRenderMode() -> int
WorldEditor.SetSkyBoxFaceTexture(filename: str, faceIndex: int) -> None
WorldEditor.GetSkyBoxFaceTexture(faceIndex: int) -> str
WorldEditor.GetSkyBoxScale() -> (float, float, float)
WorldEditor.SetSkyBoxScale(x: float, y: float, z: float) -> None
WorldEditor.GetSkyBoxCloudScale() -> (float, float)
WorldEditor.SetSkyBoxCloudScale(x: float, y: float) -> None
WorldEditor.GetSkyBoxCloudTextureScale() -> (float, float)
WorldEditor.SetSkyBoxCloudTextureScale(x: float, y: float) -> None
WorldEditor.GetSkyBoxCloudSpeed() -> (float, float)
WorldEditor.SetSkyBoxCloudSpeed(x: float, y: float) -> None
WorldEditor.GetSkyBoxCloudHeight() -> float
WorldEditor.SetSkyBoxCloudHeight(height: float) -> None
WorldEditor.GetSkyBoxCloudTextureFileName() -> str
WorldEditor.SetSkyBoxCloudTextureFileName(filename: str) -> None
WorldEditor.GetSkyBoxGradientUpper() -> int
WorldEditor.SetSkyBoxGradientUpper(value: int) -> None
WorldEditor.GetSkyBoxGradientLower() -> int
WorldEditor.SetSkyBoxGradientLower(value: int) -> None
WorldEditor.InsertGradientUpper() -> None
WorldEditor.InsertGradientLower() -> None
WorldEditor.DeleteGradient(index: int) -> None
```

---

## Lens Flare

```python
WorldEditor.GetLensFlareEnable() -> int
WorldEditor.SetLensFlareEnable(enabled: int) -> None
WorldEditor.GetLensFlareBrightnessColor() -> (float, float, float, float)
WorldEditor.SetLensFlareBrightnessColor(r: float, g: float, b: float, a: float) -> None
WorldEditor.GetLensFlareMaxBrightness() -> float
WorldEditor.SetLensFlareMaxBrightness(brightness: float) -> None
WorldEditor.GetMainFlareEnable() -> int
WorldEditor.SetMainFlareEnable(enabled: int) -> None
WorldEditor.GetMainFlareTextureFileName() -> str
WorldEditor.SetMainFlareTextureFileName(filename: str) -> None
WorldEditor.GetMainFlareSize() -> float
WorldEditor.SetMainFlareSize(size: float) -> None
```

---

## Material / Filtering

```python
WorldEditor.SetMaterialDiffuseColor(r: float, g: float, b: float) -> None
WorldEditor.SetMaterialAmbientColor(r: float, g: float, b: float) -> None
WorldEditor.SetMaterialEmissiveColor(r: float, g: float, b: float) -> None
WorldEditor.SetFilteringColor(r: float, g: float, b: float) -> None
WorldEditor.SetFilteringAlpha(alpha: float) -> None
WorldEditor.SetFilteringAlphaSrc(src: int) -> None
WorldEditor.SetFilteringAlphaDest(dest: int) -> None
WorldEditor.EnableFiltering(enabled: int) -> None
```

---

## Monster Areas

```python
WorldEditor.ShowAllMonsterAreaInfo(enabled: int) -> None
WorldEditor.IsShowAllMonsterAreaInfo() -> int
WorldEditor.SaveMonsterAreaInfo() -> int
WorldEditor.GetMonsterAreaInfoCount() -> int
WorldEditor.GetSelectedMonsterAreaInfo() -> int
WorldEditor.SelectNextMonsterAreaInfo(forward: int = 1) -> None
WorldEditor.SelectMonsterAreaInfoStart() -> None
WorldEditor.SelectMonsterAreaInfoEnd() -> None
WorldEditor.IsSelectMonsterAreaInfoStarted() -> int
WorldEditor.AddNewMonsterAreaInfo(originX: int, originY: int, sizeX: int, sizeY: int, type: int, vid: int, count: int, dir: int) -> int
WorldEditor.RemoveMonsterAreaInfoPtr(index: int) -> int
WorldEditor.SetMonsterNames() -> None
```

---

## Cursor / Picking

```python
WorldEditor.SetEditingCursorPosition(x: float, y: float, z: float) -> None
WorldEditor.GetEditingCursorPosition() -> (float, float, float)
WorldEditor.GetPickingCoordinate() -> tuple
WorldEditor.GetPickingCoordinateWithRay(rayOriginX, rayOriginY, rayOriginZ, rayDirX, rayDirY, rayDirZ) -> tuple
```

---

## Auto-save / Backup

```python
WorldEditor.SetAutoSave(enabled: int) -> None
WorldEditor.IsAutoSave() -> int
WorldEditor.SetAutoBackup(enabled: int) -> None
WorldEditor.IsAutoBackup() -> int
WorldEditor.GetTimeSave() -> int
WorldEditor.SetNextTimeSave() -> None
```

---

## Misc utilities

```python
WorldEditor.SetBrushSize(size: int) -> None
WorldEditor.GetBrushSize() -> int
WorldEditor.SetBrushStrength(strength: int) -> None
WorldEditor.GetBrushStrength() -> int
WorldEditor.GetSplatValue() -> float
WorldEditor.SetSplatValue(value: float) -> None
WorldEditor.SetWireframe(enabled: int) -> None
WorldEditor.IsWireframe() -> int
WorldEditor.ToggleWireframe() -> None
WorldEditor.GetEditArea() -> (int, int, int, int, int, int)
WorldEditor.UpdateMapInfo() -> None
WorldEditor.UpdateBaseMapInfo(baseX: int, baseY: int) -> int
WorldEditor.AddSelectedAmbienceScale(addScale: int) -> None
WorldEditor.AddSelectedAmbienceMaxVolumeAreaPercentage(percentage: float) -> None
WorldEditor.RenderSelectedObject() -> None
WorldEditor.RenderAttr() -> None
WorldEditor.RenderObjectCollision() -> None
WorldEditor.RenderAccessorTerrain(renderMode: int, attrFlag: int) -> None
```

---

## Constants

### Scene types
```python
we.SCENE_MAP       # 0
we.SCENE_OBJECT    # 1
we.SCENE_EFFECT    # 2
we.SCENE_FLY       # 3
we.SCENE_MAX
```

### Map types
```python
we.MAPTYPE_INVALID
we.MAPTYPE_INDOOR
we.MAPTYPE_OUTDOOR
```

### Property types
```python
we.PROPERTY_TYPE_NONE
we.PROPERTY_TYPE_TREE
we.PROPERTY_TYPE_BUILDING
we.PROPERTY_TYPE_EFFECT
we.PROPERTY_TYPE_AMBIENCE
we.PROPERTY_TYPE_DUNGEON_BLOCK
we.PROPERTY_TYPE_MAX_NUM
```

### Brush shapes
```python
we.BRUSH_SHAPE_NONE
we.BRUSH_SHAPE_CIRCLE
we.BRUSH_SHAPE_SQUARE
we.BRUSH_SHAPE_MAX
```

### Brush types
```python
we.BRUSH_TYPE_NONE
we.BRUSH_TYPE_UP
we.BRUSH_TYPE_DOWN
we.BRUSH_TYPE_PLATEAU
we.BRUSH_TYPE_NOISE
we.BRUSH_TYPE_SMOOTH
we.BRUSH_TYPE_MAX
```

### Attribute flags
```python
we.ATTRIBUTE_BLOCK   # movement block
we.ATTRIBUTE_WATER   # water tile
we.ATTRIBUTE_BANPK   # no-PK zone
```

### Terrain constants
```python
we.TERRAIN_SIZE
we.TERRAIN_PATCHSIZE
we.TERRAIN_PATCHCOUNT
we.MAXTERRAINTEXTURES
we.MAPBASE
we.TILEMAP_RAW_XSIZE
we.TILEMAP_RAW_YSIZE
we.ATTRMAP_XSIZE
we.ATTRMAP_YSIZE
we.CELLSCALE_IN_METER
we.HALF_CELLSCALE_IN_METER
```

### Boundary load constants
```python
we.BOUNDARY_LOAD_INVALID
we.BOUNDARY_LOAD_NOBOUNDARY
we.BOUNDARY_LOAD_TOPLEFT
we.BOUNDARY_LOAD_TOP
we.BOUNDARY_LOAD_TOPRIGHT
we.BOUNDARY_LOAD_LEFT
we.BOUNDARY_LOAD_RIGHT
we.BOUNDARY_LOAD_BOTTOMLEFT
we.BOUNDARY_LOAD_BOTTOM
we.BOUNDARY_LOAD_BOTTOMRIGHT
we.BOUNDARY_LOAD_ALLBOUNDARY
```

### Monster area
```python
we.MONSTERAREAINFOTYPE_INVALID
we.MONSTERAREAINFOTYPE_MONSTER
we.MONSTERAREAINFOTYPE_GROUP

we.MONSTER_DIR_RANDOM
we.MONSTER_DIR_NORTH
we.MONSTER_DIR_NORTHEAST
we.MONSTER_DIR_EAST
we.MONSTER_DIR_SOUTHEAST
we.MONSTER_DIR_SOUTH
we.MONSTER_DIR_SOUTHWEST
we.MONSTER_DIR_WEST
we.MONSTER_DIR_NORTHWEST
```

### Misc
```python
we.AROUND_AREA_NUM
we.MAX_WATER_NUM
we.ENV_DIRLIGHT_BACKGROUND
```
