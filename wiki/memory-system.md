---
title: Memory System
created: 2026-04-08
updated: 2026-04-12
tags: [meta, memory, system, openclaw, context]
related: [[llm-wiki-karpathy-system]], [[getting-started]], [[q2-goals-2026]]
---

# Memory System

How Sule maintains context and continuity across sessions. Overhauled 2026-04-12.

---

## Architecture (Three-Layer, Karpathy Pattern)

Based on [Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f): raw sources → compiled wiki → schema.

### Layer 1 — Raw Sources
Conversations, daily notes, session transcripts. Immutable input.

| Source | Location | Purpose |
|--------|----------|---------|
| Daily notes | `memory/YYYY-MM-DD.md` | Auto-loaded (today + yesterday) |
| Session transcripts | `memory/.dreams/session-corpus/` | Redacted conversation text |
| Topic archive | `memory/topic-archive/` | Superseded topic-specific notes |

### Layer 2 — Compiled Knowledge
LLM-maintained, structured, cross-referenced. Compounds over time.

| File | Location | Updated by | Purpose |
|------|----------|-----------|---------|
| MEMORY.md | workspace root | Manual + Dreaming auto-promotions | **Single source of truth** for durable facts |
| DREAMS.md | workspace root | Dreaming pipeline (3AM cron) | Human-readable dream diary for review |
| Wiki pages | `/root/sule-memory-wiki/wiki/` | Auto after conversations | Project pages, decisions, technical refs |
| Wiki index | `wiki/index.md` | Auto with timestamps | Catalog of all wiki pages |
| Wiki log | `wiki/log.md` | Auto, append-only | Chronological activity log |

### Layer 3 — Schema
Configuration that tells me how to work.

| File | Purpose |
|------|---------|
| AGENTS.md | Rules, delegation, workflow, sub-agent routing |
| SOUL.md | Who I am, personality, boundaries |
| USER.md | Who you are, preferences |
| IDENTITY.md | My name, emoji, vibe |

---

## Memory Promotion Pipeline

The **Dreaming** system (runs 3AM UTC):

```
Conversations → daily notes → .dreams/ short-term recall
        ↓ (Light phase: sort & stage)
        ↓ (REM phase: reflect on themes)
        ↓ (Deep phase: score & promote)
    MEMORY.md ← only the strongest facts land here
```

**Scoring signals:**
- Frequency (0.24) — how many times seen
- Relevance (0.30) — retrieval quality
- Query diversity (0.15) — distinct contexts
- Recency (0.15) — freshness decay
- Consolidation (0.10) — multi-day recurrence
- Conceptual richness (0.06) — concept tag density

**Thresholds:** score ≥ 0.8, seen ≥ 3 times, from ≥ 3 unique queries. Only then a fact gets promoted to MEMORY.md.

Review dream promotions in `DREAMS.md` periodically.

---

## Session Startup (Lean)

1. Read `SOUL.md` — who I am
2. Read `USER.md` — who you are
3. Read `MEMORY.md` — durable facts (single source of truth)
4. Read `memory/YYYY-MM-DD.md` (today + yesterday) — daily notes
5. Use `memory_search` for everything else

No more 7-file preload.

---

## Auto-Filing Rule

After every meaningful conversation:
1. **Daily notes** → write to `memory/YYYY-MM-DD.md`
2. **Wiki updates** → update relevant project/decision pages
3. **Wiki index** → update timestamps
4. **Commit + push** → never leave wiki changes uncommitted

---

## Key Rules

- **MEMORY.md** is the single source of truth for durable facts
- **Wiki** is the structured knowledge layer (project pages, decisions, technical refs)
- **Daily notes** are the raw chronological record
- **Credentials** never go in wiki — use `${ENV_VAR}` placeholders
- **Every project** gets one wiki page + one Linear project
- **Dreaming promotes** to MEMORY.md automatically — review in DREAMS.md

---

## What Changed (2026-04-12)

- **Created MEMORY.md** — was missing, dreaming had 2,638 candidates and 0 promotions
- **Slimmed `/root/sule-memory/`** — from 213MB to 172KB (removed 212MB old AI pipeline)
- **Consolidated context.md** — from 519 lines to 45 lines (supplementary only)
- **Archived topic files** — 28 oddly-named files moved to `memory/topic-archive/`
- **Updated startup** — from 7-file preload to lean 4-step sequence
- **Filed unique docs to wiki** — PrintsbyTee payment architecture, security audit

Old system: 7 files to read, 213MB duplicate store, no working promotion pipeline.
New system: 4 files to read, structured wiki, dreaming promotes automatically.

---

## See Also

- [[llm-wiki-karpathy-system]] — inspiration for this system
- [[getting-started]] — how to use and contribute to the wiki