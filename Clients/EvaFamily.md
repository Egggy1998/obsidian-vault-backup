---
type: client
platform: facebook
created: 2026-06-01
source: Telegram DM with Hồng Quảng
---

# EvaFamily — Mom & Baby Brand

## FB Pages
- **EvaFamily** (primary): `1130373526822698`
- Profile app: `app_69c24d854f603254_rrpb`

## Content Style
- Asian/Vietnamese characters (avoid Western faces)
- No text/subtitles in generated media
- Image-first for character consistency across videos
- ≥1 min final videos, broken into shorter scenes (≤8s)
- No BGM unless explicitly requested
- Prefer model-native Vietnamese VO from Veo over external TTS
- Maintain character/style continuity across scenes

## Posting Pipeline
- Webhook: `https://n8n11.ezn8n.com/webhook/evafamily`
- Payload: `row_id` ở root + content fields
- Per-row = single POST (no batch grouping)
- SA Designer tự design + update `Link ảnh đã thiết kế` trong Baserow
- See [[Workflows/FB-Post]]

## Credentials
- **Upload-Post API key**: `~/.hermes/secrets/upload_post.env` (chmod 600, exp ~2126)
- Email bound: sanonihongo@gmail.com
- App ID (EvaFamily only): `app_69c24d854f603254_rrpb`

## Related Decisions
- [[Decisions/2026-06-01-Video-Pipeline]] — Composio veo-3.0-fast-gen cho mom-baby
