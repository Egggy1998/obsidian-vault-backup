# Nitro Beverage Co

**Status:** ❌ **DROPPED 2026-06-14** — User quyết định bỏ qua, không chốt deal.
Samples delivered 2026-06-01 nhưng không close được.

## Company
- **Industry:** Nitrogen beverage machine manufacturer (compete với Sodastream)
- **Product:** Máy pha chế đồ uống nitơ, hiệu ứng "creamy" (không bubbling như soda)
- **Reference style:** https://hatfieldslondon.com/blogs/recipes
- **Contact:** Anonymous (chưa có tên, liên hệ qua Telegram forward từ user)

## Brief
- **Nhu cầu:** ~30 ảnh recipe (nitro coffee + variants) cho marketing
- **Test order:** 2 sample ảnh trước
  1. Nitro coffee: creamy foam top + wavy liquid + color gradient (darkest at bottom)
  2. Nitro coffee với chocolate sauce quanh thành cốc
- **Watermark:** Được phép dùng (bảo vệ quyền talent)
- **Deliverable:** Chỉ image, KHÔNG cần giao prompt
- **Revisions:** Có bao gồm trong 30 ảnh
- **Pricing:** Khách estimate, talent tự đề xuất

## Pricing Strategy
| Mức | Per image | 30 ảnh | Ghi chú |
|-----|-----------|--------|---------|
| Low (AI-gen, batch) | $15-25 | $450-750 | Volume play, no human touch |
| Mid (AI-gen + manual edit) | $30-50 | $900-1500 | Reasonable cho recipe work |
| High (pro photographer style) | $80-150 | $2400-4500 | Premium nếu khách cần brand-quality |

**Recommend:** $30-50/image = $900-1500 total cho 30 ảnh (sweet spot, AI-gen + 1-2 revisions).

## Pipeline
- **Model:** RunningHub `rhart-image-n-pro/text-to-image` (全能图片PRO / Nano Banana Pro) — chất lượng cao nhất cho ảnh tĩnh
- **Cost:** ¥0.06/image (~$0.008) — chạy 30 ảnh ≈ ¥1.80 = $0.25
- **Aspect ratio:** 9:16 portrait (recipe vertical)
- **Resolution:** 2K
- **Watermark:** Diagonal "SAMPLE" + small "© Sample 2026" bottom-right (PIL script `/home/node/tmp/watermark_nitro.py`)
- **Output folder:** `/home/node/clients/nitro-beverage/`

## Samples (2026-06-01)
1. `sample_1_nitro_creamy.jpg` (436 KB, 1536x2752) — creamy + wavy + gradient
2. `sample_2_nitro_chocolate.jpg` (452 KB, 1536x2752) — same + chocolate sauce
3. Raw files: `/home/node/tmp/rh-output/nitro-beverage/`
4. Watermark script: `/home/node/tmp/watermark_nitro.py` (reusable cho batch 30)

## Style Guide (cho 30 ảnh recipe)
- Clear glass cup (tall, transparent) — showcase gradient
- Nitro coffee color palette: pale caramel → amber → honey → dark chocolate
- Foam: dense, velvety, micro-bubbles (NOT bubbly espresso crema)
- Background: clean marble hoặc soft bokeh cream/beige
- Lighting: natural window light, soft, side-lit
- Mood: recipe book, magazine quality, warm/inviting
- Wavy/swirling pattern trong liquid — key visual signature
- Chocolate variant: drizzle outside glass, pool at base

## Next Steps
- [x] ❌ 2026-06-14: User decided to drop — not closing. Archive và move on.

## Reference Notes (giữ lại cho reference, không pursue)
- Customer said "around 30 images (with revisions)" → expect 1-2 revision rounds
- "Final output is only image (no need to offer the prompt)" → keep prompts internal
- "The price in the post is just estimation" → competitive bid wins
