# Agent Workflow Updates (2026-04-08)

## Summary
Updated `code-review-js` and created `software-architect` AGENTS.md with opencode-powered review and architecture workflows.

## code-review-js — opencode gpt-5.3-codex Integration

**File:** `~/.openclaw/workspace/.agents/code-review-js/AGENTS.md`

### Workflow
1. **Read PR/changes** — fetch diff, read all changed files
2. **Run validation** — `pnpm run lint` + `pnpm run typecheck` + `pnpm run build` (must all pass)
3. **Spawn opencode** — runs `opencode run --model github-copilot/gpt-5.3-codex` to do deep code review against checklist:
   - Logic errors
   - Security issues (OWASP Top 10)
   - Performance problems (N+1, missing indexes, bundle bloat)
   - TypeScript type safety
   - Error handling
   - Test coverage
   - File sizes (> 200 lines flagged)
4. **Synthesize + report** — agent reads opencode output, posts findings as PR comment (blocking/non-blocking/approved)

### Key rule
Validation MUST pass before spawning opencode. opencode does the deep review; agent synthesizes and reports.

---

## software-architect — New Agent Created

**File:** `~/.openclaw/workspace/.agents/software-architect/AGENTS.md`

### Role
Software architecture decisions — system design, data model, API contracts, scalability, tech stack choices.

### Workflow
1. **Understand the problem** — read task brief, current system state, identify core architectural question
2. **Spawn opencode** — runs `opencode run --model github-copilot/claude-opus-4.6` for deep architectural analysis across:
   - System design
   - Data model (entities, relationships, schemas)
   - API contracts (clean, consistent, versioned)
   - Scalability (users, data, traffic)
   - Maintainability
   - Trade-offs
   - Security (OWASP Top 10 implications)
3. **Synthesize findings** — present decision options with pros/cons + recommended approach
4. **File to wiki + Linear** — decisions committed and pushed to wiki immediately

### Key rule
Always spawn opencode with `opus-4.6` — deep reasoning is purpose-built for complex system design. Never make architectural decisions alone.

---

## Agent opencode Model Map

| Agent | opencode Model |
|-------|---------------|
| frontend-dev | `github-copilot/claude-sonnet-4.6` |
| backend-dev-node | `github-copilot/gpt-5.3-codex` |
| backend-dev-python | `github-copilot/gpt-5.3-codex` |
| backend-dev-go | `github-copilot/gpt-5.3-codex` |
| code-review-js | `github-copilot/gpt-5.3-codex` |
| software-architect | `github-copilot/claude-opus-4.6` |

---

## Commit
`feat(agents): code-review-js spawns opencode gpt-5.3-codex; software-architect spawns opencode opus-4.6` — ad8f840e
