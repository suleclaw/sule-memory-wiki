---
title: Hidden coins meta-goal
created: 2026-04-08
updated: 2026-04-08
tags: [mario-platformer, feature, meta-goal, hidden-content]
related: [[mario-platformer]], [[unlockable-characters]]
---

# Hidden coins meta-goal

Each level has 1-2 secret coins tucked away in a tricky or hidden spot. Finding them all across all levels unlocks Rainbow Mario. The meta-goal gives replayability and a sense of discovery.

## Hidden coin locations

**Level 1:**
- Coin A: Above tall floating platform at x=640, y=80 (requires precise jump)
- Coin B: Behind a decorative hill

**Level 2:**
- Coin A: On the highest platform at x=480, y=80 (hardest jump in the level)
- Coin B: In a "cave" formed by two platforms at x=880, y=144 (requires going inside the gap)

## Hint system

- Subtle sparkle particle floats near hidden coin location (always visible, faint)
- NOT a bright arrow — just enough for a parent to point and say "look!"

## Progress tracking

- `localStorage`: `{ hiddenCoinsFound: { level1: number, level2: number } }`
- Total hidden coins: 4
- HUD shows: ★ x/4 found (shown on menu and in-game)

## Unlock

- All 4 found → Rainbow Mario unlocked (shown as unlock celebration on menu)

## Linear

- **Issue:** SUL-60
- **Team:** Sule Orchestrator
- **Project:** Mario Platformer
- **Status:** In Progress (SUL-60 — frontend-dev agent)

## Acceptance

- [ ] Each level has exactly 2 hidden coins
- [ ] Hint sparkle visible near each hidden coin
- [ ] Collecting hidden coin increments counter in localStorage
- [ ] HUD shows x/4 stars
- [ ] Collecting all 4 triggers Rainbow Mario unlock
- [ ] Counter persists across sessions
