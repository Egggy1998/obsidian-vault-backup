---
type: skill
name: event-harness
created: 2026-06-08
owner: Hồng Quảng / Babyboo
status: v1.0.0
domain: vietnam-marketing-events
---

# Skill: event-harness

## What
Procedural harness cho AI agent tổ chức sự kiện marketing offline tại VN. Khi agent nhận task "lên kế hoạch tổ chức sự kiện", skill wrap agent đó để output 1 file event brief 10 phần.

## File
- **Local**: `/home/node/.hermes/skills/event-harness/SKILL.md` (24KB, 447 dòng)
- **Pattern**: Procedural harness (giống `ai-ugc-ads` của chadboyda/agent-gtm-skills)
- **License**: CC-BY-4.0

## The 10-step harness
1. Brief intake (1-pager anchor)
2. Design asset list (print lớn / vừa / nhỏ / POSM / sample)
3. Work breakdown (RACI matrix 15 roles)
4. Mini-game concepts (6 mẫu: vòng quay / lucky draw / quiz / check-in / vận động / workshop)
5. Vendor & contact list (14 categories)
6. Timeline (T-30 → T+7 + Event Day checklist)
7. Budget breakdown (% theo hạng mục + 10% contingency)
8. Promotional plan (pre / during / post multi-channel)
9. Risk & compliance VN (sữa, TPCN, ngoài trời, PDPA, lao động)
10. KPI & post-event report template

## Trigger phrases
- "lên kế hoạch tổ chức sự kiện"
- "tổ chức activation / sampling / booth / hội chợ / workshop"
- "ra mắt sản phẩm tại..."
- "event brief / event runbook / event playbook"
- "mini game sự kiện / tờ rơi / standee / banner cho event"
- "đầu mối công việc sự kiện"
- "agency brief cho event"
- "showroom khai trương / activation sân khấu"

## Brand context được hardcode
- **EvaFamily (Nutura Organic)**: dùng đúng "Stage 1/2/3" + "Smoothie Cognitive/Calm/Immunity". KHÔNG bịa tên SP.
- **Baby A&M**: mẹ bỉm sữa → cần consent form
- **MyTV-Ads**: B2B
- **3D Laser**: cần PCCC check + bảo hiểm thiết bị

## VN compliance đã cover
- Nghị định 100/2014/NĐ-CP (quảng cáo sữa — sửa đổi 2022)
- Quảng cáo TPCN (giấy phép Sở Y tế, disclaimer)
- Sự kiện ngoài trời (UBND phường, Sở VH-TT)
- Nghị định 13/2023/NĐ-CP (PDPA — consent thu thập data)
- Lao động part-time

## Integrations với skills hiện có
- `aitoearn` — draft post FB/TikTok pre/during/post event
- `ai-ugc-ads` — brief KOL theo UGC format
- `autovideo-api` — quay highlight reel
- `ai-media-production-workflows` — hậu kỳ ảnh + video
- `topviewai-skill` — talking head testimonial
- `runninghub` — render ảnh SP branding nhanh
- `image-edit` — sửa asset in (đổi số KOL...)
- `composio-app-integrations` — FB/IG/TikTok post qua MCP

## Examples đã viết
1. EvaFamily sampling tại Aeon Mall (30tr, 300 khách, mom có con 2-6 tuổi)
2. 3D Laser showroom khai trương (80tr, 150 doanh nghiệp)

## Pitfalls đã ghi rõ (10 anti-patterns)
Skip intake, in ấn trễ, quên compliance sữa, KOL không brief, không backup, sample sai date, không đếm KPI, bỏ PDPA, bịa tên SP, không có T+7 report.

## Next steps
- [ ] Test với 1 brief thật (vd sampling EvaFamily Stage 3 cho mẹ bỉm)
- [ ] Nếu OK → push lên GitHub vault `Egggy1998/quang-obsidian-vault` (repo system)
- [ ] Nếu nhiều brand dùng → extract vendor list thành 1 file riêng `references/vendors-vn.md`
- [ ] Viết eval set 3-5 brief test để benchmark

## Related
- AGENTS.md → "Self-Improving Agent" + "Obsidian Save Protocol"
- `tech-leads-club/gtm/ai-ugc-ads` — inspiration pattern
- `tech-leads-club/creation/skill-architect` — methodology (Discovery → Architecture → Craft → Validate → Deliver)
