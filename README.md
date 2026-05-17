# gbrain-notion-sync

[![secret-scan](https://github.com/bouob/gbrain-notion-sync/actions/workflows/secret-scan.yml/badge.svg)](https://github.com/bouob/gbrain-notion-sync/actions/workflows/secret-scan.yml)
[![typecheck](https://github.com/bouob/gbrain-notion-sync/actions/workflows/typecheck.yml/badge.svg)](https://github.com/bouob/gbrain-notion-sync/actions/workflows/typecheck.yml)

One-way Notion PAI second-brain to local [gbrain](https://github.com/garrytan/gbrain)
knowledge graph sync, packaged as a Claude Code plugin.

Notion is source of truth; gbrain is an agent-friendly local mirror that gives
Claude Code hybrid search and graph traversal across all four PAI databases
(Projects, To-Do, Inbox, Knowledge Base).

---

## Install (Claude Code plugin)

```bash
/plugin marketplace add bouob/gbrain-notion-sync
/plugin install gbrain-notion-sync@bouob
```

Then complete the setup in [RUNBOOK.md](./RUNBOOK.md): gbrain install,
Notion integration, `.env`, dependency install, doctor, first dry-run,
and (optionally) `/notion-sync schedule` for automatic periodic sync.

After install, the `/notion-sync` slash command exposes six sub-commands.
See [skills/notion-sync/SKILL.md](./skills/notion-sync/SKILL.md) for details.

---

## Sub-commands at a glance

| Sub-command | Purpose |
|---|---|
| `/notion-sync setup` | First-time environment verification |
| `/notion-sync pull` | One-shot Notion to gbrain pull |
| `/notion-sync schedule` | Install Windows Task Scheduler entry (default 15 min) |
| `/notion-sync postprocess` | Run `gbrain extract links`, `dream`, and (opt) `embed --stale` |
| `/notion-sync status` | Show brain contents and scheduled task state |
| `/notion-sync doctor` | Seven-check health probe |

---

## Project layout

```
.
|-- .claude-plugin/         Plugin manifest (plugin.json, marketplace.json)
|-- skills/notion-sync/     SKILL.md - the /notion-sync slash command
|-- src/                    TypeScript source (Notion client, block converter, gbrain adapter)
|-- scripts/                Executable scripts (sync-pull, postprocess, doctor, install-task)
|-- subagents/              gbrain plugin subagent definition
|-- docs/compat-matrix.md   gbrain version compatibility log
|-- tests/                  Smoke test scaffold
|-- gbrain.plugin.json      gbrain plugin manifest (separate from Claude plugin)
|-- RUNBOOK.md              Setup guide
|-- CHANGELOG.md            Release history
`-- .env.example            Template - copy to .env and fill in
```

---

## Architecture

```
[Notion PAI DB]
       |
       | (1) Task Scheduler triggers every N minutes
       v
[scripts/sync-pull.mjs] --- (2) NOTION_DB_* -> fetch blocks -> markdown
       |
       | (3) `gbrain put <slug> --content <md>` per page
       v
[gbrain PGLite brain]
       |
       | (4) user runs /notion-sync postprocess
       v
[gbrain extract links] + [gbrain dream] + (opt) [gbrain embed --stale]
       |
       | (5) Claude Code queries via gbrain MCP
       v
[gbrain query / mcp__gbrain__*]
```

Sync stays fast and predictable; post-processing (graph re-extraction,
dreams, embeddings) is decoupled into a separate sub-command.

---

## Status

v0.1 ships Phase 1 (one-way Notion to gbrain pull) only.

| Phase | Status |
|---|---|
| Phase 1 - One-way pull | Shipped in v0.1 |
| Phase 2 - Bidirectional sync, `sync-state.db` | Planned v0.2 |
| Phase 3 - Conflict detection, `.conflict/` backup | Planned v0.3 |
| Phase 4 - Extended block types (callout, toggle, ...) | Planned v0.4 |
| Phase 5 - Auto embedding after sync | Planned v0.5 |

See [CHANGELOG.md](./CHANGELOG.md) for release history.

---

## License

MIT. See [LICENSE](./LICENSE).
