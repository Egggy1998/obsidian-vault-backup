---
title: Agentic.Market MCP Server Launch Plan v1
date: 2026-06-02
status: research-complete, ready-to-build
tags: [stream-2, x402, bazaar, runninghub, mcp-server, cloud-run]
---

# 🎯 Product: Hermes Video MCP — Pay-per-Video USDC API

## TL;DR
Build 1 MCP server trên GCP Cloud Run, expose 4 tier video generation từ RunningHub backend, bán qua x402 protocol (USDC on Base). Listing trên Agentic.Market = TỰ ĐỘNG (chỉ cần 1 payment settle thành công qua CDP Facilitator). Thị trường video gen trên Bazaar gần như trống (1 competitor duy nhất, $0.50/10s Grok). Cost backend ¥0.10-0.50 ($0.015-0.07) → margin 10-30x.

## 🏆 Market Snapshot (verified 2026-06-02)
Bazaar search "video generation" → **5 resources, chỉ 1 thật sự gen video**:
| # | Price | Description | Model |
|---|-------|-------------|-------|
| 1 | $0.50/10s | xona-agent | Grok Imagine Video |
| 2-5 | $0.001-0.005 | 4x prompt/strategy services (text only) | — |

→ **Blue ocean.** 1 video gen competitor duy nhất. Nhu cầu ngầm có (vì sao Coinbase build cả x402+Bazaar+Agentic.Market).

## 🏗️ Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│  AI Agent       │───▶│  Hermes MCP      │───▶│  RunningHub     │
│  (any client)   │    │  (Cloud Run)     │    │  Backend API    │
└─────────────────┘    │                  │    └─────────────────┘
        │              │  ┌────────────┐  │            │
        │              │  │ x402 gate  │  │            │
        │  HTTP 402    │  │ (USDC pay) │  │            │
        │◀─────────────│  └────────────┘  │            │
        │              │       │           │            │
        │  USDC settle │       ▼           │            │
        │─────────────▶│  CDP Facilitator  │            │
        │              │       │           │            │
        │  settle OK   │       │ auto-index│            │
        │◀─────────────│       ▼           │            │
        │              │  x402 Bazaar ◀────│ (catalog)  │
        │              │       │           │            │
        │              │       ▼           │            │
        │              │  Agentic.Market   │            │
        │              │  (search UI)      │            │
        └──────────────┴──────────────────┘
