---
title: Trading Bot
created: 2026-04-08
updated: 2026-04-08
tags: [project, trading, alpaca, python, capstone]
related: [[alpaca-api]], [[capitol-trades]]

---

# Trading Bot

## Overview

Python agent that copies US politician stock trades via Capitol Trades, executes via Alpaca paper trading.

**Repo:** https://github.com/suleclaw/trading-bot
**Discord:** #trading-bot (1491401295960080455)
**Linear:** f2b10b6c-33de-4d6d-90e1-12bc4d3ca206

## Stack

- Python + uv (package manager)
- Alpaca API (paper trading)
- Capitol Trades (politician trade data — blocked on cloud IPs)

## Current Status (2026-04-08)

**Done:**
- Repo created and pushed
- `trading_bot.py` — main entry point
- `wheel_strategy.py` — options wheel engine (CSP + covered calls)
- `alpaca_client.py` — Alpaca API wrapper
- `capitol_trades.py` — politician trade fetcher
- `scheduler.py` — continuous scheduler with wheel automation ✅ **RECENT**
- README, .env.example, pyproject.toml

**Positions (paper trading):**
- NVDA: 27 @ $183.46
- MSFT: 13 @ $384.55
- TSLA: 8 @ $364.03
- AAPL: 15 @ $259.33
- GOOGL: 14 @ $316.78
- All have 10% stop loss + trailing stop configured

**Wheel Positions (active covered calls):**
| Symbol | Strike | Premium | Expires |
|--------|--------|---------|---------|
| AAPL | $282.84 | $771.39 | 2026-04-29 |
| GOOGL | $348.19 | $949.62 | 2026-04-29 |
| MSFT | $421.17 | $1,148.66 | 2026-04-29 |
| NVDA | $201.76 | $550.27 | 2026-04-29 |
| TSLA | $391.90 | $1,068.83 | 2026-04-29 |

**Backlog:**
- SUL-52: Wheel automation in scheduler ✅ MERGED
- SUL-51: Local dev setup — DONE
- SUL-50: Real Capitol Trades API (blocked on cloud IPs — need local dev)
- SUL-48: Options wheel strategy — DONE

## Key Blocker

Capitol Trades API is blocked on Google Cloud IPs. For real politician trade data:
- Clone and run from home IP, OR
- Use alternative: Quiver Quantitative, Benzinga

## Setup (Dami's local machine)

```bash
git clone https://github.com/suleclaw/trading-bot.git
cd trading-bot
uv sync
cp .env.example .env
# Edit .env with Alpaca keys
uv run python trading_bot.py
```

## Related

- [[alpaca-api]]
- [[capitol-trades]]
- [[wheel-strategy]]
