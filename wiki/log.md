# Log — Sule Memory Wiki

Append-only record of wiki activity.

---

## [2026-04-08] ingest | Karpathy LLM Wiki Gist

**Source:** Video transcript from Dami (YouTube video: "Karpathy's Viral Tweet & Why It Matters") + GitHub gist https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f

**Action taken:**
- Created `wiki/llm-wiki-karpathy-system.md` — full summary of Karpathy's LLM wiki system
- Created `wiki/index.md` — master catalog
- Created `SCHEMA.md` — wiki maintenance rules
- Set up GitHub repo `suleclaw/sule-memory-wiki`
- Configured GitHub Pages deployment

**Key decisions:**
- Deploy wiki at `suleclaw.github.io/sule-memory-wiki`
- Memory trigger: all conversations filed by default, `#file` tag for explicit filing
- Three-layer structure: RAW/ | wiki/ | SCHEMA.md

**Tags:** #ai #knowledge-management #second-brain #karpathy

---

## [2026-04-08] decision | Drop copy trading — focus on trailing stops + options wheel

**Source:** DM with Dami — explicit decision to remove copy trading

**Action taken:**
- Created `wiki/trading-bot-strategy-decision-2026-04-08.md`
- Updated `wiki/index.md` — updated trading-bot description
- Plan: update trading-bot README, add wheel strategy to scheduler

**Key decisions:**
- Copy trading PARKED — Capitol Trades API inaccessible without auth
- Focus: trailing stops (risk management) + options wheel (income)
- Dami stops running bot locally — Sule sends Telegram + Discord updates
- Updates to #trading-bot Discord channel (1491401295960080455)

**Tags:** #trading-bot #strategy #wheel-strategy

---

## [2026-04-08] system | Wiki initialized

- GitHub repo created and cloned locally to `/root/sule-memory-wiki`
- GitHub Pages enabled (Settings → Pages → main branch)
- Jekyll config for GitHub Pages deployment (needs `_config.yml` creation)
- First wiki pages created

**Note:** Need to add `_config.yml` for Jekyll and enable GitHub Pages in repo settings