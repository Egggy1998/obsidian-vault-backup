# EvaFamily 30-day Pixar 3D Batch Pipeline — Decision (2026-06-04)

## Goal
Generate 29 educational mom-baby videos (Day 2-30) for EvaFamily in **Pixar 3D style** with **Vietnamese VO**, **character consistency**, **45s each**, **meaningful + educational**, continuing from Day 1 already-produced pilot.

## Account
- **AutoVideo.app account 147** "Flow - autovideo.app" — 24,330 credits starting balance, expires 2026-06-04 21:07 UTC (≈ 04:07 sáng 2026-06-05 VN)
- Login email: `teandean1998@gmail.com` (credentials in `~/.hermes/auth/autovideo.md` chmod 600)

## Pipeline Architecture (final, working)
1. **Master reference image** (Pixar 3D, Vietnamese mom + baby) → upload as `attachment_id` to autovideo.app
2. **Per scene**: `IMAGE_TO_VIDEO` with master ref + scene prompt + embedded Vietnamese VO in prompt
3. **Per day**: concat 6 scenes via `/api/ai/google/video/concatenation` → 1 final video
4. **Output**: 6 × 8s = 48s per day (close to 45s target)

## Master Reference Spec
- **Model**: GEM_PIX_2 (Google Imagen 2 via autovideo.app)
- **Subject**: Vietnamese mother late-20s, black bob hair with bangs, mint green knit cardigan over cream T-shirt, pinkish-beige cotton pants
- **Baby**: cream cotton swaddle, sleeping, closed eyes
- **Style**: Pixar 3D, subsurface scattering skin, soft volumetric lighting, modern VN apartment, pastel color palette
- **Aspect**: 9:16 portrait, 768×1376
- **attachment_id**: 64899
- **Stored at**: `/home/node/Media/evafamily-batch-2026-06-04/master_ref/master_reference_v1.jpg`
- **CDN URL**: `https://cdn.autovideo.app/attachments/20260604/08a16ffe1984437016c2629139cbfab48b76972f.jpg`

## Model Choice: Veo 3.1 - Fast (I2V)
**Why Fast over Quality/Omni Flash:**
- ✅ **I2V support**: Only Veo 3.1 family supports `IMAGE_TO_VIDEO` + `attachmentIds`. Omni Flash is T2V only (`abra_t2v_6s`).
- ✅ **Character lock**: I2V with master ref → consistent character across 174 scenes
- ✅ **Cost/time**: Fast ≈ 1.25 min/scene; Quality ≈ 2.5 min/scene. Batch = 4.5h vs 9h
- ✅ **Pixar 3D**: Prompt-driven style. Master ref is Pixar, Fast I2V preserves look
- ⚠ Trade-off: Quality slightly more polished but Fast is "đẳng cấp" enough with master ref

**Internal model IDs observed:**
- `veo_3_1_i2v_s_fast_6s` — 6s duration
- `veo_3_1_i2v_s_fast_portrait_ultra` — 8s duration
- `veo_3_1_t2v_quality_6s` — T2V Quality
- `veo_3_1_t2v_fast_*` — T2V Fast (no I2V)
- `abra_t2v_6s` — Omni Flash T2V (no I2V)

## Per-Scene Prompt Structure (working template)
```
Scene {N} of 6 from day {D} of EvaFamily educational baby care series.
Same Vietnamese mother character from reference image (black bob hair with bangs,
mint green knit cardigan over cream T-shirt, pinkish-beige cotton pants).
Baby in cream cotton swaddle, sleeping peacefully with closed eyes.
Specific action: {STORYBOARD_DURATION_DESC}
Pixar-style 3D animated family-film, high-end cinematic quality,
soft pastel modern Vietnamese apartment setting, soft warm volumetric lighting,
subsurface scattering on skin, 9:16 vertical frame,
no logos, no foreign text, no traditional or formal clothing, family-friendly, heartwarming.
Add embedded Vietnamese motherly narration: the mother speaks in a warm,
reassuring, expert-but-tender Vietnamese motherly voice, saying clearly in Vietnamese:
'{VO_LINE_FOR_THIS_SCENE}'
No foreign language. No on-screen text.
```

## Per-Scene API Call
```json
POST /api/ai/google/video/generate
Headers: Authorization: Bearer ***  "application/json"
{
  "generationMode": "IMAGE_TO_VIDEO",
  "recaptchaToken": "TEST_PLACEHOLDER_TOKEN",
  "requests": [{
    "prompt": "...",
    "model": "Veo 3.1 - Fast",
    "aspectRatio": "VIDEO_ASPECT_RATIO_PORTRAIT",
    "attachmentIds": [64899],
    "duration": 8
  }]
}
```

## Concat API
```json
POST /api/ai/google/video/concatenation
{
  "recaptchaToken": "TEST_PLACEHOLDER_TOKEN",
  "inputVideos": [
    {"itemId": 12345, "lengthNanos": 8000000000, "startTimeOffset": "0s", "endTimeOffset": "8s"},
    ... 6 total
  ]
}
```

## Batch Stats
- 29 days × 6 scenes = 174 scene generations
- 29 concat operations
- ~1.25 min per scene × 174 = 217 min = 3.6h
- + 29 concats × ~30s = ~15 min
- **Total: ~3.5-4 hours**

## Output Specs (observed from test scene)
- Codec: H.264 video + AAC audio
- Resolution: 720×1280 (9:16)
- Duration: 6s or 8s per scene
- Audio: 48kHz stereo, ~158 kbps
- File size: 2-3 MB per 6s scene
- Audio contains auto-generated Vietnamese VO from prompt

## Files & Directories
- Workspace: `/home/node/Media/evafamily-batch-2026-06-04/`
- Master ref: `master_ref/master_reference_v1.jpg`
- Pipeline data: `days_data.json` (180 scenes parsed from 3 plan files)
- Pipeline script: `batch_pipeline.py` (state-resumable, with skip-day-1)
- Final videos: `finals/Day{NN}.mp4`
- State file: `state.json` (incremental save, can resume)
- Test output: `master_ref/test_scene_day2_s1.mp4` (verified working)

## Open Risks
- [ ] Single scene test took 75s. Batch avg may vary. 4h estimate could be 5-6h.
- [ ] Veo 3.1 Fast I2V has slight drift in non-master-ref elements (background, props) per scene — acceptable for educational content
- [ ] Audio: each scene generates own audio. Concat joins them. Risk of slight volume/tone mismatch at scene boundaries.
- [ ] Account expires today 04:07 sáng VN. If batch runs long, need to refresh token or risk interruption.

## Safety Notes
- Test scene (Day 2 Scene 1) PASSED RAI, no artifacts, no text, no logos
- Character consistent with master ref ✓
- Vietnamese motherly narration embedded ✓
- All scenes include "no logos, no foreign text" guard

## Next Steps
1. Verify Day 2 full pipeline test (6 scenes + concat) — currently running
2. If test succeeds, kick off full batch Day 2-30 in background (~3.5-4h)
3. After batch done: upload to FB via existing n8n webhook `https://n8n11.ezn8n.com/webhook/evafamily`
4. Update `autovideo-api` skill with master ref / I2V / concat findings
5. Save 30 video URLs to Obsidian + cron for scheduled posting
