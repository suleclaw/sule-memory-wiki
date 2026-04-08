---
title: Wiki Cleanup Proposal
created: 2026-04-08
updated: 2026-04-08
tags: [meta, documentation, cleanup, proposal]
related: [[index]], [[SCHEMA]], [[getting-started]], [[log]]
---

# Wiki Cleanup Proposal

**Author:** Automated audit (claude-opus-4.6)
**Date:** 2026-04-08
**Scope:** Full review of all 21 pages in `wiki/` plus 3 root-level files (`SCHEMA.md`, `README.md`, `index.md`)

---

## 1. Broken Cross-References

These are pages referenced via `[[page-name]]` links in the index or other pages that **do not exist** as files in the wiki directory.

### 1.1 Referenced in `wiki/index.md` — pages that don't exist

| Referenced Link | Where Referenced | Problem |
|----------------|-----------------|---------|
| `[[printsbytee-mvp]]` | index.md line 23 (Projects table) | **File does not exist.** No `wiki/printsbytee-mvp.md`. The only PrintsbyTee page is `printsbytee-website-review-2026-04-08.md`. |
| `[[alpaca-api]]` | index.md line 43 (Technical table) | **File does not exist.** No `wiki/alpaca-api.md`. Referenced also in `trading-bot.md` and `trading-bot-strategy-decision-2026-04-08.md`. |
| `[[capitol-trades]]` | index.md line 44 (Technical table) | **File does not exist.** No `wiki/capitol-trades.md`. Referenced also in `trading-bot.md` and `trading-bot-strategy-decision-2026-04-08.md`. |
| `[[obsidian-setup]]` | index.md line 46 (Technical table) | **File does not exist.** No `wiki/obsidian-setup.md`. Also referenced in `llm-wiki-karpathy-system.md`. |
| `[[scaling-school]]` | index.md line 58 (Learning table) | **File does not exist.** No `wiki/scaling-school.md`. Also referenced in `getroleup.md`. |
| `[[memory-system]]` | index.md line 69 (AI & Knowledge table), also in `getting-started.md`, `llm-wiki-karpathy-system.md`, `session-greeting-fix.md` | **File does not exist.** No `wiki/memory-system.md`. |
| `[[SCHEMA]]` | index.md line 81 (Meta table), `getting-started.md`, root `index.md`, `README.md` | **Partially broken.** `SCHEMA.md` exists at repo root (`/root/sule-memory-wiki/SCHEMA.md`), not inside `wiki/`. The `[[SCHEMA]]` wiki-link won't resolve if the wiki reader expects files in `wiki/`. |

### 1.2 Referenced in individual pages — pages that don't exist

| Referenced Link | Where Referenced | Problem |
|----------------|-----------------|---------|
| `[[trading-bot-setup]]` | `trading-bot.md` frontmatter (related) | **File does not exist.** |
| `[[wheel-strategy]]` | `trading-bot.md` line 78, `trading-bot-strategy-decision-2026-04-08.md` lines 6, 69 | **File does not exist.** |
| `[[remocn]]` | `getroleup-youtube-ad.md` frontmatter (related) | **File does not exist.** Likely a typo for "remotion" or "remocn" packages mentioned in the page body. |
| `[[medsync-pro-booking-design]]` | `medsync-pro.md` frontmatter (related) | **File does not exist.** |
| `[[medsync-pro-spec]]` | `medsync-pro.md` frontmatter (related) | **File does not exist.** |
| `[[supabase-postgres-best-practices]]` | `medsync-pro.md` frontmatter (related) | **File does not exist.** |
| `[[printsbytee-mvp]]` | `suleclaw-agency.md` frontmatter (related), `printsbytee-website-review-2026-04-08.md` lines 6, 92 | **File does not exist.** (Same as index issue.) |
| `[[suleclaw-agency-strategy]]` | `suleclaw-agency.md` frontmatter (related) | **File does not exist.** |
| `[[ai-agent-automation]]` | `llm-wiki-karpathy-system.md` lines 6, 79 | **File does not exist.** |
| `[[personal-knowledge-base]]` | `llm-wiki-karpathy-system.md` lines 6, 80 | **File does not exist.** |
| `[[morning-briefing]]` | `q2-goals-2026.md` lines 6, 54 | **File does not exist.** |
| `[[linear-projects]]` | `q2-goals-2026.md` lines 6, 55 | **File does not exist.** |
| `[[Projects]]`, `[[Technical]]`, `[[Learning]]`, `[[AI & Knowledge]]` | `getting-started.md` lines 75-78, `README.md` lines 29-32 | **Files do not exist.** These are section headers in the index, not actual pages. |

