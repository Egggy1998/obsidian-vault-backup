---
type: decision
date: 2026-06-01
status: confirmed
---

# 2026-06-01: EvaFamily Video Pipeline

## Context
- Cần tạo video EvaFamily (mom-baby 30s) cho FB
- Test 4 Vertex AI veo-3.1-lite prompts: ALL BLOCKED by RAI filter (codes 58061214, 17301594)
- AutoVideo acc 65 cũng BLOCKED
- Composio gemini toolkit: ACTIVE, no auth required, `"bare"` session_id works

## Decision
**Composio `GEMINI_GENERATE_VIDEOS` = PRIMARY for mom-baby content.**

## Routing
- **mom-baby / family**: Composio `GEMINI_GENERATE_VIDEOS` + `veo-3.0-fast-generate-001` + `person_generation="allow_all"`
  - Session: `"bare"`
  - 4-8s scenes, parallel gen, ffmpeg concat
  - S3 download: **urllib** (curl breaks signed URL)
- **other people / portrait**: Vertex AI `veo-3.1-lite` first (cheaper, higher quality)
- **non-person (products, scenery)**: Vertex AI `veo-3.1-lite` first

## Default model
- `veo-3.0-fast-generate-001` per anh Mi béo (2026-06-01: "mặc định dùng fast")
- Switch to `veo-3.0-generate-001` (standard) chỉ khi user yêu cầu chất lượng cao hơn

## Verified
- 2026-06-01: 4 scenes × 8s = 30s, parallel gen, RAI pass
- Output: `/home/node/videos/evafamily/test_mom_baby_30s_2026-06-01.mp4` (5.88 MB)
- Pipeline total: ~2 phút (30s gen + download + concat + verify)

## Blocked / Open
- Vertex AI path: chưa verify cho non-mom-baby (3D Laser, nail art) — cần test
- AutoVideo acc 65: BLOCKED (no fix planned)
- GCP Veo SA IAM role chưa enable (cost blocker for fallback)

## Related
- Skill: `autovideo-api` (rewritten 2026-06-01)
- Reference: `autovideo-api/references/composio-gemini-veo.md`
- LEARNINGS: Vertex AI RAI blocks mom-baby
- [[Clients/EvaFamily]] — content style
