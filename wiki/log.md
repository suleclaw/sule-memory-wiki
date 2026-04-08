# Log — Sule Memory Wiki

Append-only record of wiki activity.

---

## [2026-04-08] restructure | Major wiki improvements

**Action taken:**
- Replaced flat README with structured landing page + quick-access sections
- Created `wiki/getting-started.md` — user guide for Dami and contributors
- Rewrote `wiki/index.md` — table-based layout, status indicators, section organization
- Created root `index.md` — Jekyll-compatible home page with recent pages
- Updated `SCHEMA.md` — simplified, added tag conventions

**Key improvements:**
- README now explains what the wiki is + quick navigation
- Getting-started page bridges the gap between "what is this" and "how do I use it"
- Index organized by section (Projects / Technical / Learning / Meta) with status
- Root index.md for GitHub Pages compatibility

**Tags:** #meta #documentation #restructure

## [2026-04-08] ai-news-pipeline-update | New Python pipeline documented

**Action taken:**
- Investigated APIYI + nano-banana (Dami's testing) — found it in `~/projects/ai-shorts-pipeline/config.yaml`
- Located new Python pipeline: `~/projects/ai-shorts-pipeline/` — fully built pipeline using APIYI for both Gemini script + Imagen 3 images
- Key difference from old: Edge TTS (free, no API key) instead of MiniMax TTS (credits exhausted)
- Created `wiki/youtube-pipeline.md` — full documentation of both pipelines, config, usage, voice options
- Updated `wiki/index.md` — youtube-pipeline status updated to "Active"

**Key findings:**
- APIYI key: `sk-PmxXN9R4jRoe719e1eAc37312d164785B32fDe27B31bD29a` (in config.yaml)
- Image model: `nano-banana` (Gemini's image generation via APIYI)
- Script model: Gemini via APIYI
- Old pipeline broken since 2026-04-06 — MiniMax TTS status_code 1008 (insufficient balance)
- New pipeline has NO external API costs — Edge TTS + Gemini free tier + Whisper local

**Status:** Old pipeline broken, new pipeline ready. Next: wire new pipeline to daily cron.

**Tags:** #ai-news #youtube #pipeline #apiyi #edge-tts

## [2026-04-08] suleclaw-agency-page | Created wiki page for agency site

**Action taken:**
- Created `wiki/suleclaw-agency.md` — full page documenting tech stack, design system, dev history, next sprint backlog
- Design system: Industrial Noir (Instrument Sans + JetBrains Mono + Syne, amber glow, noise grain, glassmorphism)
- Status: Deployed ✅, V1 shipped 2026-03-31
- Tech: Next.js 16, TypeScript, Tailwind CSS v4, @base-ui/react, Framer Motion, Lucide, Nodemailer
- Next sprint backlog captured: logo SVG, Aceternity UI revamp, WhatsApp button, footer rework, mobile menu, etc.

**Tags:** #agency #website #nextjs #industrial-noir

## [2026-04-08] ai-news-cron-wired | New pipeline wired to daily cron

**Action taken:**
- Updated crontab — replaced broken `daily-ai-news.js` with new Python pipeline
- New cron entry: `0 7 * * * cd /root/projects/ai-shorts-pipeline && .venv/bin/python cli.py run --topic "AI news today"`
- Logs to: `/root/sule-memory/ai-news-automation/logs/pipeline_YYYY-MM-DD.log`
- Updated HEARTBEAT.md to reflect new pipeline location

**Current full cron (AI news line changed):**
```
0 7 * * * cd /root/projects/ai-shorts-pipeline && .venv/bin/python cli.py run --topic "AI news today" >> /root/sule-memory/ai-news-automation/logs/pipeline_$(date +\%Y-\%m-\%d).log 2>&1
```

**Tags:** #ai-news #cron #automation

## [2026-04-08] ai-news-cron-wired | New pipeline wired to daily cron

**Action taken:**
- Updated crontab — replaced broken `daily-ai-news.js` with new Python pipeline
- New cron entry: `0 7 * * * cd /root/projects/ai-shorts-pipeline && .venv/bin/python cli.py run --topic "AI news today"`
- Logs to: `/root/sule-memory/ai-news-automation/logs/pipeline_YYYY-MM-DD.log`
- Updated HEARTBEAT.md to reflect new pipeline location

**Tags:** #ai-news #cron #automation
- **2026-04-08 16:30:** GetRoleUp wiki page created — structured learning platform (26 career tracks)

- **2026-04-08:** Created [[medsync-pro]] wiki page — Phase 1 done, Phase 2 (Resend emails) in backlog

## 2026-04-08 (continued)
- **PR #1 merged**: feat/wheel-automation → master ✅
  - scheduler.py: removed dead CopyTradingEngine, added wheel.spin() call
  - Wheel opened covered calls on all 5 positions (AAPL, GOOGL, MSFT, NVDA, TSLA)
  - Branch deleted after merge
  - SUL-52 created, SUL-51 + SUL-48 → Done
- **2026-04-08:** PrintsbyTee website review — 20 issues documented, product photos + no checkout are critical blockers
