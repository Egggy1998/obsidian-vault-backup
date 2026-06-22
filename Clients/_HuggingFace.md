---
type: system_account
platform: huggingface
created: 2026-06-08
source: Telegram DM with Hồng Quảng
---

# Hugging Face — Egggy

## Account
- **Username**: `Egggy` (id `65dea91c04b9352580df04a4`)
- **Fullname**: Hong Quang
- **Tier**: Free (isPro=false)
- **canPay**: false | **billingMode**: prepaid
- **Orgs**: [] (no orgs)
- **Account created**: 2026-06-08 09:05:18 UTC (tạo cùng ngày với token)

## Personal Access Token

**File**: `~/.hermes/secrets/huggingface.env` (chmod 600, owner-only)
**Key**: `HUGGINGFACE_ACCESS_TOKEN`
**Type**: Fine-grained PAT (`hf_` prefix) — better than classic, scoped
**Display name**: `claude` (user đặt tên theo assistant tạo)
**Created**: 2026-06-08 09:05:18 UTC

### Scopes (verify lại trước khi dùng nặng)

**User-level (self — `Egggy`)**:
- `repo.content.read`, `repo.access.read`, `repo.write` — full repo CRUD
- **`inference.serverless.write`** ⚠️ — paid API inference, check cost
- **`inference.endpoints.infer.write`, `inference.endpoints.write`** ⚠️ — can deploy paid endpoints
- `user.webhooks.read/write` — webhook management
- `collection.read/write`
- `discussion.write`
- `user.billing.read` — đọc billing
- `job.write` — run jobs
- `user.notifications.read/write`

**Space-level (`Egggy/Egggy` personal Space)**:
- `repo.content.read`, `repo.access.read`
- `discussion.write`
- `repo.write`

⚠️ **Security notes**:
- Free tier + có `inference.*.write` permissions → dùng nặng có thể tốn tiền (HF charge cho API calls ngoài free quota)
- Fine-grained tốt hơn classic (scoped) — vẫn nên review định kỳ
- `user.billing.read` cho phép đọc billing — fine, nhưng nếu lộ → lộ usage history
- File `chmod 600` — không move, không commit
- KHÔNG paste trong chat log, KHÔNG log trong CI

## Validation
```bash
curl -sS -H "Authorization: Bearer $HUGGINGFACE_ACCESS_TOKEN" \
  https://huggingface.co/api/whoami-v2
```

## Where token is used
- **HF Inference API** — chạy model serverless (Mistral, Llama, SDXL, ...) qua `https://api-inference.huggingface.co/models/<model>`
- **HF Datasets** — download/upload dataset
- **HF Models** — push/pull model weights
- **HF Spaces** — deploy/manage Space (đã có Space `Egggy/Egggy`)
- **HF Jobs** — chạy training job trên cloud
- **HF Inference Endpoints** — deploy dedicated endpoint (paid)

## Cost heads-up
- Free tier inference có quota giới hạn (chỉ một số model, rate-limited)
- Nếu muốn dùng serverless inference nặng → upgrade Pro ($9/tháng) hoặc dùng API provider khác (Replicate, fal.ai, OpenRouter)
- Inference Endpoints từ $0.06/hr (CPU) đến $3+/hr (GPU A10G)

## Related
- `hf` Python library: `pip install huggingface_hub`
- Login CLI: `huggingface-cli login --token $HUGGINGFACE_ACCESS_TOKEN`
- `mlops/huggingface-hub` skill trong Hermes (download/upload)
- `mlops/evaluation/evaluating-llms-harness` (nếu dùng cho benchmark)
