# FastMCP Monetize Playbook — 2026-06-15

## Repo Snapshot

| Field | Value |
|-------|-------|
| Repo | PrefectHQ/fastmcp |
| URL | https://github.com/PrefectHQ/fastmcp |
| Stars | 25,631 |
| License | Apache-2.0 |
| Maintainer | Jeremiah Lowin (CEO Prefect) |
| PyPI downloads | 73.4M/tháng (~1M+/ngày) |
| Velocity | 46★/ngày (🔥 hot tier) |
| Age | 562 days |
| Stack | Python ≥3.10, uv |
| Description | The fast, Pythonic way to build MCP servers and clients |

## Why FastMCP Matters

- 70% MCP servers across all languages use some version of FastMCP (per Prefect's own stats)
- 1M+ PyPI downloads/day → top 0.1% Python packages
- FastMCP 1.0 was incorporated into the official MCP Python SDK in 2024
- v2 (standalone) is the actively maintained version
- v3 just landed with new "Apps" feature (interactive UIs in chat)

## Architecture: 3 Pillars

1. **Servers** — wrap Python functions → MCP tools/resources/prompts
2. **Clients** — connect to any MCP server (stdio, SSE, HTTP, OAuth)
3. **Apps** (v3) — interactive UIs rendered directly in chat

## Code Quality Signals

- Has AGENTS.md for LLM-driven dev (rare, signals AI-first mindset)
- prek pre-commit hooks (Ruff, Prettier, ty type checking)
- pytest -n auto (parallel test suite)
- Mintlify docs at gofastmcp.com
- Used `policies` for PR triage and DNM/DRAFT detection
- CodeRabbit + Codex bot reviews on PRs

## Monetization Angles (Ranked)

### #1: Vertical MCP Servers for Vietnamese Market (EASIEST, fastest ROI)

**Idea:** Build domain-specific MCP servers for VN market:
- shopee-mcp (search, list, order)
- baomoi-mcp (news aggregation)
- tiki-mcp (product search)
- vcb-mcp / techcombank-mcp (banking queries)
- ahamove-mcp / ghn-mcp / viettelpost-mcp (logistics)
- masothue-mcp (company lookup)

**Revenue model:**
- Free: 100 calls/tháng
- Pro: $29/tháng, unlimited calls
- Enterprise: $299/tháng, white-label, SLA

**TAM estimate:** 50K SME VN using AI agents. 1% conversion = 500 customers.
**Year 1 ARR potential:** 50 customers × $29 × 12 = $17.4K (low), 200 customers = $70K (high)

**Build cost:** $3K per server × 4 servers = $12K

### #2: FastMCP Cloud Hosting (vs Prefect Horizon)

**Idea:** Self-hosted FastMCP manager, cheaper than Horizon:
- Horizon: enterprise, $500+/tháng
- Indie version: $29-99/tháng

**Features:**
- 1-click deploy from GitHub
- Auto-scaling, monitoring
- Free: 1 server
- Pro: $29/tháng (3 servers)
- Team: $99/tháng (10 servers + team)

**Why viable:** FastMCP team focuses on framework, not indie hosting.

### #3: MCP Marketplace / Directory

**Idea:** "npm/PyPI for MCP servers" — directory with install/rating/search.

**Existing:** punkpeye/awesome-mcp-servers (89K★) is just a GitHub list, not productized.

**Monetization:**
- Featured listings: $99/tháng
- Verification badge: $299/năm
- Cut on installs

### #4: FastMCP Templates / Boilerplates

**Idea:** Sell starter kits:
- fastmcp-saas-template ($99)
- fastmcp-auth-template ($49)
- fastmcp-rag-template ($99)
- fastmcp-ecommerce-template ($149)

**Distribution:** Gumroad + Lemon Squeezy

### #5: TypeScript SDK (HIGH RISK, BIG upside)

**Insight:** FastMCP is Python-only. TS/JS devs lack official equivalent.

**Risk:** Anthropic could ship official TS SDK any day.

## Recommended Play: Start with #1

**Why:**
- Lowest risk
- Lowest cost ($15K total)
- VN market = low competition
- Fast validation
- Pivot path to #2 or #3 if successful

**60-day Build Plan:**
1. Week 1-2: shopee-mcp
2. Week 3-4: ahamove-mcp + ghn-mcp
3. Week 5-6: masothue-mcp
4. Week 7-8: Stripe + landing pages + ProductHunt launch

**Break-even:** 10 customers × $29/tháng = $290/tháng → 53 months (long)
**Realistic 12-month:** 50-200 customers = $17K-70K ARR

## Caveats

- TAM 1% conversion is very optimistic for new B2B SaaS
- Build cost may be 2x estimate (edge cases, rate limits)
- Prefect could ship "FastMCP Cloud" anytime
- VN AI adoption is early → need education + ROI proof

## Next Steps

- A) Build shopee-mcp prototype in 1 week
- B) Deep-dive another repo (firecrawl, claw-code)
- C) Technical analysis: Prefect Horizon vs indie hosting
- D) Skip — user explores independently
