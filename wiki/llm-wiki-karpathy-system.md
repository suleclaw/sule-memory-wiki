---
title: LLM Wiki - Karpathy System
created: 2026-04-08
updated: 2026-04-08
tags: [ai, knowledge-management, second-brain, andre-karpathy, llm]
related: [[memory-system]], [[morning-briefing]]
---

# LLM Wiki - Karpathy System

## What It Is

A personal knowledge base where an LLM acts as librarian, writer, and maintainer. Instead of uploading documents to a RAG system and querying from scratch every time, the AI incrementally builds and maintains a structured wiki — interlinked markdown pages that represent everything you've read, asked, and learned.

**Source:** Andre Karpathy's viral GitHub gist (2026-04)
**URL:** https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f

## Why RAG Falls Short

Traditional RAG (Retrieval Augmented Generation): you upload files, ask a question, AI searches for relevant chunks, gives an answer. The AI rediscovers everything from scratch on every query. Nothing accumulates. Complex multi-document questions fail because the AI has to hunt and piece together fragments every time.

## The Three Layers

1. **Raw Sources** — your immutable collection of source documents. Articles, papers, images, transcripts. You dump everything here; the AI never modifies this layer.
2. **The Wiki** — AI-generated markdown files. Summaries, entity pages, concept pages, comparisons. The AI owns this layer entirely — creating pages, updating when new sources arrive, maintaining cross-references.
3. **The Schema** — a config file (e.g. CLAUDE.md or AGENTS.md) that tells the AI how to organize, what conventions to follow, and what workflows to run.

## Key Insight: Compounding Knowledge

When the AI gives a great answer, you file that answer back into the wiki as a new page. Your explorations compound just like ingested sources. The wiki grows based on:
- Every source you've added
- Every question you've asked
- Every answer the AI has given

Cross-references already exist. Contradictions are flagged. Synthesis already reflects everything.

## Two Special Files

- **`index.md`** — catalog of every page in the wiki, organized by category, with one-line summaries. The AI reads this first when answering queries — works well at ~100 sources/hundreds of pages without vector search.
- **`log.md`** — append-only timeline of ingests, queries, and lint passes. Parsable with `grep "^## \[" log.md | tail -5`.

## Tools Mentioned

- **Obsidian** — local IDE for browsing the wiki, graph view, backlinks
- **Obsidian Web Clipper** — browser extension to save articles as markdown
- **qmd** — local search engine for markdown (BM25 + vector, MCP server)
- **Marp** — markdown slide decks
- **Dataview** — Obsidian plugin for querying page frontmatter
- **Git** — version history and branching comes free

## Karpathy's Four Principles

1. Knowledge is explicit — you can see everything the AI knows
2. Local files, not apps — simple structure AI can navigate easily
3. Bring your own AI — swap local or cloud models without changing data
4. Files as opposed to apps — simpler structure = better AI navigation

## The Bigger Vision

Karpathy shared an "idea gist" rather than code. His reasoning: in the age of AI agents, sharing a well-written spec lets any agent build a customized version for their owner. Value shifts from the code to the idea and how well you articulate it. GitHub becomes a repo of specs, not just code.

## Connection to Vannevar Bush's Memex (1945)

Bush's Memex was a vision of a personal knowledge store with associative trails between documents. The problem Bush couldn't solve: who does the maintenance? LLMs solve that now.

## Our Implementation

This wiki is deployed at `suleclaw.github.io/sule-memory-wiki` via GitHub Pages (Jekyll).

**Memory trigger:** Any message from Dami is filed to the wiki. `#file` tag = immediate explicit filing.

**Structure:**
- `RAW/` — source documents (server-side, I maintain from our chats)
- `wiki/` — AI-generated interlinked pages
- `SCHEMA.md` — rules for how I maintain this wiki

## Related

- [[memory-system]]
- [[obsidian-setup]]