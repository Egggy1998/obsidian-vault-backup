# AiToEarn — Content Monetization

## Setup
- **Environment**: International (`aitoearn.ai`)
- **API Key**: `/home/node/.aitoearn-key` (chmod 600, **KHÔNG commit Git public**)
- **MCP endpoint**: `https://aitoearn.ai/api/unified/mcp`
- **Required headers** (missing any = different error):
  - `Content-Type: application/json`
  - `Accept: application/json, text/event-stream`
  - `x-api-key: <key>`
- **Required UA**: `Mozilla/5.0 ... Chrome/120.0.0.0` — Cloudflare 1010 blocks default urllib UA

**Status:** Last live check 2026-06-15 10:35 UTC — **BLOCKED** by expired OAuth for Twitter. User accepted new task `6a2fd3f61b695861c509befd` (CPE $10) at 10:29 UTC, expires 12:29 UTC. Needs urgent re-auth.

## Active User Tasks (2026-06-15)
| userTaskId | taskId | Status | Note |
|---|---|---|---|
| `6a2fd3f61b695861c509befd` | `6a28cce58686f60bdb376d9a` | **doing** (expires 12:29 UTC) | Twitter CPE $10, accepted 10:29 UTC, keepTime 7200s |
| `6a1bb891090ad573b6a45e62` | `69e6dcbb88c4821be5104ffd` | **expired** (2026-06-14) | Threads $100, code 11 forfeited |
| `6a02ea7cf9823326e719a65a` | `69d4649b92a22b5d1dc79c7d` | **settled** | TikTok $100 paid |

## MCP Tool Notes (2026-06-15 live)
- ✓ `getTaskDetail` now works (no more "Validation failed")
- ✗ `listTaskMarket` returns empty `total: 0` (API change or user-filtered)
- ✗ `listCampaignMarket` also empty — cannot recommend new tasks
- ✗ `getAllAccounts` still "Unknown tool" (confirmed 2026-06-14)
- 72 tools total, 18 task-related

## OAuth expiry
- Twitter: last worked 2026-05-31, expired 2026-06-14. Now 2026-06-15 still expired.
- Threads: same status.
- Re-auth flow: web-UI only, no MCP workaround.
- Window observed: ~14 days from last successful post.

## Action required
1. User re-authorize Twitter at aitoearn.ai (URGENT — 1h55m to task expiry)
2. After re-auth, agent should: write content → `createChannelPublishFlow` → `publishChannelTaskNow` → `submitTask` with workLink
3. After task settles, check `getAffiliateSettlement` within 48h to confirm pending (lesson: code 11 silently voids rewards)
