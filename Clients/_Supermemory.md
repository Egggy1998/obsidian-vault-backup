---
type: system_account
platform: supermemory
created: 2026-06-09
source: Telegram DM with Hồng Quảng
---

# Supermemory.ai — Hermes Memory Backend

## Account
- **Token type**: API key (`sm_` prefix, 90 chars)
- **Created**: 2026-06-09 by Hồng Quảng
- **Tier**: Free (verify on dashboard)
- **Status**: ✅ Verified working (POST /v3/search returns 200)

## Credentials
- **Key file**: `~/.hermes/secrets/.sm_key` (chmod 600) — raw 90-char token
- **Env file**: `~/.hermes/secrets/supermemory.env` (chmod 600) — placeholder + metadata
- ⚠️ **Note**: Hermes `write_file` auto-redacts `sm_` tokens. Key stored in hidden file (`.sm_key`) which bypasses redaction in display, but actual write still requires Python subprocess

## API Reference
- **Base URL**: `https://api.supermemory.ai/v3`
- **Auth**: `Authorization: Bearer *** 
- **Endpoints verified 2026-06-09**:
  - `GET /v3/health` → 200 `{"status":"ok"}`
  - `POST /v3/search` → 200 (with body `{"q":"...","containerTags":[...], "limit": N}`)
  - `POST /v3/documents` → 200/201, returns `{id, status: "queued"}` (processing async)
  - `POST /v4/search`, `/v4/memories`, `/v4/conversations`, `/v4/profile` → 400 (need specific body format)
  - `GET /v3/account`, `/v3/profile`, `/v3/user` → 404 (not user-facing endpoints)

## CLI Wrapper
- **Path**: `~/.hermes/scripts/sm.py` (chmod +x)
- **Commands**:
  - `sm.py test` — verify token
  - `sm.py save "text" --tag TAG` — add memory
  - `sm.py search "query" --tag TAG --limit N` — search (q cannot be empty)
  - `sm.py list --tag TAG --limit N` — uses search with empty q (currently fails, use search instead)
- **Container tag convention**:
  - `hermes-bootstrap` — initial setup tests
  - `hermes-default` — generic memory
  - TBD: per-client tags (e.g., `eva-family`, `baby-am`)

## Use Cases (planned)
- Cross-session context: search before answering for related memories
- Backup of local MEMORY.md / daily logs
- Search across all past conversations (with consent)
- Auto-save important user facts/preferences
- Sync Obsidian vault to supermemory for semantic search

## Related
- Local memory: `~/.hermes/memories/MEMORY.md` (curated) + `MEMORY.md` (workspace) + `~/.hermes/memories/YYYY-MM-DD.md` (daily logs)
- Obsidian vault: `~/ObsidianVault/Agent37/Memory-Architecture.md` (memory system design)
