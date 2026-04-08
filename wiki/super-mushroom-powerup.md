---
title: Super Mushroom power-up
created: 2026-04-08
updated: 2026-04-08
tags: [mario-platformer, feature, power-up]
related: [[mario-platformer]], [[celebration-everywhere]], [[unlockable-characters]], [[hidden-coins-meta-goal]]
---

# Super Mushroom power-up

When the player collects a Super Mushroom, they grow from small to big — larger sprite, bigger hitbox, and one free hit before shrinking back instead of dying.

## Spec

- **Mushroom sprite:** red with white spots, drawn procedurally
- **Spawn:** on level start, placed on a platform
- **Collection:** player touches mushroom → particle burst, sound, player grows
- **Big state:** hitbox is taller, enemy collision = shrink back to small, no death
- **Small state:** original behavior (can still stomp enemies)
- **Persistence:** big state resets on level restart

## Linear

- **Issue:** SUL-57
- **Team:** Sule Orchestrator
- **Project:** Mario Platformer
- **Status:** Todo

## Acceptance

- [ ] Mushroom visible on each level
- [ ] Player grows on collection with particle + sound
- [ ] Big player has taller hitbox
- [ ] Enemy hit in big state = shrink, no death
- [ ] Enemy hit in small state = original death behavior
