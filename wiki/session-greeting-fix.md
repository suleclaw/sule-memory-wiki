---
title: Session Greeting Fix — Project Context Rule
created: 2026-04-08
updated: 2026-04-08
tags: [meta, system, agents, fix]
related: [[memory-system]], [[getting-started]]
---

# Session Greeting Fix — Project Context Rule

## Problem

Across all Discord channels, Sule's greeting was dumping recent global activity (PrintsbyTee, Trading Bot, etc.) regardless of which project channel the user was in.

Example: User opens `#algos-mastery` → gets greeted with PrintsbyTee and Trading Bot news. Wrong project context.

## Root Cause

Session startup read `context.md` and regurgitated everything marked "recent" without filtering by the current channel/project.

## Fix Applied

Added to `SOUL.md`:

```
## Session Greeting Rule

When greeting, **lead with the current project/channel context**, not a dump of recent global activity. If asked "what's on my plate" or similar — give project-specific status for the active channel, not a global dump. Only reference other projects if explicitly asked.
```

## Files Changed

- `/root/.openclaw/workspace/SOUL.md` — added Session Greeting Rule section

## Result

Greetings now lead with the current channel's project context. Asked Dami to start a new conversation to reload.