### 1.3 Summary

**Total broken references: 19 unique missing pages** referenced across the wiki. The index alone lists 7 pages that don't exist. This means roughly one-third of the index's Technical and Learning sections point to phantom pages.

---

## 2. Duplicate and Overlapping Content

### 2.1 `youtube-pipeline.md` ↔ `session-ai-news-pipeline-update-2026-04-08.md`

**Overlap:** Both pages document the AI news pipeline transition from old (MiniMax) to new (Edge TTS + APIYI). They contain:
- The same two-pipeline comparison table (old vs new)
- The same cron job details
- The same APIYI + nano-banana findings
- The same next steps (test end-to-end, decommission old pipeline)

**Recommendation:** `session-ai-news-pipeline-update-2026-04-08.md` is a session log that should be **merged into** `youtube-pipeline.md`. The session page adds only the specific cron "before/after" change and the session timestamp. Move the cron change details into the youtube-pipeline page's "Cron Setup" section and delete or archive the session page.

### 2.2 `trading-bot.md` ↔ `trading-bot-strategy-decision-2026-04-08.md`

**Overlap:** Both pages contain:
- The same positions table (NVDA, MSFT, TSLA, AAPL, GOOGL with identical entry prices)
- The same "Capitol Trades blocked" information
- The same trailing stop + wheel strategy description

**Recommendation:** Keep both, but **deduplicate the positions table.** The strategy decision page should reference `trading-bot.md` for current positions rather than duplicating them. The decision page's value is the *reasoning* behind dropping copy trading — keep that, remove the duplicated position data.

### 2.3 `log.md` duplicate entry

**Lines 55-68 and 70-78** contain the **exact same log entry** ("ai-news-cron-wired | New pipeline wired to daily cron") with identical content. This is a copy-paste duplication.

**Recommendation:** Remove the duplicate entry (lines 70-78).

### 2.4 `log.md` also overlaps with `session-ai-news-pipeline-update-2026-04-08.md`

The log entry for `ai-news-pipeline-update` (lines 24-41) duplicates much of what's in the session page. This is expected (log is a summary), but the **API key** (`sk-PmxXN9R4jRoe719e1eAc37312d164785B32fDe27B31bD29a`) is hardcoded in `log.md` line 34 and `youtube-pipeline.md` line 52. Secrets should not be in the wiki.

### 2.5 `getting-started.md` ↔ `SCHEMA.md` ↔ `README.md`

All three files explain:
- The three-layer architecture (RAW / wiki / SCHEMA)
- Page format rules (frontmatter template)
- File naming conventions
- How to contribute

**Recommendation:** Each file should have a distinct purpose:
- `README.md` — external-facing intro (what is this, link to wiki). Keep short.
- `getting-started.md` — contributor/user guide (how to use, how to add pages).
- `SCHEMA.md` — AI maintenance rules (how Sule files things, lint rules). Not for human contributors.

Currently, all three repeat the page format template and the file naming rules. Consolidate: `getting-started.md` should be the single source of truth for "how to contribute," and the others should link to it.

---

## 3. Inconsistent Organization

### 3.1 Frontmatter inconsistencies

| Page | Issue |
|------|-------|
| `trading-bot.md` | **Missing closing `---`** for frontmatter. Line 7 starts the heading `# Trading Bot` without the closing `---` delimiter. The frontmatter is malformed — it will break any Jekyll/static site parser. |
| `agent-workflow-updates-2026-04-08.md` | **No frontmatter at all.** Starts directly with `# Agent Workflow Updates`. Every other page has YAML frontmatter. |
| `log.md` | **No frontmatter.** Starts with `# Log — Sule Memory Wiki`. |

### 3.2 Self-referencing related links

| Page | Issue |
|------|-------|
| `youtube-pipeline.md` | `related: [[youtube-pipeline]]` — references itself. |

### 3.3 Inconsistent title conventions in frontmatter

| Page | `title:` value | Heading (`#`) value | Issue |
|------|---------------|---------------------|-------|
| `mario-platformer.md` | `mario-platformer` | `# Mario Platformer` | Title uses filename slug instead of display name |
| `super-mushroom-powerup.md` | `Super Mushroom power-up` | `# Super Mushroom power-up` | Lowercase "power-up" (OK but inconsistent with other title casing) |
| `celebration-everywhere.md` | `Celebration everywhere` | `# Celebration everywhere` | Lowercase "everywhere" |
| `hidden-coins-meta-goal.md` | `Hidden coins meta-goal` | `# Hidden coins meta-goal` | Lowercase after first word |
| `unlockable-characters.md` | `Unlockable characters` | `# Unlockable characters` | Lowercase "characters" |