```

**Deploy target:** GCP Cloud Run (Agent37 quá nhỏ ~20MB binary limit + RAM hạn chế).
**Code:** TypeScript (x402 reference SDK chính thức từ Coinbase, có `@x402/extensions/bazaar` codegen).

## 💰 Pricing Strategy (proposed)

**4 tiers, undercut xona-agent ở tier thấp + premium ở tier cao (Veo 3.1):**

| Tier | Backend Model | Duration | Price (USDC) | Cost (¥) | Margin |
|------|---------------|----------|--------------|----------|--------|
| **budget** | rhart-video-v3.1-fast (Fast) | 6s | **$0.20** | ¥0.10 (~$0.014) | 14x |
| **standard** | rhart-video-v3.1-pro (Pro) | 8s | **$0.40** | ¥0.20 (~$0.028) | 14x |
| **premium** | kling-v3.0-pro (Kling 3 Pro) | 10s | **$0.80** | ¥0.40 (~$0.056) | 14x |
| **cinematic** | veo-3.1-generate-001 (Veo 3.1 Q, native audio) | 8s | **$2.00** | ~$0.30 (Vertex direct) | 6.7x |

**Justification:**
- $0.20 budget = 60% rẻ hơn xona ($0.50), low-friction cho agent test
- $2.00 cinematic = premium segment, Veo 3.1 có native audio (chưa ai trên Bazaar có)
- Flat-fee per video (không $/s) — đơn giản cho agent integration

## 📦 Service Shape (theo pattern xona-agent đang dùng)

**1 MCP server expose 4 tools:**
- `generate_video_budget` — $0.20, 6s, Fast
- `generate_video_standard` — $0.40, 8s, Pro
- `generate_video_premium` — $0.80, 10s, Kling 3 Pro
- `generate_video_cinematic` — $2.00, 8s, Veo 3.1 Quality + native audio

**Input schema (chung):**
```json
{
  "prompt": "string (required)",
  "image_url": "string (optional, img2video)",
  "aspect_ratio": "16:9 | 9:16 | 1:1 (optional, default 16:9)",
  "duration": "int seconds (optional, override default)"
}
```

**Output schema (chung):**
```json
{
  "video_url": "string (CDN, 24h TTL)",
  "thumbnail_url": "string",
  "model": "string",
  "duration": "int",
  "cost_usdc": "string",
  "request_id": "string"
}
```

## 🚀 Launch Sequence (8 steps, ~1 tuần)

1. **Build skeleton** (~2-3h) — TypeScript MCP server + x402 middleware + 1 dummy endpoint, deploy Cloud Run, get URL
2. **Add RunningHub adapter** (~2h) — Wrap 4 video models, fallback logic, async job polling
3. **Wire x402 payment gate** (~1h) — `@x402/extensions/bazaar` config, route-level pricing, USDC on Base
4. **Test full flow** (~1h) — Local: pay 1 USDC micropayment, verify settle, check indexing trên `https://api.cdp.coinbase.com/platform/v2/x402/discovery/search`
5. **Optimize latency** (~1-2h) — RunningHub async, store results in GCS, return CDN URL; target <90s p95
6. **Add error handling** (~1h) — Refund on gen failure, queue depth limits, rate limiting
7. **CDP validation** — Hit `https://agentic.market/validate` endpoint, confirm cataloged
8. **Soft launch** — Manual post trên X/LinkedIn với link Agentic.Market listing, monitor 1 tuần

**Critical first-payment trigger:** Indexing chỉ xảy ra SAU khi 1 payment settle thành công. Em phải tự pay 1 lần (~$0.20) để trigger. Có thể dùng CDP test wallet.

## 🛡️ Risk Mitigation

| Risk | Mitigation |
|------|------------|
| ToS RunningHub cấm resale API | Wrap calls (xem session research), diversify với PicsArt/Vertex làm backup |
| Latency RunningHub cao (>90s) | Async job + webhook callback, return request_id ngay, video sau |
| 30-day rolling window cho visibility | Cron job hàng tuần tự pay 1 call để giữ cataloged |
| Cloud Run cold start | Min instances = 1, ~$5/month |
| USDC price volatility | Lock pricing trong USDC (không phải local fiat), buyer chịu FX risk |
| Vertex Veo 3.1 cost có thể cao hơn estimate | Track margin per-call trong DB, alert nếu margin <5x |

## 📊 Success Metrics (first 30 ngày)

- **Visibility:** Listed on Agentic.Market + CDP Bazaar search results
- **Adoption:** 10+ unique buyers, 50+ paid calls
- **Revenue target:** $50-300 MRR (3-month goal per Stream 2)
- **Margin:** Maintain >10x average

## ❓ Cần anh quyết

**Q duy nhất:** Anh muốn em start với tier nào trước?
- (A) **Lean MVP** — 1 tier (budget $0.20/rhart-video-v3.1-fast) → launch trong 2-3 ngày, validate flow
- (B) **Full 4 tiers** — build tất cả cùng lúc, launch trong 5-7 ngày, đa dạng từ đầu
- (C) **Premium-first** — 1 tier cinematic ($2.00/Veo 3.1) → tập trung margin cao, niche agents

Em recommend **(A) Lean MVP** — verify flow + indexing + 1 sale → rồi expand. Rủi ro thấp nhất, học nhanh nhất.

## 🔗 References
- x402 Bazaar docs: https://docs.cdp.coinbase.com/x402/bazaar
- x402 extensions: `@x402/extensions/bazaar` (route config + JSON schema validation)
- Network: Base Mainnet (eip155:8453), USDC, eip712
- Competitor benchmark: xona-agent (Grok Imagine, $0.50/10s, Base)
- Higgsfield MCP (out-of-ecosystem competitor, subscription model — không phải x402)
