# Neon.tech (NeonTech)

**Status:** 🆕 Added 2026-06-07 — minimal context, expand after user clarifies org/project/region
**API Key:** `napi_ybf4fzhk9ppfuy4awk9s3h92e5h1s5zzgtqmm4sc4nne9gfl0qgmn8us1b95y2o2`
**Key type:** Neon Personal API Key (`napi_...`) — account-scoped
**Storage:** `/home/node/.neontech-credentials` (chmod 600) + this note (cross-agent)

> ⚠️ Security: token in private repo cho cross-agent sharing. Rotate tại https://console.neon.tech/app/settings/api-keys nếu nghi ngờ lộ.

## Context cần confirm với user
- **Org ID:** _pending_ (key auth OK, nhưng `/api/v2/projects` cần `?org_id=...` để list projects)
- **Project ID:** _pending_
- **Region:** _pending_ (aws-us-east-1, aws-ap-southeast-1, aws-eu-central-1...?)
- **Use case:** _pending_ (đang chạy production DB nào? dùng với app gì?)

## Account
- Email: _pending — xác nhận với user_
- Plan: _pending_
- 2FA: _pending — recommend bật_

---

## API Quick Reference

```
Base URL: https://console.neon.tech/api/v2
Auth: Authorization: Bearer napi_...
```

## Verified endpoints (2026-06-07)
| Endpoint | Method | Status |
|----------|--------|--------|
| `/api/v2/projects?org_id=...` | GET | ⚠️ cần `org_id` query param |

**Chưa verify được các endpoint khác** vì thiếu `org_id`:
- `/api/v2/projects/{project_id}/endpoints` (list connection endpoints — hostname, port)
- `/api/v2/projects/{project_id}/branches` (list branches)
- `/api/v2/projects/{project_id}/connection_uri` (lấy full `postgresql://...` URI)

## Connection string format (chuẩn Neon)
```
postgresql://<user>:<password>@<endpoint-hostname>.neon.tech/<dbname>?sslmode=require
```
Neon enforce SSL, luôn cần `?sslmode=require` hoặc `sslmode=verify-full`.

## Load trong scripts

### Cách 1: từ Obsidian (cho AI agents không có filesystem)
```python
import os
NEON_KEY = os.environ.get(
    "NEON_API_KEY",
    "napi_ybf4fzhk9ppfuy4awk9s3h92e5h1s5zzgtqmm4sc4nne9gfl0qgmn8us1b95y2o2"
)
```

### Cách 2: từ file secure (preferred)
```python
import json, pathlib
creds = json.loads((pathlib.Path.home() / '.neontech-credentials').read_text())
NEON_KEY = creds['api_key']
NEON_ORG_ID = creds.get('org_id')  # set sau khi user cung cấp
```

### Cách 3: connection string cho app
```python
import json, pathlib, os
creds = json.loads((pathlib.Path.home() / '.neontech-credentials').read_text())
DATABASE_URL = os.environ['DATABASE_URL']  # lưu full connection string ở env, KHÔNG commit
# Hoặc build từ components: postgresql://<user>:<pw>@<host>:<port>/<db>?sslmode=require
```

## Rotation
- Dashboard: https://console.neon.tech/app/settings/api-keys
- Update cả 2 chỗ: file + note này
- Recommend enable 2FA: https://console.neon.tech/app/settings/profile

## Lưu ý quan trọng
- **KHÔNG BAO GIỜ** commit `DATABASE_URL` (full connection string có password) lên repo
- File này chỉ chứa API key (dùng để gọi Neon API), KHÔNG phải DB connection string
- Nếu cần lưu connection string → dùng `.env` (chmod 600) hoặc env vars của deployment platform