Most pages use Title Case (`Trading Bot Strategy Decision`, `GetRoleUp YouTube Ad Launch Video`), but the Mario sub-pages use sentence case. Pick one convention.

### 3.4 Inconsistent date format in log.md

- Lines 7, 24, 44, 55, 70: Use `## [2026-04-08] slug | Description` format
- Line 79: Uses `- **2026-04-08 16:30:** Description` (bullet format, different from section headers)
- Line 81: Uses `- **2026-04-08:** Description` (bullet format)
- Line 83: Uses `## 2026-04-08 (continued)` (no brackets, no slug)
- Line 89: Uses `- **2026-04-08:** Description` (bullet format)

The log format is specified in SCHEMA.md as `## [YYYY-MM-DD] ingest | Source description` but is not consistently followed.

### 3.5 Tag inconsistencies

Tags are not standardized across pages. Examples:
- `trading-bot.md` uses `capstone` tag — no other page uses this.
- `getroleup.md` uses `saas` but `medsync-pro.md` does not (despite also being a SaaS project).
- Mario sub-pages use `mario-platformer` as a tag (good), but the main `mario-platformer.md` uses `game` and `phaser` instead.
- `celebration-everywhere.md` uses `kids` and `polish` — these are one-off tags.
- `agent-workflow-updates-2026-04-08.md` has no tags at all (no frontmatter).

### 3.6 Session pages vs. decision pages — no naming convention

The wiki has session/decision log pages with inconsistent naming:
- `session-greeting-fix.md` — starts with `session-`
- `session-ai-news-pipeline-update-2026-04-08.md` — starts with `session-`, includes date
- `trading-bot-strategy-decision-2026-04-08.md` — uses `-decision-`, includes date
- `printsbytee-website-review-2026-04-08.md` — uses `-review-`, includes date
- `agent-workflow-updates-2026-04-08.md` — uses `-updates-`, includes date

There's no consistent prefix for ephemeral/session pages vs. permanent reference pages.

---

## 4. Pages That Should Be Created or Deleted

### 4.1 Pages that should be CREATED

These are the most critical missing pages — referenced multiple times and clearly represent real concepts/entities:

| Page to Create | Why | Referenced From |
|---------------|-----|----------------|
| `printsbytee-mvp.md` | Listed in index as a project page; referenced by 3 other pages. PrintsbyTee has no main project page — only a review page exists. | index.md, suleclaw-agency.md, printsbytee-website-review-2026-04-08.md |
| `alpaca-api.md` | Listed in index Technical section; referenced by trading-bot.md and strategy-decision page. Should document the Alpaca paper trading setup. | index.md, trading-bot.md, trading-bot-strategy-decision-2026-04-08.md |
| `capitol-trades.md` | Listed in index Technical section; referenced by trading-bot.md. Should document the data source and the cloud IP blocking issue. | index.md, trading-bot.md, trading-bot-strategy-decision-2026-04-08.md |
| `memory-system.md` | Listed in index AI & Knowledge section; referenced by 4 other pages. Should document Sule's current memory/context architecture. | index.md, getting-started.md, llm-wiki-karpathy-system.md, session-greeting-fix.md |
| `wheel-strategy.md` | Referenced by both trading-bot pages. The wheel strategy is implemented in code (`wheel_strategy.py`) and described in the decision page, but has no dedicated wiki page. | trading-bot.md, trading-bot-strategy-decision-2026-04-08.md |

**Lower priority creates:**

| Page to Create | Why |
|---------------|-----|
| `obsidian-setup.md` | Listed in index, referenced in llm-wiki-karpathy-system.md. |
| `scaling-school.md` | Listed in index, referenced in getroleup.md. Should document the 26 career track audit. |
| `morning-briefing.md` | Referenced in q2-goals-2026.md twice. Describes an active automation. |

### 4.2 Pages that should be DELETED or ARCHIVED

| Page | Recommendation | Reason |
|------|---------------|--------|
| `session-ai-news-pipeline-update-2026-04-08.md` | **Merge into `youtube-pipeline.md`, then delete.** | 95% overlap with youtube-pipeline.md. The only unique content is the cron before/after diff and the session timestamp. Move that into youtube-pipeline.md. |
| `session-greeting-fix.md` | **Keep but consider archiving.** | It documents a one-time fix to SOUL.md. The fix is already applied. Useful as historical record but low ongoing value. Could be collapsed into a "system fixes" page or appended to log.md. |

### 4.3 Pages that should NOT be deleted but need attention

