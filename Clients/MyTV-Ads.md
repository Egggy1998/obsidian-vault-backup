---
type: client
platform: facebook-ads
created: 2026-06-05
source: Telegram DM with Hồng Quảng
---

# MyTV — Truyền Hình MyTV (FIFA World Cup 2026 Ads)

## Client
- **Brand**: Truyền Hình MyTV
- **Campaign theme**: Đồng hành cùng VTV lan tỏa FIFA World Cup 2026
- **Vertical**: Streaming / TV
- **Platforms**: Facebook Ads (primary), engagement + clicks-to-web objectives

## Campaign Name Convention (full names, no abbreviation)
- `ABO_FA_Engagement_TRUYỀN HÌNH MyTV ĐỒNG HÀNH CÙNG VTV LAN TỎA FIFA WORLD CUP 2026`
- `ABO_FA_Engagement_TRUYỀN HÌNH MyTV ĐỒNG HÀNH CÙNG VTV LAN TỎA FIFA WORLD CUP 2026 - Bản sao`
- `DucVH_Clicks to Web_MyTV TIẾP LỬA WORLD CUP 2026™: X2 ƯU ĐÃI – TRÚNG QUÀ KHỦNG HƠN 3 TỶ ĐỒNG!`
- `DucVH_Clicks to Web_MyTV TIẾP LỀA WORLD CUP 2026™: X2 ƯU ĐÃI – TRÚNG QUÀ KHỦNG HƠN 3 TỶ ĐỒNG! - Bản sao`
- `DucVH_Clicks to Web_MyTV TIẾP LỀA WORLD CUP 2026™: X2 ƯU ĐÃI – TRÚNG QUÀ KHỦNG HƠN 3 TỶ ĐỒNG! - Bản sao 2`
- `DucVH_KHTN_Clicks to Web_MyTV TIẾP LỀA WORLD CUP 2026™: X2 ƯU ĐÃI – TRÚNG QUÀ KHỦNG HƠN 3 TỶ ĐỒNG!`
- `FA_Engagement_TRUYỀN HÌNH MyTV ĐỒNG HÀNH CÙNG VTV LAN TỎA FIFA WORLD CUP 2026`
- `FA_Engagement_TRUYỀN HÌNH MyTV ĐỒNG HÀNH CÙNG VTV LAN TỎA FIFA WORLD CUP 2026 - Bản sao`

## Data Sources
### Google Sheets
- **Sheet mytv01** (English headers, 12 cols) — GID `1309668675`
  - Spreadsheet ID: `1CyWkzLTD80_y-wcGfNPLJChwLA8VHJ1mhwm_4hT_VMM`
  - URL: https://docs.google.com/spreadsheets/d/1CyWkzLTD80_y-wcGfNPLJChwLA8VHJ1mhwm_4hT_VMM/edit?gid=1309668675
  - Public CSV export (no auth required)
  - Header row at index 1 (2 title rows above)
  - Columns: `Campaign name | Amount spent | Impressions | Reach | Clicks (all) | CPC (all) | Results | Cost Per Result | Post engagements | Follows or likes | Link clicks | Day`
- **Sheet mytv02** (Vietnamese headers, 22 cols) — GID `249834428`
  - Spreadsheet ID: `1vCQ8v-pfuz2xaEy5AFrty2cPczc-DECT_5WtF6INU2U`
  - URL: https://docs.google.com/spreadsheets/d/1vCQ8v-pfuz2xaEy5AFrty2cPczc-DECT_5WtF6INU2U/edit?gid=249834428
  - Public CSV export
  - Header row at index 0 (no title rows)
  - Columns: `Tên chiến dịch | Ngày | Trạng thái phân phối | Cấp độ phân phối | Người tiếp cận | Lượt hiển thị | Tần suất | Loại kết quả | Kết quả | Chi phí trên mỗi kết quả | Số tiền đã chi tiêu (VND) | Bắt đầu | Kết thúc | CPM (Chi phí trên mỗi 1.000 lượt hiển thị) | Lượt click vào liên kết | CPC (chi phí trên mỗi lượt click vào liên kết) | CTR (tỷ lệ click vào liên kết) | (Mẫu tìm kiếm) khách hàng tiềm năng | Lượt thích trên Facebook | Lượt tương tác với bài viết | Lượt bắt đầu báo cáo | Lượt kết thúc báo cáo`

### Dashboard (Lovable)
- **PnL Metrics API** (client-managed by anh Mi béo)
  - Base URL: `https://project--adf533f8-8fa0-45da-a275-558aaef1e264.lovable.app/api/public/metrics`
  - No auth required (public endpoint)
  - ⚠️ Lovable blocks default Python User-Agent — MUST set browser UA header

## Field Mapping (sheet → dashboard)
| Dashboard field | Sheet mytv01 col | Sheet mytv02 col | Notes |
|---|---|---|---|
| `campaign` | Campaign name | Tên chiến dịch | full name, no abbreviation |
| `metric_date` | Day | Ngày | format YYYY-MM-DD |
| `viewers` | Reach | Người tiếp cận | |
| `reach` | Reach | Người tiếp cận | duplicate of viewers |
| `impressions` | Impressions | Lượt hiển thị | |
| `engagement` | Post engagements | Lượt tương tác với bài viết | |
| `link_clicks` | Link clicks | Lượt click vào liên kết | |

## Decisions
- 2026-06-05: Use **full campaign names** (no abbreviation) per anh's instruction
- 2026-06-05: `viewers` = **Reach** (Người tiếp cận), NOT Impressions
- 2026-06-05: Push **7 fields** (full schema) — not just 5 — dashboard DB has `reach` + `impressions` columns too

## Related
- [[Workflows/MyTV-Sheet-Sync]] — sync procedure
- [[Tools/PnL-Dashboard]] — dashboard API spec
