---
title: Morning Briefing
created: 2026-04-08
updated: 2026-04-08
tags: [meta, automation, discord, linear, daily]
related: [[q2-goals-2026]]
---

# Morning Briefing

Automated daily standup posted to Discord project channels every morning.

---

## How It Works

Every morning at **9:10 AM GMT**, the orchestrator runs `morning-briefing.js` which:

1. Reads `~/.openclaw/projects/q2-goals/AUTONOMOUS.md` — extracts open backlog items
2. Posts Q2 Goals task brief to **#q2-goals** Discord channel
3. Fetches Linear issue status for all active projects
4. Posts project updates to each Discord channel:
   - #algos-mastery
   - #medsync-pro
   - #tb-luxe-apparel
   - #printsbytee
   - #web-quote-calculator
   - #suleclaw-agency
5. Creates Linear issues for open backlog items if none exist
6. Marks briefed date in `~/.openclaw/workspace/briefed_YYYY-MM-DD.txt`

---

## Script

`node ~/.openclaw/workspace/morning-briefing.js`

---

## Rules

- Only briefs once per day (checks flag file `briefed_YYYY-MM-DD.txt`)
- If no open backlog items → posts "All clear today ✅" in #q2-goals
- Projects with no active (non-Done) issues → skipped in posting

---

## Channel IDs

| Channel | Discord ID |
|---------|------------|
| #q2-goals | 1490664200756924597 |
| #algos-mastery | 1487181768057552968 |
| #medsync-pro | 1488159963477184615 |
| #printsbytee | (in PrintsbyTee Discord) |
| #web-quote-calculator | (in Suleclaw Discord) |
| #suleclaw-agency | (in Suleclaw Discord) |

---

## Related

- [[q2-goals-2026]] — Q2 goals being briefed
- [[HEARTBEAT.md]] — orchestrator heartbeat tasks
