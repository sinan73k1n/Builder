# Builder

A stacking game for Android, built in Unity. Published on Google Play.

Blocks swing past overhead; you drop them one at a time and try to build the tallest structure you can.
Physics decides the rest — a block landing off-centre shifts the tower, and a tower that leans far enough
comes down.

> **Play Store:** the published build lives on my
> [developer page](https://play.google.com/store/apps/dev?id=5968286876093646143).

---

## What is worth reading

The scripts I wrote are under [`Assets/_SCRIPTS/`](Assets/_SCRIPTS); the rest of the project is scenes,
sprites, fonts and imported packages.

| File | Lines | What it does |
|---|---:|---|
| `Game/GameManager.cs` | 213 | The whole round: spawning the next block, dropping it, tracking the stack, scoring, and the bomb that clears blocks when the tower gets stuck |
| `Game/Cube.cs` | 131 | A single block — placement, material, and the faces that report contact |
| `UI/CANVAS_UI.cs` | 77 | In-game HUD |
| `Statics/KYTGameFree.cs` | 57 | Persistent state: best score, unlocked block materials |
| `Game/FaceSensor.cs` + `SensorCollapse.cs` | 44 | Collapse detection — the cubes carry sensors on their faces rather than the manager polling every block each frame |
| `SoundPack/SoundBox.cs` | 41 | Audio playback by enum name instead of string lookups |
| `_PREFABS/Game/CameraLooks.cs` | 36 | Camera follows the top of the tower as it grows |

### How collapse is detected

The obvious way to check whether a tower is still standing is to ask every block, every frame. This project
does the opposite: each cube carries sensors on its faces (`FaceSensor`), and a cube reports upward only when
its contact state actually changes. `SensorCollapse` turns those reports into a single "the stack broke"
event. Work happens when something happens, not sixty times a second.

Enums do the naming throughout — `NameOfCubeMaterial`, `NamesOfSound`, `TagsOfGO`, `Scenes`. A typo in a
material or scene name becomes a compile error instead of a silent null at runtime.

---

## Building it

```
Unity 2022.3 LTS or newer, Android build support
```

Open the folder as a Unity project and let it import. The Android build needs a signing keystore, which is
**not** in this repository — set your own under *Project Settings → Player → Publishing Settings*.

## Layout

```
Assets/_SCRIPTS/      gameplay, UI, sound, persistent state
Assets/_PREFABS/      prefabs and the scripts attached to them
Assets/_FONTS/        fonts used by the UI
Assets/TextMesh Pro/  imported package, not my code
ProjectSettings/      Unity project configuration
```

## Notes

Some class names are shouted (`CANVAS_UI`, `STCube`) and some are English-Turkish mixed — that is how the
project was written, and renaming it now would only add noise to a shipped game's history.

The Photoshop source of the icon is not versioned here: it is a 50 MB file that nothing in the build needs.

The art, sound and the published build are mine and are not offered for reuse. The code is here to be read.
