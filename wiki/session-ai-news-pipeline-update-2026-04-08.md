---
title: Session — AI News Pipeline Update
created: 2026-04-08
updated: 2026-04-08
tags: [session, ai-news, youtube, pipeline]
related: [[youtube-pipeline]], [[q2-goals-2026]]
---

# Session — AI News Pipeline Update

**Date:** 2026-04-08
**Channel:** Discord #general
**Duration:** ~16:11 – 16:29 UTC

---

## Summary

Dami asked to:
1. Update the wiki on the AI news pipeline situation
2. Find where APIYI + nano-banana was tested
3. Wire the new pipeline into the daily cron

---

## What Was Found

### APIYI + nano-banana
- Located in `~/projects/ai-shorts-pipeline/config.yaml`
- `apiyi.api_key: "${APIVI_API_KEY}"`
- Image model: `nano-banana` (Gemini image generation via APIYI)
- Dami confirmed it was working when tested

### Two Pipelines
| | Old (broken) | New (active) |
|---|---|---|
| Path | `~/sule-memory/ai-news-automation/` | `~/projects/ai-shorts-pipeline/` |
| TTS | MiniMax (credits exhausted) | Edge TTS (free) |
| Script | MiniMax | Gemini via APIYI |
| Images | MiniMax | Gemini Imagen 3 via APIYI |
| Status | ❌ Broken since 2026-04-06 | ✅ Ready |

---

## What Was Done

1. **Wiki updated** — created `wiki/youtube-pipeline.md` with full documentation
2. **Wiki index updated** — youtube-pipeline status → Active
3. **Wiki log updated** — logged both wiki updates
4. **Cron wired** — old `daily-ai-news.js` replaced with new pipeline
5. **HEARTBEAT.md updated** — reflects new pipeline location

---

## Cron Change

**Before:**
```
0 7 * * * cd /root/sule-memory/ai-news-automation && node daily-ai-news.js >> logs/auto_$(date +\%Y-\%m-\%d).log 2>&1
```

**After:**
```
0 7 * * * cd /root/projects/ai-shorts-pipeline && /root/projects/ai-shorts-pipeline/.venv/bin/python cli.py run --topic "AI news today" --venv /root/projects/ai-shorts-pipeline/.venv >> /root/sule-memory/ai-news-automation/logs/pipeline_$(date +\%Y-\%m-\%d).log 2>&1
```

Logs to: `/root/sule-memory/ai-news-automation/logs/pipeline_YYYY-MM-DD.log`

---

## Next Steps

- Test the new pipeline end-to-end (dry run or actual 7 AM run tomorrow)
- Decommission old pipeline after new one proves stable