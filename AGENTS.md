# openQ4-game-awakening archival maintenance guide

Game-code development has moved to [openQ4](https://github.com/themuffinator/openQ4). This repository is
retained as a historical archive. Apply new SDK/game changes in openQ4’s
`src/game/` and `src/mpgame/`; Awakening SP additions belong in
`src/game/awakening/` and are scoped to active `q4xbase` content. Do not revive
the companion build, source stage or expansion multiplayer module.

Only archival documentation maintenance belongs here. Keep README and
[MIGRATION.md](MIGRATION.md) aligned with openQ4’s component licences and
provenance. Preserve the original source, licence notices and credits.

## Historical guide (before consolidation)

The metadata, procedures and cross-repository rules below describe the frozen
pre-migration project. They do not override the canonical locations above.

**Project Metadata**
- Name: openQ4-game-awakening
- Author: themuffinator
- Company: DarkMatter Productions
- Version: 0.1.0 (see `layer.json`)
- Website: `www.darkmatter-quake.com`
- Repository: `https://github.com/themuffinator/openQ4-game-awakening`
- Companion engine repo (local): `E:\Repositories\openQ4`
- Companion game-library repo (local): `E:\Repositories\openQ4-game`

**Goals**
- Make the unreleased Quake 4 expansion, *The Awakening* (`q4xbase`), playable on openQ4 with game code written for openQ4.
- Stay a mod of the current openQ4 game: every q4xbase module is the current openQ4-game plus this layer, never a copy of it.
- Keep the expansion's content working as authored: its spawnclasses, weapon classes, script events, frame commands and def keys.

**The Layer Contract**
- This repository is a game-library *layer* (`openQ4/tools/build/game_layer.py`). `layer.json` names the layer (`awakening`), the game directory it serves (`q4xbase`) and its source trees.
- A layer only adds files. openQ4 stages `src/shared` into both `src/game/awakening/` and `src/mpgame/awakening/`, `src/game` into the first and `src/mpgame` into the second, next to an unchanged openQ4-game snapshot. The q4xbase modules link the same openQ4-game core libraries as the baseoq4 modules plus the layer's objects.
- Layer sources include base headers the way base sources do: a file at the top of a layer tree uses `#include "../Game_local.h"`, one level deeper `#include "../../Game_local.h"`. `precompiled.h` is forced by the build; do not include it.
- Never copy or fork an openQ4-game file into this repository. When the expansion needs behaviour inside a stock class, prefer, in order:
  1. a subclass registered through openQ4-game's class-substitution extension point (the stock spawnclass keeps working in content);
  2. a generic, documented extension point in openQ4-game (land it there first, in both `src/game` and `src/mpgame`);
  3. a generic compatibility feature in openQ4-game when the expansion content uses a script event, def key or state name on a stock class (for example `resetTalkCount` on `idAI`).
- `src/shared` code must compile against both game trees. Guard real differences with `#ifdef GAME_MPAPI`.
- Script events declared by `q4xbase/scripts/events.script` must exist in both modules (see `src/shared/ScriptEvents.*`); the script compiler refuses an unknown one before any map loads.

**Clean-Room Boundary**
- openQ4 does not load legacy Quake 4 game code, including the expansion's leaked `gamex86.dll`. This repository is new code written for openQ4.
- The leaked binaries, their decompilation and the GPL engine reconstruction are behaviour references only: use them to learn class and member names, spawn/def keys, state names, constants and what a feature does. Do not paste decompiled or reconstructed code into this repository, and do not copy code from `../quake4-awakening/` (it is GPL; this repository follows the Quake 4 SDK EULA like openQ4-game).
- Describe what a class must do in prose (behaviour, def keys, states, timings) in the comment block at the top of its source file, then implement from that description in openQ4 style.

**Build And Validation**
- Build through openQ4: `powershell -ExecutionPolicy Bypass -File E:\Repositories\openQ4\tools\build\meson_setup.ps1 compile -C E:\Repositories\openQ4\builddir`. The `awakening` meson feature (auto) finds this repository at `../openQ4-game-awakening` or `OPENQ4_AWAKENING_REPO`.
- The layer is staged at configure time into `openQ4/.tmp/gamelibs_stage_awakening/`. `meson_setup.ps1` re-stages when a layer file changes; a raw `ninja` does not.
- Outputs: `builddir/q4xbase/{game-sp_x64.dll, game-mp_x64.dll, mod.json}` and, after `meson install`, `.install/q4xbase/`.
- Runtime: the expansion content must be in `<Quake 4 install>/q4xbase/`. Launch the staged client with `+set fs_game q4xbase`; openQ4 adds its own `baseoq4` runtime beneath every mod automatically.
- Validate in-game (load an expansion map, reach gameplay) and read `fs_savepath\q4xbase\logs\openq4.log`; main-menu startup is not validation.
- Use `openQ4/.tmp/` for temporary files.

**Local References (Not In Repo)**
- Expansion drop (content and leaked binaries): `E:\Games\Quake_4_Alpha-main` (`q4xbase/`, `quake4.exe`, `quake4.pdb`).
- GPL reconstruction of the expansion engine from its PDB: `E:\Repositories\quake4-awakening\src`.
- Expansion support audit: `openQ4/docs/dev/q4x-awakening-support-audit.md`.
- Integration plan and status: `openQ4/docs/dev/plans/q4x-awakening.md`.
- Quake 4 SDK: `E:\_SOURCE\_CODE\Quake4-1.4.2-SDK`.
- Quake 4 install (Steam, 1.4.2): `E:\SteamLibrary\steamapps\common\Quake 4`.

**Upstream Credits**
- Raven Software and Ritual Entertainment, authors of Quake 4 and The Awakening.
- id Software.
- Justin Marshall, for recovering and publishing the expansion drop and the engine reconstruction used as a reference.
- openQ4 contributors.
