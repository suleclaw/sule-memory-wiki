---
title: AI News Shorts Pipeline
created: 2026-04-06
updated: 2026-04-08
tags: [ai-news, youtube, pipeline, automation]
related: [[q2-goals-2026]]
---

# AI News Shorts Pipeline

## Overview

"AI In 90 Seconds" YouTube Shorts channel — automated daily AI news video pipeline.
Channel: [AI In 90 Seconds](https://www.youtube.com/channel/UC9iF_DF6Vcu6vTNUycgvxyQ) | ID: UC9iF_DF6Vcu6vTNUycgvxyQ

---

## Two Pipelines (Transition in Progress)

### Old Pipeline — `~/sule-memory/ai-news-automation/` ❌ BROKEN
- **Stack:** Node.js + MiniMax TTS + MiniMax Images + NemoVideo
- **Problem:** MiniMax TTS credits exhausted (status_code 1008 since ~2026-04-06)
- **Last successful run:** 2026-04-05
- **Failing step:** TTS generation (no credits left)
- **Fix needed:** Switch to new pipeline below

### New Pipeline — `~/projects/ai-shorts-pipeline/` ✅ READY
- **Stack:** Python + Gemini (script + Imagen 3 images) + Edge TTS (free!) + Whisper + FFmpeg
- **Status:** Fully built and tested (2026-04-08)
- **Key advantage:** Edge TTS is completely free — no API key needed, 300+ voices
- **Config:** `config.yaml`

---

## New Pipeline Specs

**Stage details:**

| Stage | Provider | Cost | Notes |
|-------|----------|------|-------|
| Research | DuckDuckGo + scrape | Free | Anti-hallucination gate |
| Script | Gemini (`apiyi` key) | Free tier | Hook-driven, niche-aware |
| Visuals | Gemini Imagen 3 (`nano-banana`) | Free tier | 9:16 portrait, dark/neon |
| Ken Burns | FFmpeg | Free | Zoom/pan animation on stills |
| Voice | Edge TTS | **Free** | 300+ voices, no API key |
| Captions | Whisper (local) | Free | Word-level ASS burn-in |
| Music | Royalty-free | Free | Bundled tracks |
| Assemble | FFmpeg | Free | B-roll + VO + captions + ducking |
| Upload | YouTube Data API v3 | Free | Private by default |

**Image generation model:** `nano-banana` (Gemini's image model via APIYI provider)
**Script model:** Gemini via APIYI (`${APIVI_API_KEY}`)

---

## Configuration

```yaml
# ~/projects/ai-shorts-pipeline/config.yaml

youtube:
  client_id: "${YOUTUBE_CLIENT_ID}"
  client_secret: "${YOUTUBE_CLIENT_SECRET}"
  refresh_token: "${YOUTUBE_REFRESH_TOKEN}"
  channel_id: "UC9iF_DF6Vcu6vTNUycgvxyQ"

gemini:
  api_key: "${GEMINI_API_KEY}"

apiyi:
  api_key: "${APIVI_API_KEY}"

pipeline:
  niche: ai_news
  voice: en-GB-SoniaNeural
  aspect_ratio: "9:16"
  output_resolution: "720x1280"
  upload_privacy: "private"
  speed: 1.1
```

---

## Usage

```bash
# Run full pipeline for a topic
cd ~/projects/ai-shorts-pipeline
source .venv/bin/activate
python cli.py run --topic "OpenAI raises $122 billion"

# Dry run (skip upload)
python cli.py run --topic "Claude Code leaked" --dry-run

# Use specific venv
python cli.py run --topic "AI funding Q1 2026" --venv /root/projects/ai-shorts-pipeline/.venv
```

---

## Cron Setup (Daily Automation)

Needs a daily cron job to run the new pipeline instead of the broken `daily-ai-news.js`.

Suggested crontab entry:
```
0 7 * * * cd ~/projects/ai-shorts-pipeline && source .venv/bin/activate && python cli.py run --topic "AI news today" >> ~/sule-memory/ai-news-automation/logs/pipeline_$(date +\%Y-\%m-\%d).log 2>&1
```

---

## Edge TTS Voice Options

| Voice | Description |
|-------|-------------|
| `en-GB-SoniaNeural` | British female, warm professional |
| `en-US-GuyNeural` | American male, news anchor |
| `en-AU-NatashaNeural` | Australian female, clear |
| `en-GB-ThomasNeural` | British male, serious |
| `en-US-SaraNeural` | American female, friendly |

Current default: `en-GB-SoniaNeural` at 1.1x speed (news delivery pace)

---

## Status History

| Date | Event |
|------|-------|
| 2026-04-02 | Old pipeline first created |
| 2026-04-05 | Last successful old pipeline run |
| 2026-04-06 | MiniMax TTS credits exhausted — old pipeline breaks |
| 2026-04-07 | PrintsbyTee MVP shipped, trading bot launched |
| 2026-04-08 | New Python pipeline built and tested with APIYI + nano-banana |
| 2026-04-08 | Dami tested APIYI + nano-banana for image generation — confirmed working |
| 2026-04-08 | New pipeline wired to cron — replaced broken `daily-ai-news.js` |

---

## Next Steps

1. **Wire new pipeline to daily cron** — replace the broken `daily-ai-news.js`
2. **Test full end-to-end** — run new pipeline dry-run and verify output quality
3. **Switch cron** from `daily-ai-news.js` to `cli.py run` with new pipeline
4. **Decommission old pipeline** — archive after new pipeline is running daily

---

## Reference

- Reference repo: https://github.com/rushindrasinha/youtube-shorts-pipeline
- Old pipeline: `/root/sule-memory/ai-news-automation/daily-ai-news.js`
- YouTube OAuth credentials: reused from old pipeline config