# Installation

## Prerequisites

- **Windows** (7 / 10 / 11, 32-bit or 64-bit)
- A M2 server resource tree accessible at `d:\ymir work\` (see [Drive setup](#drive-setup) below)
- `pack\Index` present if you want **pack loading** (optional)
- `pack\property` present if you want **property lookup from packs** (optional)

No installation wizard. Simply extract and run.

---

## Release file layout

After extracting the archive you should have exactly this structure:

```
gran212.dll
WorldEditorRemix.ini
WorldEditorRemix_MfcDebug_v49.exe
WorldEditorRemix_MfcDebug_v49.pdb
WorldEditorRemix_MfcRelease_v49.exe
WorldEditorRemix_MfcRelease_v49.pdb

lib38\
  libcrypto-1_1.dll
  libffi-7.dll
  libssl-1_1.dll
  LICENSE.txt
  pyexpat.pyd
  python.exe
  python3.dll
  python38.dll
  python38.zip
  python38._pth
  pythonw.exe
  select.pyd
  sqlite3.dll
  unicodedata.pyd
  vcruntime140.dll
  winsound.pyd

  WorldEditorRemix.py          ↝ your main script (edit this)
  WorldEditor_API.txt          ↝ API reference (read-only reference)

  WorldEditorRemixWrapper\
    __init__.py
    main.py
    terrain_operations.py
    ui.py
```

> **Do not move** `lib38\` relative to the `.exe` files. The embedded Python interpreter resolves its standard library from that folder.

---

## Drive setup

The editor hard-codes `d:\ymir work\` as its resource root. If your content lives elsewhere, use Windows `subst`:

```cmd
subst d: "C:\path\to\your\mt2stuff"
```

Add this to a batch file or your startup script so the drive is always available when you launch the editor.

Expected sub-folders under `d:\ymir work\`:

| Folder | Purpose |
|---|---|
| `environment\` | `.msenv` environment scripts |
| `effect\` | `.mse` effect scripts |
| `tree\` | Tree assets |
| `zone\` | Zone / building assets |
| `special\` | Special assets (excluded from map export) |

---

## Choosing which executable to run

| Executable | When to use |
|---|---|
| `WorldEditorRemix_MfcRelease_v49.exe` | Normal editing — best performance |
| `WorldEditorRemix_MfcDebug_v49.exe` | When you need crash dumps (written to `logs\`) |

The Debug build is noticeably slower on maps with heavy sphere/collision checks (e.g. `n_flame_dungeon`, `n_ice_dungeon`). Use Release for those.

---

## Crash dumps

When the Debug build crashes it writes a file to:

```
logs\WorldEditorRemix_{target}_{date}.dmp
```

You can open `.dmp` files in Visual Studio or WinDbg to inspect the call stack.

---

## Localization DLL (optional)

By default the binary uses its own internal English resources (`LOCALE = "EN"` in the `.ini`).

To load a translated resource DLL:

1. Build a resource-only DLL named `WorldEditorRemix_{LOCALE}.dll` following the [sample project](https://github.com/WorldEditorRemix/Translations).
2. Place the `.dll` next to the `.exe`.
3. Set `LOCALE = "XX"` in `WorldEditorRemix.ini` (e.g. `"DE"`, `"FR"`).

> Note: new versions may make previous DLLs incompatible.

---

## Python scripting setup

Python scripting is **enabled by default** (`ENABLE_PYTHON = true`). No additional installation is required — Python 3.8 is fully embedded in `lib38\`.

The entry point is `lib38\WorldEditorRemix.py`. Press **F5** while any of the four tab scenes (Map / Object / Effect / Fly) is active to execute it. See [SCRIPTING.md](SCRIPTING.md) for the full guide.
