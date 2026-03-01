# Changelog

All releases are available on the [GitHub Releases tab](https://github.com/WorldEditorRemix/Releases/releases) as prebuilt `.exe` packages. Each release includes both the `_MfcRelease` and `_MfcDebug` builds alongside the updated Python scripts.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [v49] - Latest

### Added
- Added Automatic Grass placement via python script
- Added Automatic Grass removal via python script
- Added Automatic Block Attr on stoneXX texture areas, and BanPK on tileXX texture areas via python script
- Added Automatic Object Replacement id1 -> id2 via python script
- Added Automatic Cleanup of Water if Water Attr is missing.
- Added Rotation from common pivot of selected objects with MouseWheel+8 shortcut.
- Added side window opening when F5 is pressed via python script

### Fixed
- Fixed v48 create map VisibleTerrain regression active by default.
- Fixed v48 manifest crash regression.

### Changed
- Extended with Python38
- Removed gate textures from Export Map skiplist.
- Adjusted Undo/Redo teleport coordinates.

---

## [v48]

### Added
- created object search
- implemented attr/water undo/redo
- created the following options too ENABLE_AUTOGEN_PROPERTY, AUTOGEN_PROPERTY_EFFECT, NEW_OBJECT_ID_FROM_CRC

### Fixed
- [V48 TestFix002] Fix startup crash on very few PCs
- fixed wireframe checkbox refresh

### Changed
- [V48 TestFix002] Improved the export map by force-including any potential used property's pointed object (probably not needed)
- [V48 TestFix002] Removed from the export map blacklist some gate's effects' textures: gatein.dds, detail_lens.dds, upline.dds
- refactored attr slider to 4 new attr checkboxes
- removed ATTR_SLIDER_REMOVAL option
- converted korean to english dialog encoding
- new AUTO_SAVE option to auto-toggle the relative checkbox
- the ScanNewObject button can now autogenerate the detected objects, and separately refresh the property list too.

---

## [v47]

### Added
- created close and reload map buttons
- created auto_backup map feature which is called before any save
- created export map feature which exports every loaded file for an easy release (all included except few files from d:/ymir work/special/)
- created quick environment list with real time update if d:/ymir work/environment/ .msenv list changes
- created "adjust all water height" button to change all the water height to your current one (it may work only on adjacent areas yet)
- enabled the water height edit field as editable
- created a new "water height pick" button to pick the water height from current position
- created a new "scan new objects" button to list all the new objects/trees from d:/ymir work/tree|zone/ that have no property/ and save a list of it

### Fixed
- fixed skybox cleanup on map load
- fixed weird gripper docking on menu interface
- fixed tree property rotation (check etc/cpp/ folder for the launcher implementation)
- fixed some crashes when loading some trees
- fixed graphs on different window resolution than 1024x768
- fixed camera position when switching tabs (MAP/OBJECT/ANIMATION/FLY)
- fixed the TerrainVisible map option based on the "Terrain Out" checkbox
- fixed lower gradient button crash if upper gradients were missing

---

## [v46]

### Added
- implemented new hierarchy object list of the current area (ctrl+left mouse click for multiselection, right mouse click for teleport to object)
- added new option INTERSECT_OBJECT_PICKING to allow intersect object picking (selecting objects inside other objects and moving them around)
- added new option ENABLE_OBJECT_HIERARCHY to disable the hierarchy
- added new button to unselect all the object selections
- added green box selection for trees and effects too, they can move like the objects do now

### Fixed
- fixed height bias load precision
- fixed wireframe render on attr tab
- fixed selected monsterarea render if the option was toggled off
- fixed pick selection of objects from adjacent areas
- fixed speedtree memory corruption if the texture files are missing

### Changed
- limited pickable area of ambience effects

---

## [v45]

### Added
- created new button to reset the selected objects' rotation
- created new button to reset the selected objects' height
- implemented Ctrl+C Ctrl+V feature to copy paste objects across maps/we instances via textual clipboard (you can save this - text, reuse it, share it, etc)
- created new ground compass texture (see minimal-pack archive for the texture)
- created new ruler compass (mali addition)
- added ENABLE_GR2_ANIMATION option for object animation (mali addition)
- added MAI_IMAGE_WITHOUT_MONSTER_AREA_INFO
- added MINIMAP_EFFECT_SNAPSHOT option to allow effects in minimaps with f6 (mali addition)

### Fixed
- fixed crash when displaying the preview of a missing terrain texture
- fixed colored main character' shadow
- fixed crash on server_attr gen / atlas map gen due to devIL initialization
- fixed a rendering about the fix (mali addition)
- fixed tga/dds load header mismatch

### Changed
- refactored object rotation and relative sliders (yaw pitch roll)
- removed logbox of background_stone.gr2
- extended the weremix with python3 + initial script (i disabled this one, i will improve it in the future)

---

## [v44]

### Changed
- Some icons have been replaced

---

## [v43]

### Fixed
- Fixed saving the .msm files having some Group keywords missing

---

## [v42]

### Added
- Implemented the box collision handling (it requires additional code if you use .mde with boxes for launcher)

### Fixed
- Fixed the saving of msf files in the FlyTab

---

## [v40]

### Changed
- New config option LOCALE
- You can now translate the program UI (and also more in the future) by generating the relative WorldEditorRemix_{}.dll file.
- To translate the program: https://github.com/WorldEditorRemix/Translations

---

## [v39]

### Added
- Auto-Save option added

### Fixed
- Fixed some crashes like in save server attr button

### Changed
- New config option AUTO_SAVE_TIME

---

## [v38]

### Fixed
- Fixed the height brush regression from v3

---

## [v37]

### Added
- Added Config option CENTERED_WINDOW to center the program window when opened
- Added global/local position in the f11 info board (https://gyazo.com/0205078025702dd58e523b6ffeebce82)

### Fixed
- Fixed moving with WASD if ctrl was pressed (e.g. ctrl+s)
- Fixed when switching height brushes, the eraser remained active but not the relative button

---

## [v36]

### Fixed
- Fixed displayed script name and version in environment tab

---

## [v35]

### Added
- Enabled large address aware

### Fixed
- Fixed selected object text area

### Changed
- "About box" a little bit larger

---

## [v34]

### Changed
- This can be considered a minor update.
- I compiled the project with /c++latest and other optimized flags.

---

## [v33]

### Added
- Enabled win10 style https://imgur.com/a/4Av7cM3 (it looks stable)

### Fixed
- Fixed crash if TimeEventAlpha is empty inside the .mse
- Fixed lens flare

---

## [v32]

### Changed
- This is just a minor update. I simply disabled the DPI-awareness, so the app won't scale weirdly if you set your windows's scaling (over 150% kinda) at all.
- Don't forget that, if you get crashes/asserts, you'll get a logs/{name}.dmp file. You can upload it and send it to me for debugging.

---

## [v31]

### Added
- Removed useless Toolbar options and added Redo https://imgur.com/a/qKHxBY2
- New config option RENDER_CURSOR_COLOR_{R|G|B}
- New config option OBJECT_HEIGHT_MAX
- New config option OBJECT_HEIGHT_SLIDER_MAX
- New config option ATTR_SLIDER_REMOVAL

### Changed
- Removed useless File Dialog options

---

## [v30]

### Fixed
- Fixed object attachment in Model Tab
- Fixed the names of the elements in Effect Tab
- Fixed crash when saving a .mse script with no mesh model
- Fixed crash when inserting a lower gradient

---

## [v29]

### Fixed
- Fixed multi-object-selection box display
- Fixed creating new property folders in root tree

### Changed
- Load from PACK first only if pack/property exists, otherwise it loads from FILE first [v28]

---

## [v28]

### Changed
- Load from PACK first only if pack/property exists, otherwise it loads from FILE first

---

## [v27]

### Fixed
- Fixed multi-object-selection crash
- Fixed the missing diffuse lighting in the objects
- Fixed a crash when previewing a missing texture

### Changed
- New config options VIEW_OBJECT_LIGHTING, MOB_PROTO_PATH, VIEW_MONSTER_AREA_INFO
- Default texture displaying count has been increased from 8 to 255 (aka lshift+0 by default)

---

## [v26]

### Added
- Added water texture change in msenv (you need additional client c++ code; ignore it for now)
- Added wind strength change in msenv (for speedtree; you need additional client c++ code; ignore it for now)
- Added generation of `logs/WorldEditorRemix_{target}_{date}.dmp` in case of crashes
- Added config flag SERVERATTR_REMOVE_WEIRD_FLAGS

### Fixed
- Fixed locale/ymir/mob_proto load (autodetect struct)
- Fixed <map>/regen.txt save/load/edit (very nice for "m" regens)
- Fixed ./group.txt load
- Fixed some crashes

### Changed
- The WorldEditorRemix v26 is out! (after 5 years)
- The v24 was compiled with vs2010, but the v26 is compiled with vs2019. If you find any regressions, report me all of them.
- Updated some icons (logo, menus)
- Updated granny to 2.11
- Changed WorldEditor.txt config file to WorldEditorRemix.ini
- Load from PACK is available if property/ is missing and pack/property is present! Be sure pack/Index exists! (textureset from PACK ignores textureset/ if the relative pack exists)

---

## [v25]

### Changed
- Nothing different from v24 except being compiled on vs2019 instead of vs2010


