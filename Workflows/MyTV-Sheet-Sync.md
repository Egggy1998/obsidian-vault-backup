---
type: workflow
category: data-sync
created: 2026-06-05
---

# MyTV Sheet → PnL Dashboard Sync

## Pipeline
```
Google Sheet (mytv01 or mytv02) → CSV export → parse → DELETE all dashboard → POST batch → verify
```

## When to run
- Mỗi khi anh Mi béo gửi link sheet mới hoặc bảo "update lại"
- Cadence: ad-hoc (manual trigger via Telegram)

## Step 1: Detect sheet schema
- mytv01 (GID `1309668675`): 12 cols, English, **2 title rows** above header
- mytv02 (GID `249834428`): 22 cols, Vietnamese, **no title rows**
- Use header-name lookup (NOT column index) for robustness across schema changes

## Step 2: DELETE all current dashboard data
```python
import urllib.request, json
API = "https://project--adf533f8-8fa0-45da-a275-558aaef1e264.lovable.app/api/public/metrics"
UA = "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 Chrome/120.0 Safari/537.36"

# GET all
req = urllib.request.Request(API, headers={"User-Agent": UA, "Accept": "application/json"})
data = json.loads(urllib.request.urlopen(req).read())["data"]

# DELETE each by id
for r in data:
    req = urllib.request.Request(f"{API}?id={r['id']}", method="DELETE",
        headers={"User-Agent": UA})
    urllib.request.urlopen(req).read()
```

## Step 3: Read sheet CSV
```python
import urllib.request, csv, io
url = "https://docs.google.com/spreadsheets/d/{ID}/export?format=csv&gid={GID}"
req = urllib.request.Request(url, headers={"User-Agent": UA})
raw = urllib.request.urlopen(req).read().decode("utf-8-sig")
rows = list(csv.reader(io.StringIO(raw)))
# Skip title rows: mytv01 = 2, mytv02 = 0 (auto-detect by finding row where col 0 = "Campaign name" or "Tên chiến dịch")
```

## Step 4: Build records (7 fields)
```python
{
  "campaign": row["Tên chiến dịch"],       # or "Campaign name"
  "metric_date": row["Ngày"],              # or "Day"
  "viewers":     reach_value,              # Người tiếp cận / Reach
  "reach":       reach_value,              # same as viewers
  "impressions": row["Lượt hiển thị"],     # or "Impressions"
  "engagement":  row["Lượt tương tác với bài viết"],  # or "Post engagements"
  "link_clicks": row["Lượt click vào liên kết"],       # or "Link clicks"
}
```

## Step 5: POST batch (chunk 50)
```python
req = urllib.request.Request(API, data=json.dumps(batch).encode(),
    method="POST",
    headers={"Content-Type":"application/json","User-Agent": UA})
```

## Step 6: Verify
- GET all → count = expected
- Distinct (campaign, metric_date) pairs = count
- Spot check: `reach` and `impressions` > 0 on ≥ 95% records (some may be legitimately 0)

## Pitfalls
- **Lovable blocks default Python UA** → 403. Always send browser UA
- **VN number format** `"1.234.567"` = 1234567 → strip dots, commas, NBSP
- **Empty cells** → 0 (not error)
- **No title rows** in mytv02 — header is row 0
- **2 title rows** in mytv01 — find header by `col[0] == "Campaign name"`
- **Schema can change** (mytv02 went 22 → 17 cols on 2026-06-05) — always use header-name lookup, not column index
- **Duplicate (campaign, date) rows** in same sheet — multiple ad sets per day. Postgres `ON CONFLICT` 500s if 2 rows in batch share the same key. **Aggregate by SUM** before POST
- Dashboard DB has 7 metric fields (campaign, metric_date, viewers, engagement, link_clicks, reach, impressions) — push ALL, not just the 5 from the curl spec
- **Dashboard is empty after delete** if POST fails — re-run POST after fix, no need to re-delete

## Aggregation pattern (2026-06-05)
```python
from collections import defaultdict
agg = defaultdict(lambda: {"viewers":0,"reach":0,"impressions":0,
                           "engagement":0,"link_clicks":0})
for row in data_rows:
    key = (row[col["Tên chiến dịch"]], row[col["Ngày"]])
    a = agg[key]
    a["viewers"]     += to_int(row[col["Người tiếp cận"]])
    a["impressions"] += to_int(row[col["Lượt hiển thị"]])
    a["engagement"]  += to_int(row[col["Lượt tương tác với bài viết"]])
    a["link_clicks"] += to_int(row[col["Lượt click vào liên kết"]])
    a["reach"] = a["viewers"]   # same source col as viewers
records = [{"campaign":k[0],"metric_date":k[1], **v} for k,v in agg.items()]
```

## Related
- [[Clients/MyTV-Ads]] — campaign names, sheet IDs, field mapping
- [[Tools/PnL-Dashboard]] — API spec & common mistakes
