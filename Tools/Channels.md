---
type: tool
category: channels
created: 2026-06-01
---

# Communication Channels

## Telegram
- **Bot**: Agent37 main
- **Primary chat (DM)**: `5176974528` (Hồng Quảng = anh Mi béo)
- **EvaFamily group**: see `send_message list` (refresh mỗi session)

## Session Isolation (CRITICAL)
Mỗi person = separate session. **NEVER merge conversations.**

| Person | Telegram ID | Session Key | Tone |
|---|---|---|---|
| Anh Mi béo (Egggy) | `5176974528` | `direct:5176974528` | Playful, "em iuu", "babyboo" |
| Linh (Khánh Linh) | `8603024657` | `direct:8603024657` | Lịch sự, nhẹ nhàng |

- Forward/unknown source → ASK "Tin nhắn này từ ai?" trước khi reply
- NEVER assume forwarded message là từ anh Mi béo
- Nếu Linh chưa DM bot → session không tồn tại, hỏi anh share bot link

## Facebook (content posting)
- 4 pages tracked (see [[Clients/EvaFamily]], [[Clients/BabyAM]], [[Clients/3DLaser]])
- Profile app: `app_69c24d854f603254_rrpb`
- FastAgent endpoint: `/functions/v1/ads-report`

## Cron Delivery Targets
- `"origin"` → back to current chat (default, recommended)
- `"local"` → save to `~/.hermes/cron/output/` only (không gửi Telegram)
- `"all"` → fan out to all connected home channels
- `"platform:chat_id:thread_id"` → specific destination
