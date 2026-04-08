---
title: GetRoleUp YouTube Ad Launch Video
created: 2026-04-08
updated: 2026-04-08
tags: [getroleup, video, remotion, launch, youtube-ad]
related: [[getroleup]], [[remocn]]
---

# GetRoleUp YouTube Ad Launch Video

**Goal:** Replace the existing YouTube ad with a polished 55-second Remotion-based launch video.

---

## Overview

- **Duration:** 55 seconds (1650 frames @ 30fps)
- **Format:** MP4, H.264, 1920×1080
- **Repo:** `CyberDexa/GetRoleUP`
- **Local path:** `~/.openclaw/projects/GetRoleUP/remotion/`
- **Rendered output:** `remotion/out/06-launch-ad.mp4`
- **Status:** ✅ Rendered — delivered to Dami

---

## Script

### [0:00–0:05] HOOK
- Screen of browser tabs (tutorial hell) → all close at once → black screen → logo springs in
- Audio: Tab-close clicks, then silence
- Text: **"Still watching tutorials? Still can't build things?"**

### [0:05–0:15] PROBLEM
- Split-screen: chaotic tutorials left, clean roadmap right
- Text: "YouTube rabbit holes. Courses you'll never finish. Tutorials that teach you to follow along — but not to think."
- Sub-text: **"Tutorial hell is real. And it's keeping you stuck."**

### [0:15–0:30] SOLUTION
- GetRoleUp dashboard materializes, camera pans over 26 track cards
- Text: "GetRoleUp is different. 26 structured career tracks. No guesswork. No filler."
- Stats animate: **26 Tracks / 993+ Lessons / 399 Terminal Exercises**

### [0:30–0:50] FEATURES
- Three feature cards fly in (3-up layout):
  - 🖥️ **Terminal:** "Practice real commands in your browser — no setup required."
  - ▶️ **Watch:** "Curated YouTube videos from the best educators, organized by topic."
  - ✨ **AI Tutor:** "Stuck? Ask your AI tutor — it knows exactly where you are in the curriculum."

### [0:50–0:55] CTA
- **getroleup.com**
- **"Start your path today. It's free."**
- Sub-text: "Free & Open Source · 26 Career Tracks"

---

## Scene Breakdown

| Scene | Frames | Duration | Content |
|-------|--------|----------|---------|
| 1 — Tab Chaos | 0–150 | 5s | Hook — browser tabs chaos |
| 2 — Problem | 150–450 | 10s | Split-screen chaos vs roadmap |
| 3 — Solution | 450–900 | 15s | Dashboard + kinetic path + stats |
| 4 — Features | 900–1500 | 20s | 3 feature cards |
| 5 — CTA | 1500–1650 | 5s | Glass card + URL |

---

## Components Used

### Existing in GetRoleUP remotion project
- `StatBadge` — count-up animation
- `FeatureCard` — delay prop + spring entrance
- `SceneBg` — dark bg with noise + radial glow
- `GetRoleUpLogo` — animated logo
- `Outro` — CTA card

### Custom built (remocn-style)
- `BrowserTabsVisual` — animated closing tabs
- `SplitScreenLayout` — Scene 2 split-screen

### Custom transitions
- `Chromatic Aberration Wipe` (T1: S1→S2)
- `Frosted Glass Wipe` (T2: S2→S3)
- `Grid Pixelate` (T3: S3→S4)
- `Spatial Push` (T4: S4→S5)

---

## Files

| File | Lines | Purpose |
|------|-------|---------|
| `src/videos/06-launch-ad/LaunchAd.tsx` | 65 | Main composition (1650 frames) |
| `src/videos/06-launch-ad/scenes/LaunchAdScene1.tsx` | 200 | Tab chaos hook |
| `src/videos/06-launch-ad/scenes/LaunchAdScene2.tsx` | 109 | Problem — split-screen |
| `src/videos/06-launch-ad/scenes/LaunchAdScene3.tsx` | 132 | Solution + kinetic path + stats |
| `src/videos/06-launch-ad/scenes/LaunchAdScene4.tsx` | 147 | 3 feature cards |
| `src/videos/06-launch-ad/scenes/LaunchAdScene5.tsx` | 67 | CTA |

---

## Linear Issues

| ID | Title | Status |
|----|-------|--------|
| SUL-61 | Scene 3: Kinetic Path + Roadmap | Done |
| SUL-62 | Scene 4: Feature Cards | Done |
| SUL-63 | Scenes 1-2: Tab Chaos + Problem | Done |
| SUL-64 | Scene 5: CTA | Done |
| SUL-65 | Final Assembly + Render + Export | Done |

---

## Notes

- **Audio:** Not yet added — tab-close sounds + royalty-free music bed needed
- **Branch:** `feat/getroleup-youtube-ad` pushed to `origin`
- **Render:** `pnpm run render:launchad` from `remotion/` directory
- **remocn packages:** `@remocn/*` packages don't exist on npm — custom Remotion implementations were built instead
