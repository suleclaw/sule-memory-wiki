---
title: The Witchie Curse
created: 2026-04-09
updated: 2026-04-09
tags: [mario-platformer, feature, witchie, curse]
related: [[mario-platformer]], [[super-mushroom-powerup]]
---

# The Witchie Curse

A 5-year-old designed this mechanic. She wanted: a scary witch who chases Mario, mutual fear, body-swap on touch, and a dark world. The Super Mushroom cures the curse.

## The Feature

### Witch Sprite (procedural)
- Black robe + pointy hat + glowing green eyes
- Floats slightly above ground (no walking animation)
- Giggle sound: high-pitched staccato procedural notes
- Draw with `drawWitchTexture(scene, key)`

### Witch Behavior
- Spawns on each level (one witch per level)
- Actively flies toward Mario (not patrol — hunts, AI-driven)
- Moves slower than Mario so she can be outrun
- Cannot be stomped — must always be avoided
- When she touches Mario → THE CURSE triggers

### The Curse (on first touch)
1. Screen flashes deep purple (150ms)
2. Mario sprite swaps to witch sprite (player now IS the witch)
3. Level turns dark:
   - Background: solid black
   - Platforms: very dark grey silhouettes
   - Coins/enemies: barely visible (alpha 0.3)
4. Faint green glow around the player ( Witchie Curse world marker)
5. Curse is permanent for this level only
6. Reach flag → level complete still works, curse persists to next level you play

### The Cure: Super Mushroom
- Collecting the mushroom while cursed → curse lifted
- Mario restored to normal sprite
- World returns to normal colors
- Triumphant fanfare plays
- Mushroom respawns on the level after curse is lifted
- This is an ADDITION to the existing Super Mushroom power-up behavior

## Tech Details

### New State (GameScene)
- `this.isCursed: boolean` — true while in dark world on this level
- `this.witchTouched: boolean` — prevents curse triggering twice

### Witch Movement (GameScene update loop)
- Witch has a velocity toward player: `this.physics.moveToObject(witch, player, witchSpeed)`
- `witchSpeed` = 80 (slower than player at 180)
- Witch bounces off world edges

### Dark World Rendering
- When `this.isCursed = true`:
  - Background: fill with solid black
  - All platform sprites: setAlpha(0.25)
  - All coin sprites: setAlpha(0.2)
  - All enemy sprites: setAlpha(0.2)
  - Player: set witch sprite + faint green glow tint
- When curse lifted: restore all alphas, restore player sprite

### Witch Sprite Drawing
```
# Hat: black pointy triangle
# Robe: black rectangle
# Eyes: glowing green circles (0x00ff00)
# Floating animation: gentle bob up/down
```

### Audio
- Witch giggle: 3-4 high staccato notes on spawn, then randomly every 2-3 seconds
- Curse trigger: low rumbling tone + "ooh" descending notes
- Cure (mushroom): existing powerUp() arpeggio

## Level Integration
- Level 1 witch spawns at x=400, y=160
- Level 2 witch spawns at x=600, y=140
- Witch is a physics sprite (not static group)

## Acceptance Criteria
- [ ] Witch sprite draws correctly — pointy hat, black robe, green glowing eyes
- [ ] Witch actively flies toward Mario on each level
- [ ] Witch cannot be stomped — always triggers curse on contact
- [ ] On curse: screen flashes purple, Mario becomes witch, world goes dark
- [ ] Dark world: platforms/enemies/coins barely visible (alpha < 0.3)
- [ ] Player has faint glow in dark world
- [ ] Curse persists through level completion on same level
- [ ] Super Mushroom cures curse: Mario restored, world back to normal, mushroom respawns
- [ ] Giggle sound plays when witch spawns
- [ ] All existing features (coins, enemies, regular Super Mushroom) still work
