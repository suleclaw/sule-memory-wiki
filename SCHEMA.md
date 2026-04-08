# SCHEMA — Sule Memory Wiki

This wiki is maintained by an AI (Sule) on behalf of Dami. The goal is a growing, interlinked knowledge base built from conversations, decisions, research, and projects.

## Three Layers

1. **RAW/** — source documents (articles, transcripts, notes, screenshots). Immutable. I add to this from our conversations.
2. **wiki/** — AI-generated markdown pages. summaries, entity pages, concept pages, comparisons. I write and maintain all of this.
3. **SCHEMA.md** — this file. Rules for how I maintain the wiki.

## Core Principles

- Knowledge is explicit — every page is readable, searchable, linked
- The wiki is a compounding artifact — it grows with every conversation and every good answer
- I never delete pages — only update, extend, or flag as superseded
- Cross-references between pages are as valuable as the pages themselves

## How I File Things

When a conversation triggers filing (any conversation is filed by default, or `#file` tag):
1. Extract key decisions, insights, concepts, and links
2. Write a new wiki page (or update an existing one)
3. Update `index.md` with the new page entry
4. Append to `log.md` with timestamp
5. Link to related existing pages
6. Flag any contradictions with existing knowledge

## Page Format

Each wiki page should have:
```markdown
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2, topic-area]
related: [[Page Name]], [[Another Page]]
---

# Page Title

Content...
```

## File Naming

- lowercase-with-hyphens.md
- Entity pages: `person-name.md`, `project-name.md`
- Concept pages: `concept-description.md`
- Topic pages: `topic-overview.md`
- Log entries: `log-YYYY-MM-DD.md` (for batch ingests)

## Ingest Sources

- Discord conversations (this is the primary channel)
- Videos (transcripts or summaries)
- Articles (via web clipper or manual save)
- Decisions from planning sessions
- Project updates and learnings

## Index.md

`wiki/index.md` is the master catalog. Every new page gets listed here with:
- Link to the page
- One-line summary
- Tags
- Date added

## Log Format

`wiki/log.md` — append-only chronological record:
```
## [YYYY-MM-DD] ingest | Source description
- Action taken
- Pages created/updated
- Key decisions
```

## Lint

Periodically I will:
- Check for broken links
- Find orphan pages (no inbound links)
- Flag contradictions between pages
- Identify gaps where important topics lack pages
- Suggest new pages based on conversation patterns

## Tools

- GitHub Pages (Jekyll) — wiki deployed at suleclaw.github.io/sule-memory-wiki
- Obsidian — local sync option for visual graph view
- Git — version history and branching

## This Is Living

This schema evolves. As we discover what works, I update this file.