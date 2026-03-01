# WorldEditorRemix

[![Latest Release](https://img.shields.io/github/v/release/WorldEditorRemix/Releases?label=latest&color=f5a623)](https://github.com/WorldEditorRemix/Releases/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/WorldEditorRemix/Releases/total?color=3ddc84)](https://github.com/WorldEditorRemix/Releases/releases)
[![Issues](https://img.shields.io/github/issues/WorldEditorRemix/Releases)](https://github.com/WorldEditorRemix/Releases/issues)

A heavily patched and extended replacement for the traditional M2 WorldEditor binary, focused on stability, usability, and Python scripting power.

> **Closed-source release.** You receive the compiled `.exe` plus a set of editable Python scripts. The source code is not distributed.

---

## What is it?

WorldEditorRemix is a drop-in replacement for `WorldEditor_en.exe`. It ships as two executables built from the same codebase:

| File | Use |
|---|---|
| `WorldEditorRemix_MfcRelease_v49.exe` | Day-to-day work — fast, no overhead |
| `WorldEditorRemix_MfcDebug_v49.exe` | Debugging sessions — generates `.dmp` crash files |

Both are 100% English, large-address-aware, compiled with C++latest and optimised flags, and DPI-awareness disabled so they render correctly on high-DPI monitors.

---

## Key highlights

- **100% translated** — no Korean strings remain in the UI
- **Granny 2.12** runtime (compatible with standard m2 assets)
- **Python 3.8 embedded** scripting — press **F5** to run your script live inside the editor
- **WASD + arrow keys** for camera movement (diagonal movement, async)
- **Dozens of crash fixes** — buffer overflows, assertion failures, atlas saves, regen load/save, etc.
- **New tools** — server_attr generator, auto-backup, map export, hierarchy object list, water-height pick, object search, property auto-generation
- **Undo/Redo** for terrain and object operations
- **Pack loading** — reads from `pack/Index` if present
- **Configurable** via `WorldEditorRemix.ini`

For the full feature list see the [Configuration guide](CONFIGURATION.md) and the [Known Issues page](KNOWN_ISSUES.md).

---

## Download

**[⬇ Download latest release](https://github.com/WorldEditorRemix/Releases/releases/latest)** — prebuilt `.exe` package, both Release and Debug builds included.

Each release on the [Releases tab](https://github.com/WorldEditorRemix/Releases/releases) contains:
- `WorldEditorRemix_MfcRelease_vXX.exe` + `.pdb`
- `WorldEditorRemix_MfcDebug_vXX.exe` + `.pdb`
- `gran212.dll`
- `WorldEditorRemix.ini`
- `lib38\` — Python 3.8 runtime + editable scripts

See [CHANGELOG.md](CHANGELOG.md) for what changed in each version.

---

## Quick start

1. Extract the release archive so that `WorldEditorRemix_MfcRelease_v49.exe` sits alongside the `lib38\` folder and `WorldEditorRemix.ini`. See [Installation](INSTALLATION.md) for the exact layout.
2. Mount your content drive. The editor expects assets at `d:\ymir work\`. The simplest way on a modern machine:
   ```
   subst d: "C:\mt2stuff"
   ```
3. Launch `WorldEditorRemix_MfcRelease_v49.exe`.
4. Open or create a map via **File → Open Map**.
5. Press **F5** at any time while a map scene is active to execute `lib38\WorldEditorRemix.py`.

---

## Documentation index

| Document | Contents |
|---|---|
| [INSTALLATION.md](INSTALLATION.md) | File layout, prerequisites, drive setup |
| [CONFIGURATION.md](CONFIGURATION.md) | Every `WorldEditorRemix.ini` option |
| [SHORTCUTS.md](SHORTCUTS.md) | Keyboard and mouse shortcut reference |
| [SCRIPTING.md](SCRIPTING.md) | Python scripting guide — entry point, modules, workflow |
| [API_REFERENCE.md](API_REFERENCE.md) | Full `WorldEditor.*` function and constant reference |
| [WRAPPER_REFERENCE.md](WRAPPER_REFERENCE.md) | `WorldEditorWrapper` helper class |
| [TERRAIN_OPERATIONS.md](TERRAIN_OPERATIONS.md) | `terrain_operations` high-level helpers |
| [UI.md](UI.md) | Built-in overlay UI panel |
| [KNOWN_ISSUES.md](KNOWN_ISSUES.md) | Known limitations and workarounds |
| [TROUBLESHOOTING.md](TROUBLESHOOTING.md) | Common problems and solutions |
| [CHANGELOG.md](CHANGELOG.md) | Full version history |
| [ROADMAP.md](ROADMAP.md) | Planned features, in-progress work, and won't-fix decisions |

---

## Reporting issues & feature requests

Use the [GitHub Issues tab](https://github.com/WorldEditorRemix/Releases/issues) with the appropriate template:

- **🐛 Bug Report** — crashes, incorrect behaviour, regressions. Attach the `.dmp` from `logs/` if available.
- **✨ Feature Request** — new tools, API functions, or config options.

Before filing, check [KNOWN_ISSUES.md](KNOWN_ISSUES.md) and [ROADMAP.md](ROADMAP.md) — your item may already be tracked.

> This project does not accept code pull requests. See [ROADMAP.md](ROADMAP.md#notes-for-contributors) for other ways to contribute.
