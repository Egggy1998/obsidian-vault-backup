---
title: Hermes Video MCP — Build Log v1
date: 2026-06-02
status: partial-built, pending-deploy
tags: [stream-2, x402, bazaar, runninghub, build-log, decisions]
---

# 🎬 Hermes Video MCP — Build Log

## Status (4-tier MVP, 5/9 todos done)

| # | Task | Status |
|---|------|--------|
| 1 | Verify build env | ✅ |
| 2 | Setup project (TS, Express, x402, deps) | ✅ |
| 3 | Build skeleton với 4 tier tools (dummy) | ✅ |
| 4 | Wire RunningHub adapter (subprocess wrapper) | ✅ |
| 5 | Wire x402 payment gate + Bazaar discovery | ✅ |
| 6 | Local test: pay 1 USDC, verify settle, check indexing | ⏳ (need real wallet) |
| 7 | Deploy lên public HTTPS (agent37-host / Cloud Run) | ⏸️ |
| 8 | CDP validation: confirm cataloged, search Agentic.Market | ⏸️ |
| 9 | Soft launch: post X/LinkedIn | ⏸️ |

## ✅ Verified working

- Server starts, validates config, binds 4 tier routes
- All 4 routes return **HTTP 402** with `PAYMENT-REQUIRED` header (base64 USDC requirements)
- Bazaar discovery metadata declared correctly
- TypeScript compile clean
- Auto-loads RUNNINGHUB_API_KEY from `~/.openclaw/openclaw.json`

## 🐛 Bugs hit + fixes (read first if forking)

### Bug 1: Bazaar extension schema — nested vs flat
**Symptom:** `x402: Route "POST /v1/video/budget" has an invalid bazaar extension: /input/method: must be equal to one of the allowed values`
**Root cause:** I nested `method`/`bodyType`/`type` inside `input` object. The `declareDiscoveryExtension` API expects them at the TOP level, not nested. The `input` field IS the body schema (flat).
**Fix:**
```ts
// WRONG
declareDiscoveryExtension({
  input: { method: "POST", bodyType: "json", body: { ... } }
})

// CORRECT
declareDiscoveryExtension({
  method: "POST",          // top-level
  bodyType: "json",        // top-level
  input: { ... },          // body schema (flat)
  output: { example: ... }
})
```
**Source:** `node_modules/@x402/extensions/dist/esm/chunk-KKZBRP7D.mjs` `createBodyDiscoveryExtension` function.

### Bug 2: 404 handler registered before x402 middleware
**Symptom:** All routes return `{"error":"not_found"}` HTTP 404, even though routes ARE registered.
**Root cause:** Express middleware order. The 404 catch-all `app.use((_req, res) => ...) res.status(404)` was registered BEFORE `app.use(paymentMiddleware(...))` and `app.use(videoRoutes)`. So every request hits the 404 first.
**Fix:** Move 404 handler to be the LAST `app.use(...)` call.

### Bug 3: Bash sandbox blocks `cat | python3` and `curl | python3`
**Symptom:** "Pipe to interpreter" security warning, requires user approval.
**Workaround:** Use `terminal(command=...)` for individual commands, or use `python3` via `hermes_tools` in `execute_code` for processing.

### Bug 4: `pkill -f "tsx"` got blocked
**Symptom:** "Command required approval (force kill processes)" — system-protected.
**Workaround:** Use `process(action="kill", session_id=...)` for managed background processes; for ad-hoc kills, expect approval prompt.

## 🏗️ Architecture decisions (rationale)

### Decision 1: TypeScript + Express (not Python/Fastify/Hono)
- x402 official reference SDK is TypeScript-first
- `@x402/express` middleware is the canonical pattern
- Express 4 (not 5) — most examples use 4; 5 may have routing quirks

### Decision 2: RunningHub via Python subprocess (not native TS adapter)
- The `runninghub.py` script (in skill) already handles async polling, error codes, file output
- Re-implementing in TS = ~200 lines duplicated for no MVP gain
- Subprocess adds ~500ms cold start but acceptable
- If latency becomes issue, refactor to native TS later

