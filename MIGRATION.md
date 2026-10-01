# Source migration to openQ4

Migration date: 1 October 2026. Canonical development continues in
[openQ4](https://github.com/themuffinator/openQ4); this repository is a historical archive.

## Preserved source and history

- Last source revision imported from this repository: `c49842986c0a519feeebc68d78e6f5509d99c3a7`.
- Import merge: [`53a9bf28`](https://github.com/themuffinator/openQ4/commit/53a9bf2823e3123b8e1c2e19e7ff2dfbda3379bb). Both
  companion histories remain reachable through the merge’s parent commits.
- New canonical location: `src/game/awakening/`.
- Original notices, SDK EULA and upstream attribution remain intact. See the
  [source provenance record](https://github.com/themuffinator/openQ4/blob/main/docs/dev/game-source-provenance.md).

## Current build and runtime

Build one openQ4 checkout using its [Meson wrappers](https://github.com/themuffinator/openQ4/blob/main/BUILDING.md).
The engine compiles canonical game sources directly; companion checkouts, source
staging and separate Awakening game modules are no longer required. Engine-only
and game-only configurations use the same checkout and interfaces.

Quake 4 and The Awakening share `baseoq4`’s SP module. **Single Player → Campaign**
discovers user-supplied Awakening content under `q4xbase`, with an explicit
`fs_awakeningpath` available for content stored elsewhere. Discovery does not
mount expansion overrides into the base campaign. Campaign changes reload their
content and keep separate `baseoq4`/`q4xbase` saves and settings. Returning to
Quake 4, Arena or multiplayer removes the expansion mount. Old Awakening DLLs
are ignored; its multiplayer layer is excluded. No expansion assets are shipped.

See the [campaign guide](https://github.com/themuffinator/openQ4/blob/main/docs/user/campaigns.md) for
installation, alpha-content limitations and troubleshooting. Report current
bugs and submit changes to [openQ4’s issue tracker](https://github.com/themuffinator/openQ4/issues).

## Licensing

This move preserves component terms. The engine remains GPL-covered with its
accompanying notices and Additional Terms; SDK-derived game code retains the
Quake 4 SDK EULA. The [licensing inventory](https://github.com/themuffinator/openQ4/blob/main/LICENSING.md)
does not assert that source consolidation settles linked-distribution licence
compatibility or provides new rights-holder permission.
