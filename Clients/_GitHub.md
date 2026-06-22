---
type: system_account
platform: github
created: 2026-06-08
source: Telegram DM with Hồng Quảng
---

# GitHub — Egggy1998

## Account
- **Username**: `Egggy1998`
- **User ID**: 208957811
- **Email bound**: (check GitHub settings)

## Personal Access Token

**File**: `~/.hermes/secrets/github.env` (chmod 600, owner-only)
**Key**: `GITHUB_ACCESS_TOKEN`
**Type**: Classic PAT (`ghp_` prefix)
**Scope**: FULL admin — `repo, workflow, admin:org, delete_repo, write:packages, gist, user, ...` (see [GitHub PAT scopes](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#scopes-for-personal-access-tokens-classic))

⚠️ **Security notes**:
- Classic PAT = broad org-level access, not scoped to specific repos
- Consider rotating to **fine-grained PAT** (per-repo, read-only/write-only) for new use cases
- File has `chmod 600` — do not move to shared location
- Never commit to git, never paste in chat logs, never log in CI

## Where token is used
- **`gh` CLI** — repo management, PR workflow, code review
- **Composio `GITHUB_*` actions** — file create/update, issues, releases (per AGENTS.md, primary tool for Obsidian vault push)
- **Direct REST API** — ad-hoc automation, cron jobs

## Related
- AGENTS.md → "OBSIDIAN = GITHUB CANONICAL" rule
- `Egggy1998/quang-obsidian-vault` (private, default branch `main`) — Obsidian sync target
