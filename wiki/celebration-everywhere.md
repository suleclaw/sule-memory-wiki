---
title: Celebration everywhere
created: 2026-04-08
updated: 2026-04-08
tags: [mario-platformer, feature, polish, kids]
related: [[mario-platformer]], [[super-mushroom-powerup]], [[unlockable-characters]], [[hidden-coins-meta-goal]]
---

# Celebration everywhere

Every positive action in the game should feel like a celebration for a 4-5 year old. Coins, jumps, level complete — all amplified with particles, flashes, and sounds. No scary game over ever.

## Spec

### Coins
- Sparkle particle burst at coin position on collection
- Score popup floats up (+100) and fades

### Super Mushroom collection
- Big starburst particle explosion
- Screen flash white briefly (100ms)
- Triumphant sound

### Enemy stomp
- Enemy explodes into particles
- Coin popup appears above enemy

### Level complete
- Confetti particle rain
- Flag lowers with fanfare

### Death/Respawn (gentle)
- No game over screen ever
- On death: quick fade, gentle respawn at level start
- If lives = 0, auto-restart level seamlessly

## Linear

- **Issue:** SUL-58
- **Team:** Sule Orchestrator
- **Project:** Mario Platformer
- **Status:** In Progress (SUL-58 — frontend-dev agent)

## Acceptance

- [ ] Coin collect = sparkle particles + sound
- [ ] Mushroom collect = starburst + screen flash
- [ ] Enemy stomp = particle explosion
- [ ] Level complete = confetti + fanfare
- [ ] No scary death moment (no game over screen)
