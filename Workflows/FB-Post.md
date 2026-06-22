---
type: workflow
category: content
created: 2026-06-01
---

# FB Post Workflow (EvaFamily)

## Pipeline
```
Baserow content row → n8n webhook → SA Designer (image) → SA Poster (publish) → Baserow update
```

## Step 1: Webhook trigger
- URL: `https://n8n11.ezn8n.com/webhook/evafamily`
- Trigger: New row in Baserow content calendar (status = "ready")
- Payload MUST include `row_id` at root (`row_id` / `id` / `baserow_row_id`)

## Step 2: n8n → SA Designer
- SA Designer đọc row, generate image theo brief
- Update `Link ảnh đã thiết kế` trong Baserow row

## Step 3: SA Poster
- Download image từ Baserow
- Compose title + content (Facebook ignores `description` → **combine vào `title`**)
- POST to FB Graph API
- Update Baserow với post URL

## Rules
- Mỗi row = 1 POST riêng, **KHÔNG gộp batch**
- Verify webhook status trước khi gửi batch
- Verify product images trước khi gửi SA Designer
- **KHÔNG rewrite content** — chỉ generate từ brief đã có
- KHÔNG tự viết content thay vì spawn SA Writer
