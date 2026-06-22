---
type: tool
category: analytics
created: 2026-06-01
---

# PnL KPI Dashboard

## URLs
- Web: `https://pnl-kpi-dashboard.vercel.app`
- MCP endpoint: `/api/mcp` (tools: campaign/PnL/KPI/report CRUD)
- **MyTV Metrics API** (Lovable): `https://project--adf533f8-8fa0-45da-a275-558aaef1e264.lovable.app/api/public/metrics`
  - Public, no auth
  - ⚠️ Blocks default Python User-Agent → use browser UA
  - Schema: `campaign`, `metric_date`, `viewers`, `engagement`, `link_clicks`, `reach`, `impressions`
  - UPSERT key: `(campaign, metric_date)`

## Schema (CRITICAL — exact fields)
- Campaign dates: **`startDate`** and **`endDate`** (NOT `dateStart` / `start_date` / `date_start`)
- Sample EvaFamily campaign ID: `camp-1779704146784-cw88`

## Common mistakes
- Inventing field aliases (e.g. `dateStart` instead of `startDate`) → schema validation fails
- Skipping verification on schema change → 400/422 errors
- Treating sample IDs as real → confusion in reports

## Linked
- All EvaFamily campaigns loaded here
- Update via MCP `updateCampaign` sau mỗi ad run
- Pull KPI snapshots cho daily/weekly reports