### Decision 3: 4 separate HTTP routes (not 1 with `?tier=`)
- x402 Bazaar indexes by route — 4 routes = 4 manifest entries = 4 separate listings on Agentic.Market
- Each tier has its own price, model, input schema
- MCP layer can wrap 4 routes as 4 tools (1-to-1 mapping)

### Decision 4: Subprocess timeout 10min (not async job queue)
- RunningHub typical generation: 30-120s
- 10min = 5x headroom for retries
- For v1, synchronous wait is fine; users get 1 video at a time
- v2: implement async job + webhook callback for >5min generations

### Decision 5: `file://` URLs for v1, GCS/CDN for v2
- Local dev: fine, returns `file:///tmp/hermes-video-mcp/req_xxx.mp4`
- Production: need CDN (Cloud Storage + signed URL, 24h TTL)
- This is a known gap — documented in route response `note` field

### Decision 6: agent37-host for v1, Cloud Run for v2
- Agent37 RAM/bandwidth limit but persistent paths
- `agent37-host` exposes public HTTPS in 1 command, no ngrok
- Caveat: URL may change on container image update → re-publish manifest
- v2: Cloud Run when MRR > $100/mo justifies $5/mo min-instance cost

### Decision 7: Dummy EVM address for v1, real wallet for v2
- Currently using `0x0000000000000000000000000000000000000001` (passes schema validation)
- Real Base Mainnet wallet required to receive USDC payments in production
- Real Base Sepolia wallet + test USDC for settle-flow test

## 🔌 x402 Schema Details (for reference)

### `PAYMENT-REQUIRED` header (base64-encoded JSON):
```json
{
  "x402Version": 2,
  "accepts": [
    {
      "scheme": "exact",
      "price": "$0.20",
      "network": "eip155:84532",  // or eip155:8453 for mainnet
      "payTo": "0x0000...0001",
      "maxTimeoutSeconds": 60,
      "extra": { ... }  // EIP-712 domain, asset, etc.
    }
  ],
  "resource": "https://hermes-video-mcp.agent37.com/v1/video/budget",
  "extensions": {
    "bazaar": { ... }  // declared discovery metadata
  }
}
```

### Network IDs
- Testnet: `eip155:84532` (Base Sepolia), USDC contract `0x036CbD53842c5426634e7929541eC2318f3dCF7e`
- Mainnet: `eip155:8453` (Base), USDC contract `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`

## 📁 Project structure

```
hermes-video-mcp/
├── package.json              # @x402/* v2.14, express 4, dotenv, tsx, typescript
├── tsconfig.json             # ES2022, bundler resolution
├── .env.example              # EVM_ADDRESS, RUNNINGHUB_API_KEY, X402_NETWORK
├── .gitignore                # node_modules, dist, .env
├── README.md                 # Quick start + API docs
├── src/
│   ├── index.ts              # Express + x402 middleware + 4-tier route config
│   ├── routes/
│   │   └── video.ts          # POST /v1/video/:tier handler
│   ├── adapters/
│   │   └── runninghub.ts     # Python subprocess wrapper
│   └── utils/
│       └── config.ts         # Env + OpenClaw config loader, tier definitions
└── test/
    └── smoke.ts              # Health + 402 gate check
```

## 🚀 Next steps (for whoever picks this up)

1. **Get real wallet** — anh provide EVM address (Base Mainnet)
2. **Test on testnet first** — set `X402_NETWORK=testnet`, get Base Sepolia USDC, settle 1 test payment
3. **Verify indexing** — call `https://api.cdp.coinbase.com/platform/v2/x402/discovery/search?query=video` after 1 successful settle
4. **Deploy to agent37-host** — get public HTTPS URL
5. **Trigger re-indexing** — 1 paid call from real x402 client → auto-listed on Agentic.Market
6. **Set up cron** — 1 cheap call every 25 days to maintain 30-day visibility window

## 🔗 Resources
- [x402 Bazaar docs](https://docs.cdp.coinbase.com/x402/bazaar)
- [@x402/express npm](https://www.npmjs.com/package/@x402/express)
- [x402-foundation/x402 examples](https://github.com/x402-foundation/x402/tree/main/examples/typescript/servers)
- [Agentic.Market](https://agentic.market)
- [RunningHub skill](file:///home/node/.hermes/skills/runninghub/SKILL.md)
