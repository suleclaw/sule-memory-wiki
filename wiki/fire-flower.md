---
title: Fire Flower power-up
created: 2026-04-09
updated: 2026-04-09
tags: [mario-platformer, feature, power-up, fire]
related: [[mario-platformer]], [[super-mushroom-powerup]], [[1up-mushroom]]
---

# Fire Flower

Collect the Fire Flower → Mario turns orange/red and can shoot fireballs. Fireballs are purely for fun — no enemy kills, just the joy of shooting something across the screen.

## Spec

### Fire Flower sprite (procedural)
- Orange/red flower with petals
- Bob animation like Super Mushroom
- Size: 24×28

### Spawn
- Level 1: x=640, y=144 (on tall floating platform)
- Level 2: x=1000, y=144
- One per level, does not respawn

### Fire state
- `this.hasFire: boolean`
- Player sprite gets orange tint when hasFire
- Fire flower destroyed on collection

### Fireballs
- Press JUMP while moving → shoots fireball in facing direction
- Fireball: orange circle with particle trail, whoosh sound
- Destroyed on wall/screen edge
- Purely cosmetic — does NOT damage enemies or affect witch
- 300ms cooldown between shots

### Losing fire
- Getting hit while hasFire → lose fire (but keep big state if big)
- Fire does not respawn on the level

## Acceptance
- [ ] Fire flower sprite draws correctly
- [ ] Visible on each level with bob animation
- [ ] Collecting tints player orange
- [ ] Moving + jumping shoots fireball in facing direction
- [ ] Fireball has particle trail + whoosh sound
- [ ] Fireball destroyed on contact
- [ ] Getting hit loses fire
