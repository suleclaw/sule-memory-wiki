---
title: Getting Started
created: 2026-04-08
updated: 2026-04-08
tags: [meta, documentation, getting-started]
related: [[index]], [[memory-system]]
---

# Getting Started — Sule Memory Wiki

Welcome. This wiki is a living knowledge base — part second brain, part project log, part decision archive.

## For Dami

You don't need to maintain this manually. Just talk to Sule and it happens automatically.

**To add something to the wiki:**
- Just have a conversation — filing is the default
- Use `#file` tag for explicit filing
- React with any emoji on Discord → auto-files

**To find something:**
- Search this page (Ctrl+F) for quick hits
- Browse [[index]] for all pages by category
- Ask Sule — she'll search and find it

## For Contributors

If you're editing the wiki directly:

### Page Format

```markdown
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2, topic-area]
related: [[Related Page]], [[Another Page]]
---

# Page Title

Content here...
```

### Rules

1. **Append-only** — never delete pages, only update or supersede
2. **Cross-link** — connect related pages with `[[Page Name]]` links
3. **Tag consistently** — use existing tags before creating new ones
4. **One-line summaries** — every page needs a clear one-liner in index.md

### File Naming

- `lowercase-with-hyphens.md`
- Entity pages: `person-name.md`, `project-name.md`
- Concept pages: `concept-description.md`
- Log entries: `log-YYYY-MM-DD.md`

## Architecture

```
RAW/           Source documents — immutable originals
wiki/          AI-generated pages — summaries, decisions, research
SCHEMA.md      Rules for maintenance
```

See [[SCHEMA]] for the full spec.

## Current Sections

| Section | What it covers |
|---------|---------------|
| Projects | trading-bot, printsbytee, suleclaw-agency, getroleup |
| Technical | alpaca-api, capitol-trades, youtube-pipeline |
| Learning | q2-goals-2026, scaling-school |
| AI & Knowledge | llm-wiki-karpathy-system, memory-system |

Browse all → [[index]]
