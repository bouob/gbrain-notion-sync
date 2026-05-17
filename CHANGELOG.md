# Changelog

All notable changes to this project will be documented in this file.

## [0.1.2](https://github.com/bouob/gbrain-notion-sync/compare/v0.1.1...v0.1.2) (2026-05-17)


### Bug Fixes

* add /notion-sync argument-hint for inline autocomplete ([#8](https://github.com/bouob/gbrain-notion-sync/issues/8)) ([410ea50](https://github.com/bouob/gbrain-notion-sync/commit/410ea50052fe7a05f511679f89409c069c47dd67))

## [0.1.1](https://github.com/bouob/gbrain-notion-sync/compare/v0.1.0...v0.1.1) (2026-05-17)


### Features

* add /notion-sync init for interactive onboarding ([#6](https://github.com/bouob/gbrain-notion-sync/issues/6)) ([dc728a2](https://github.com/bouob/gbrain-notion-sync/commit/dc728a2782656b14fd9445273ecc4347c975a7b2))

## 0.1.0 (2026-05-17)


### Features

* **gbrain-notion-sync:** sync updates 2026-05-17 ([#1](https://github.com/bouob/gbrain-notion-sync/issues/1)) ([784ead6](https://github.com/bouob/gbrain-notion-sync/commit/784ead6e7e01a3f181bbe39400180661e88f0dca))


### Miscellaneous Chores

* pin initial release to v0.1.0 ([#5](https://github.com/bouob/gbrain-notion-sync/issues/5)) ([0051bc9](https://github.com/bouob/gbrain-notion-sync/commit/0051bc9730790762c96c61446ce04afb3b03d1c6))

## [0.1.0] - 2026-05-17

### Added
- Initial release as a Claude Code plugin.
- One-way Notion to gbrain pull pipeline supporting four PAI databases
  (Projects, To-Do, Inbox, Knowledge Base).
- `/notion-sync` slash command with sub-commands `setup`, `pull`, `schedule`,
  `postprocess`, `status`, `doctor`.
- `scripts/install-task.ps1` — Windows Task Scheduler installer with
  `-Interval`, `-Uninstall`, `-Status`, `-Force` modes.
- `scripts/postprocess.mjs` — runs `gbrain extract links`, `gbrain dream`,
  and (if `OPENAI_API_KEY` is set) `gbrain embed --stale`.
- `scripts/doctor.mjs` — seven-check health probe covering env, build
  artifacts, gbrain CLI alignment, Notion token validity, and per-database
  reachability.
- Block converter supports `paragraph`, `heading_1/2/3`,
  `bulleted_list_item`, `numbered_list_item`, `to_do`, `code`, `quote`,
  `divider`.
- gbrain plugin manifest (`gbrain.plugin.json`) for subagent registration.
- CI: gitleaks secret scan and `bun run typecheck` on push and pull request.

### Known limitations
- Windows-only scheduler (macOS/Linux planned in v0.2).
- Pages with more than 100 blocks are truncated; `fetchBlockChildren` does
  not paginate yet (planned for v0.4).
- Vector search requires `OPENAI_API_KEY`. Without it, `gbrain embed` is
  skipped and search falls back to PostgreSQL `tsvector`.

### Not in this release (planned)
- v0.2 — Bidirectional sync, `sync-state.db` change tracking, push to Notion.
- v0.3 — Conflict detection with `.conflict/` backup directory.
- v0.4 — Extended block types (callout, toggle, database mention, file, image).
- v0.5 — Auto-embedding after sync (opt-in).
