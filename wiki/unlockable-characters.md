---
title: Unlockable characters
created: 2026-04-08
updated: 2026-04-08
tags: [mario-platformer, feature, unlockable]
related: [[mario-platformer]], [[hidden-coins-meta-goal]], [[super-mushroom-powerup]]
---

# Unlockable characters

Players unlock new color variants of the same player sprite through gameplay. Character picker on the menu screen.

## Characters (6 total)

| Character | Color | Unlock Condition |
|-----------|-------|-----------------|
| Default Mario | Blue overalls | Always unlocked |
| Blue Mario | Blue overalls | Collect 10 coins total |
| Green Mario | Green outfit | Complete Level 1 |
| Yellow Mario | Yellow outfit | Collect 20 coins total |
| Purple Mario | Purple outfit | Complete Level 2 |
| Rainbow Mario | Rainbow shimmer | Find all hidden coins |

## Persistence

- `localStorage`: `{ unlockedCharacters: string[], selectedCharacter: string }`
- Unlocks persist across sessions

## Character picker

- "Choose Character" button on menu (visible once at least 1 extra is unlocked)
- Grid of unlocked character sprites
- Selected character highlighted with glow
- Tap to select, tap PLAY to confirm

## Tech

- `drawPlayerTexture(scene, key, colorOverride)` — accepts hex color for overalls
- `BootScene` generates all 6 character textures on load
- Menu reads/writes `selectedCharacter` from localStorage

## Linear

- **Issue:** SUL-59
- **Team:** Sule Orchestrator
- **Project:** Mario Platformer
- **Status:** Todo

## Acceptance

- [ ] Default character always available
- [ ] All 6 color variants draw correctly
- [ ] Character picker accessible from menu
- [ ] Selected character persists across sessions
- [ ] Unlock conditions trigger correctly