| Page | Issue |
|------|-------|
| `agent-workflow-updates-2026-04-08.md` | Needs frontmatter added. Currently the only page without YAML frontmatter. |
| `log.md` | Needs frontmatter added. Needs duplicate entry removed. Needs consistent formatting. |

---

## 5. Security Issue

**API keys are hardcoded in wiki pages:**

| File | Line | Secret |
|------|------|--------|
| `youtube-pipeline.md` | 52 | APIYI key: `sk-PmxXN9R4...` |
| `log.md` | 34 | Same APIYI key |

These should be replaced with `${APIYI_API_KEY}` or a reference to where the key is stored (e.g., "stored in `~/projects/ai-shorts-pipeline/.env`"). **This wiki is deployed publicly on GitHub Pages.**

---

## 6. Proposed New Index Structure

The current index has 5 sections: Projects, Technical, Learning & Goals, AI & Knowledge Management, Meta. The proposed structure addresses the problems identified above.

### Design Principles

1. Only list pages that actually exist as files
2. Distinguish between reference pages (long-lived) and session/decision logs (ephemeral)
3. Group session/decision pages under their parent project
4. Add a "Missing Pages" section to track what needs to be created

### Proposed `wiki/index.md`

```markdown
---
title: Wiki Index
created: 2026-04-08
updated: 2026-04-08
tags: [meta, documentation, index]
related: [[getting-started]], [[log]]
---

# Wiki Index

All pages in the Sule Memory Wiki. Last updated: 2026-04-08.

---

## Projects

### Trading Bot
| Page | Summary | Status |
|------|---------|--------|
| [[trading-bot]] | Python bot — trailing stops + options wheel (Alpaca paper trading) | Active |
| [[trading-bot-strategy-decision-2026-04-08]] | Decision: drop copy trading, focus on wheel + trailing stops | Decision Log |

### GetRoleUp
| Page | Summary | Status |
|------|---------|--------|
| [[getroleup]] | Structured learning platform (26 career tracks) | Active |
| [[getroleup-youtube-ad]] | 55s Remotion launch ad — rendered + delivered | Done |

### Mario Platformer
| Page | Summary | Status |
|------|---------|--------|
| [[mario-platformer]] | Phaser 3 browser platformer for Dami's kids | Shipped |
| [[super-mushroom-powerup]] | Grow power-up with forgiving hitbox | Todo |
| [[celebration-everywhere]] | Particles, flashes, sounds — no scary game over | In Progress |
| [[unlockable-characters]] | 6 color variants unlocked via gameplay | Todo |
| [[hidden-coins-meta-goal]] | Secret coins → unlock Rainbow Mario | Todo |

### PrintsbyTee
| Page | Summary | Status |
|------|---------|--------|
| [[printsbytee-website-review-2026-04-08]] | Website review — 20 issues, photos + checkout are blockers | Review |

### Suleclaw Agency
| Page | Summary | Status |
|------|---------|--------|
| [[suleclaw-agency]] | Creative agency website (Industrial Noir design) | Shipped |

### MedSync Pro
| Page | Summary | Status |
|------|---------|--------|
| [[medsync-pro]] | Clinical appointment booking system — Phase 1 done | Active |

---

## Technical

| Page | Summary | Updated |
|------|---------|---------|
| [[youtube-pipeline]] | AI news shorts pipeline — Edge TTS, daily cron | 2026-04-08 |
| [[agent-workflow-updates-2026-04-08]] | code-review-js + software-architect agent configs | 2026-04-08 |

---

## Learning & Goals

| Page | Summary | Updated |
|------|---------|---------|
| [[q2-goals-2026]] | Q2 Goals: SaaS MVP, Azure AI-102, reading habit | 2026-04-08 |

---

## AI & Knowledge Management

| Page | Summary | Updated |
|------|---------|---------|
| [[llm-wiki-karpathy-system]] | Karpathy's personal wiki system for second brain | 2026-04-08 |
| [[session-greeting-fix]] | Fixed greeting to lead with project context, not global dump | 2026-04-08 |

---

## Meta

| Page | Summary | Updated |
|------|---------|---------|
| [[getting-started]] | How to use and contribute to this wiki | 2026-04-08 |
| [[log]] | Append-only activity log | 2026-04-08 |

---

## Pages Referenced but Not Yet Created

These pages are linked from existing wiki pages but don't exist yet.
Create them or remove the references.

| Missing Page | Referenced From | Priority |
|-------------|----------------|----------|
| `printsbytee-mvp` | index (old), suleclaw-agency, printsbytee-review | High |
| `alpaca-api` | trading-bot, strategy-decision | High |
| `capitol-trades` | trading-bot, strategy-decision | Medium |
| `memory-system` | getting-started, llm-wiki-karpathy, session-greeting-fix | High |
| `wheel-strategy` | trading-bot, strategy-decision | Medium |
| `obsidian-setup` | llm-wiki-karpathy | Low |
| `scaling-school` | getroleup | Low |
| `morning-briefing` | q2-goals-2026 | Low |

---

## Recent Activity

- **2026-04-08:** Agent workflow updates — code-review-js + software-architect
- **2026-04-08:** Wiki restructured — added getting-started, improved index
- **2026-04-08:** Trading bot strategy decision (drop copy trading)
- **2026-04-08:** AI news pipeline: new Python pipeline wired to cron
- **2026-04-07:** PrintsbyTee website review (20 issues documented)
- **2026-04-07:** Mario Platformer shipped

Full log → [[log]]
```

