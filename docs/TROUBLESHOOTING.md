# Troubleshooting

## The editor won't start

**`d:` drive not found / resource errors at launch**
- Make sure your content is mounted at `d:\ymir work\`. Run `subst d: "C:\your\content"` from a command prompt **before** launching.

**Missing DLL errors**
- Ensure `gran212.dll` is next to the `.exe`.
- Ensure the entire `lib38\` folder is present and its contents are intact. Do not move individual files out.

**"Python not enabled" or scripting does nothing**
- Check `WorldEditorRemix.ini`: `ENABLE_PYTHON = true`.
- Ensure the `lib38\` folder is present in the same directory as the `.exe`.

---

## Map won't load / crashes on load

**Crash on map load**
- Use the Debug build (`_MfcDebug`) to generate a `.dmp` in `logs\`. Open it in WinDbg or Visual Studio to identify the crash location.
- Check that `pack\Index` exists if you have a `pack\` folder (the editor attempts to open it).

**Textureset load fails silently**
- After a failed textureset load, the old filename stays in the UI (Known Issue #14). Close and reopen the map.

**`regen.txt` not loading**
- Confirm the file is in `<map>/regen.txt`. The editor auto-detects common `mob_proto` structures; set `MOB_PROTO_PATH` in `.ini` if your path differs.

---

## Performance

**Editor is very slow**
- Use `WorldEditorRemix_MfcRelease_v49.exe` for normal editing. The Debug build has significant overhead.
- On maps like `n_flame_dungeon` / `n_ice_dungeon`, the Debug build is especially slow (Known Issue #4, partially fixed in v44). Switch to Release for these.

**Minimap / atlas generation is slow**
- This is expected — disable `MINIMAP_EFFECT_SNAPSHOT` and `MAI_IMAGE_WITHOUT_MONSTER_AREA_INFO` in `.ini` to speed it up.

---

## Terrain / attribute issues

**Attributes not showing on terrain**
- Make sure you are on the **Attr Tab**.
- Press **F11** to toggle wireframe if the terrain appears blank (Known Issue #18, fixed in v46).
- Run `we.RefreshAllTerrainAttrs()` from a script if painting attributes programmatically.

**Only 8 textures visible per region**
- Press `LShift+0` to raise the limit to 255. This is a rendering cap (Known Issue #12), not a data limit.

**MDATR height not updating after moving a building**
- Press **H** to manually refresh MDATR heights (Known Issue #11).

---

## Water issues

**Water brush disappears near waterwheel effects**
- Known Issue #19. No workaround currently — avoid having the camera capture the waterwheel effect while painting.

**Water exists where it shouldn't**
- Run `terrain_operations.CleanupWaterWithoutAttr()` from a script or the UI panel to remove water pixels that lack `ATTRIBUTE_WATER`.

---

## Object / scripting issues

**F5 does nothing / Python not executing**
- The scene must be active (Map / Object / Effect / Fly tab). The script won't run from the loading screen.
- Check the console output (Debug build) for Python import errors.

**`IsMapReady()` returns `False` in script**
- A map must be fully loaded before most terrain and object API calls work. Call `we.IsMapReady()` and gate your logic behind it.

**Script changes have no effect**
- The file is re-read from disk on every F5 press. Make sure you saved the file before pressing F5.

**UI panel doesn't appear**
- Check the console for `"Failed to create UI: ..."` output. This usually means a Win32 API error (rare on modern Windows).
- If a previous panel HWND is stale, run `from WorldEditorRemixWrapper import cleanup_ui; cleanup_ui()` and then `show_ui()` again.

**`DeleteObjectsByCRCList` / `ReplaceObjectsByCRC` affect wrong objects**
- Verify your CRC values. CRCs are computed from the **normalised** object filename (forward slashes, lower-case). Use the object hierarchy list to confirm which objects have which CRCs.

---

## Pack loading

**Objects list is empty when loading from pack**
- The object placer reads `property\` from the filesystem, not from `pack\property\`. Extract the property folder if you want the placer to populate correctly (Known Issue #16).

**Pack textureset not loading**
- If the textureset file exists in both `pack\` and as a loose file, the pack version takes priority. If neither loads, double-check `pack\Index` integrity.

---

## Skybox / environment

**Skybox not fully displayed**
- Add the **top** face texture first, then the remaining faces (Known Issue #21).

**Environment not clearing when switching maps**
- Fixed since a previous version. If still occurring, use **File → Close Map** before opening another.

---

## Crash dumps

All crashes in the Debug build write a `.dmp` to:

```
logs\WorldEditorRemix_{target}_{date}.dmp
```

Open with: **Visual Studio → File → Open → Crash Dump** or **WinDbg → File → Open Crash Dump**.

If you are reporting a bug to the community threads, attach the `.dmp` and describe what you were doing when the crash occurred.
