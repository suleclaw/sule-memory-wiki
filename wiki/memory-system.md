---
title: Memory System
created: 2026-04-08
updated: 2026-04-08
tags: [meta, memory, system, openclaw, context]
related: [[llm-wiki-karpathy-system]], [[getting-started]], [[q2-goals-2026]]
---

# Memory System

How Sule (the AI assistant) maintains context and continuity across sessions.

---

## Architecture

Three layers:

### 1. Wiki (`/root/sule-memory-wiki/`)
Deployed at [suleclaw.github.io/sule-memory-wiki](https://suleclaw.github.io/sule-memory-wiki). Long-term knowledge base. Pages are created and updated by agents after sessions. Wiki is the single source of truth for cross-session context.

### 2. Session Memory (`/root/sule-memory/`)
Active context for the current session and recent history:
- `context.md` — current session state, pending decisions, active credentials
- `projects.md` — all project metadata (repos, URLs, Linear, Discord)
- `integrations.md` — connected services and API keys
- `memory/YYYY-MM-DD.md` — daily session logs
- `MEMORY.md` — long-term curated memory (main session only)

### 3. OpenClaw Workspace (`/root/.openclaw/workspace/`)
System configuration, agent definitions, skills, and runtime state:
- `AGENTS.md` — orchestrator rules
- `SOUL.md` — assistant persona
- `USER.md` — human profile
- `HEARTBEAT.md` — scheduled background tasks
- `.agents/` — sub-agent AGENTS.md files

---

## Session Startup Sequence

On every new session, the assistant reads in order:
1. `SOUL.md` → persona
2. `USER.md` → human profile
3. `/root/sule-memory-wiki/wiki/index.md` → wiki changes since last session
4. `/root/sule-memory/context.md` → current context
5. `/root/sule-memory/projects.md` → all projects
6. `/root/sule-memory/integrations.md` → credentials
7. `memory/YYYY-MM-DD.md` (today + yesterday) → recent context

---

## Cross-Channel Transparency

When work happens in a project channel (Discord), the assistant posts updates to that channel so the team stays informed. Session logs are written to `/root/sule-memory/` so any channel can see what was discussed.

---

## Key Rules

- Wiki changes → commit + push immediately
- Session context → write to `/root/sule-memory/context.md` after every session
- Credentials → never in wiki, always in `integrations.md`
- Every project → one wiki page + one Linear project
- Daily notes → `memory/YYYY-MM-DD.md`

---

## See Also

- [[llm-wiki-karpathy-system]] — inspiration for this system
- [[getting-started]] — how to use and contribute to the wiki