### Key Changes From Current Index

1. **Removed 7 phantom page listings** (printsbytee-mvp, alpaca-api, capitol-trades, obsidian-setup, scaling-school, memory-system, SCHEMA from wiki section)
2. **Added "Missing Pages" section** to make broken references explicit and trackable
3. **Grouped session/decision pages under their parent project** instead of as separate project rows
4. **Removed `session-ai-news-pipeline-update-2026-04-08.md`** from index (merged into youtube-pipeline)
5. **Added status column** to project pages for quick scanning
6. **Removed section-as-link references** (`[[Projects]]`, `[[Technical]]` etc. that aren't real pages)
7. **Moved SCHEMA reference out of wiki index** since it lives at repo root, not in `wiki/`

---

## 7. Action Items Summary

### Immediate Fixes (Can be done now)

| # | Action | Effort |
|---|--------|--------|
| 1 | Fix `trading-bot.md` frontmatter — add missing closing `---` | Trivial |
| 2 | Add frontmatter to `agent-workflow-updates-2026-04-08.md` | Trivial |
| 3 | Add frontmatter to `log.md` | Trivial |
| 4 | Remove duplicate log entry in `log.md` (lines 70-78) | Trivial |
| 5 | Remove hardcoded API key from `youtube-pipeline.md` line 52 and `log.md` line 34 | Trivial |
| 6 | Fix self-reference in `youtube-pipeline.md` frontmatter (`related: [[youtube-pipeline]]`) | Trivial |
| 7 | Fix typo `[[remocn]]` → remove or correct in `getroleup-youtube-ad.md` | Trivial |
| 8 | Standardize title case in Mario sub-page frontmatter | Low |

### Content Work (Requires writing)

| # | Action | Effort |
|---|--------|--------|
| 9 | Create `printsbytee-mvp.md` — main project page for PrintsbyTee | Medium |
| 10 | Create `alpaca-api.md` — Alpaca API documentation | Medium |
| 11 | Create `memory-system.md` — Sule's memory architecture | Medium |
| 12 | Create `capitol-trades.md` — data source documentation | Low |
| 13 | Create `wheel-strategy.md` — options wheel strategy reference | Low |
| 14 | Merge `session-ai-news-pipeline-update-2026-04-08.md` into `youtube-pipeline.md` | Low |
| 15 | Deduplicate positions table from `trading-bot-strategy-decision-2026-04-08.md` | Low |

### Structural Improvements

| # | Action | Effort |
|---|--------|--------|
| 16 | Replace `wiki/index.md` with proposed new structure (Section 6 above) | Medium |
| 17 | Consolidate page-format docs — `getting-started.md` as single source, others link to it | Low |
| 18 | Standardize log.md entry format to `## [YYYY-MM-DD] slug \| Description` | Low |
| 19 | Establish naming convention for ephemeral pages (e.g., `log/YYYY-MM-DD-slug.md` or `decision-` prefix) | Low |
| 20 | Remove phantom `[[Projects]]`, `[[Technical]]`, `[[Learning]]`, `[[AI & Knowledge]]` links from getting-started.md and README.md | Low |

---

## 8. Metrics

| Metric | Count |
|--------|-------|
| Total wiki pages (files) | 21 |
| Broken cross-references (unique missing pages) | 19 |
| Pages listed in index that don't exist | 7 |
| Pages with malformed/missing frontmatter | 3 |
| Duplicate content pairs | 3 |
| Hardcoded secrets | 2 occurrences (1 key) |
| Self-referencing related links | 1 |

---

*This proposal was generated by reading all 21 wiki pages, 3 root-level files, and cross-referencing every `[[wiki-link]]` against the actual file system.*
