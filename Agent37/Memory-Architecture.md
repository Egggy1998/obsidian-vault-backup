---
type: architecture
created: 2026-06-01
---

# Hermes Memory Architecture (Agent37)

## 6 Tầng, mỗi tầng một mục đích

### 1. MEMORY.md (~2.2k chars, hot path)
- Critical rules only
- Pointer ngắn tới skill/Obsidian, không inline detail
- Examples: "API-first not browser", "default = fast"

### 2. USER.md (~1.4k chars, hot path)
- Identity + working style
- Persona: address, tone, minimal confirmations
- Behavioral prefs: image-edit, AI Creative style

### 3. Skills (on-demand, no char limit)
- Procedural memory: "how to use X"
- SKILL.md = main file
- `references/`, `templates/`, `scripts/` = long detail
- Load bằng `skill_view(name)` khi cần

### 4. Obsidian vault (durable, MB-scale)
- Project context: clients, decisions, history
- Path: `/home/node/ObsidianVault/`
- Folders: `Clients/`, `Tools/`, `Workflows/`, `Decisions/`, `Agent37/`

### 5. Daily logs (`memory/YYYY-MM-DD.md`)
- Raw session notes
- Append-only
- Search via `session_search` cho past context

### 6. LEARNINGS/ERRORS (`.learnings/*.md`)
- Cross-session lessons
- Append-only

## Decision Matrix: where does this go?

| Type | Goes to | Example |
|---|---|---|
| "I prefer X" | USER | "minimal confirmations" |
| "API X has quirk Y" | MEMORY | "PnL dates = startDate/endDate" |
| "How to use tool X" | Skill | autovideo-api/SKILL.md |
| "Client X has Y" | Obsidian /Clients/ | EvaFamily.md |
| "Yesterday I made Z" | Daily log | memory/2026-06-01.md |
| "Lesson: X fails" | LEARNINGS.md | RAI blocks mom-baby |
| "Yesterday I had this discussion" | session_search | past chat recall |
| "Long SOP for X" | Obsidian /Agent37/ | Routing SOP |

## Anti-Overflow Pattern
1. Trước khi add MEMORY/USER entry → check skill/Obsidian đã có chưa
2. Detail >200 chars → Obsidian hoặc skill `references/`
3. "Important decision" → Obsidian (per AGENTS.md rule)
4. Daily session log → `memory/YYYY-MM-DD.md`, không pollute MEMORY

## When MEMORY/USER > 85%
- Compress verbose phrasing
- Remove outdated entries (e.g. "console required" lines that are stale)
- Dedup duplicates (3 entries cùng nhắc video pipeline)
- Move info đúng chỗ (user prefs → USER, facts → MEMORY, project → Obsidian)

## Notes
- Hard limits: MEMORY 2,200 chars, USER 1,375 chars
- After 2026-06-01 migration: MEMORY ~18% (~400 chars), USER ~22% (~300 chars)
- Vault path persisted in `~/.hermes/.env` as `OBSIDIAN_VAULT_PATH`
