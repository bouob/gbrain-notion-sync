# GBrain Skill Resolver

This is the dispatcher. Skills are the implementation. **Read the skill file
before acting.** If two skills could match, read both.

## Brain operations

| Trigger | Skill |
|---------|-------|
| "/notion-sync", "sync notion", "pull notion", "push notion", "refresh brain", "init notion sync", "setup notion sync", "notion sync status", "notion sync conflicts", "run notion postprocess", "notion sync doctor", "同步 notion", "拉取 notion", "推回 notion", "雙向同步", "跑後處理", "brain 沒更新", "檢查 notion 同步", "notion 衝突", "capture", "記進腦", "存到 brain", "把這次學到的記下來" | `skills/notion-sync/SKILL.md` |

## Disambiguation rules

1. All Notion ↔ gbrain sync intents route to `skills/notion-sync/SKILL.md`;
   pick the sub-command (init/setup/pull/push/conflicts/schedule/postprocess/
   status/doctor/capture) from the user's verb.
2. When in doubt about scope (one DB vs all four), ask the user before writing.
