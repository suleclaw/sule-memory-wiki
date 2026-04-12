---
title: 1-Up Mushroom
created: 2026-04-09
updated: 2026-04-09
tags: [mario-platformer, feature, power-up, 1up]
related: [[mario-platformer]], [[super-mushroom-powerup]], [[fire-flower]]
---

# 1-Up Mushroom

Collect the 1-Up Mushroom → instant extra life. Simple, always exciting for kids.

## Spec

### Sprite (procedural)
- Green mushroom cap with white spots (like Super Mushroom but green 0x43a047)
- Small heart or "1" detail on cap
- Size: 24×24

### Spawn
- Level 1: x=192, y=176 (early floating platform — easy to reach)
- Level 2: x=740, y=128
- One per level, does not respawn

### Collection
- Lives +1 immediately
- "1-UP!" popup floats up
- Blue screen flash
- Triumphant arpeggio sound
- Particle burst
- Does NOT give big state — only extra life

## Acceptance
- [ ] Green 1-Up mushroom sprite draws correctly
- [ ] Visible on each level with bob animation
- [ ] Collecting adds +1 life with "1-UP!" popup
- [ ] HUD lives counter updates immediately
- [ ] Does not grant big state
