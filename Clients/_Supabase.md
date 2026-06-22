---
type: system_account
platform: supabase
created: 2026-06-08
source: Telegram DM with Hồng Quảng
---

# Supabase — SmartMedia

## Account
- **Email**: `quangnh@smartmedia.vn` (verified via API)
- **Organization ID**: `orvcdatvtnwnyluchqec`
- **Organization slug**: `orvcdatvtnwnyluchqec` (placeholder slug, not branded)
- **Region**: `ap-northeast-2` (Seoul)

## Projects (1)
| Project name | Project ref | Region | DB | Status | Created |
|---|---|---|---|---|---|
| quangnh@smartmedia.vn's Project | `hffwdthzhhlugvpulgvk` | ap-northeast-2 | Postgres 17.6.1 | ACTIVE_HEALTHY | 2026-05-21 |

DB host: `db.hffwdthzhhlugvpulgvk.supabase.co`

## Personal Access Token

**File**: `~/.hermes/secrets/supabase.env` (chmod 600, owner-only)
**Key**: `SUPABASE_ACCESS_TOKEN`
**Type**: Personal Access Token (`sbp_` prefix)
**Value**: `sbp_95...70a4`
**Scope**: full Management API (verified working: list projects, db, functions, secrets)
**Created**: 2026-06-08 by Hồng Quảng

⚠️ **Security notes**:
- Email-bound token (`quangnh@smartmedia.vn`) — different identity from Egggy1998's GitHub
- Personal Access Token inherits the user, NOT the org. If `quangnh@smartmedia.vn` is removed from org, token breaks
- File has `chmod 600` — do not move to shared location
- Never commit to git, never paste in chat logs, never log in CI
- Rotate immediately if exposed

## Where token is used
- **Supabase Management API** — `https://api.supabase.com/v1/*` (projects, db, functions, secrets, auth, storage)
- **Direct REST API** — ad-hoc automation, cron jobs
- **NOT for** SQL/PostgREST/Auth API (those use different keys per project: `anon`, `service_role`, project URL `https://<ref>.supabase.co`)

## Token validation
```bash
curl -sS -H "Authorization: Bearer $SUPABASE_ACCESS_TOKEN" \
  https://api.supabase.com/v1/projects
```

## Related
- Composio `SUPABASE_*` actions — when wiring MCP automation
- SmartMedia context — confirm with Hồng Quảng which client/project uses this org
