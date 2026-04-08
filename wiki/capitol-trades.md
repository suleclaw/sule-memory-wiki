---
title: Capitol Trades
created: 2026-04-08
updated: 2026-04-08
tags: [trading, data-source, politician-trades, scraping]
related: [[trading-bot]]
---

# Capitol Trades

**Website:** [capitoltrades.com](https://www.capitoltrades.com)
**Purpose:** Source of US politician stock trade data — buys/sells by senators and representatives
**Status:** Blocked — cloud IP range detected

---

## Overview

Capitol Trades publishes the stock trades made by US politicians (senators, representatives). It's the primary data source for the trading bot's copy-trading feature — the bot watches for trades by politicians and mirrors them in a paper trading account.

---

## Current Problem

Capitol Trades is blocking requests from cloud hosting IPs. The trading bot's server (hosted in a cloud provider) gets detected and blocked when trying to scrape politician trade data.

**Impact:** Copy trading is paused. The bot still runs trailing stops and wheel strategy using its own logic — it's not dependent on Capitol Trades.

---

## Data Available (when accessible)

- Politician name + party + chamber (House/Senate)
- Stock ticker traded
- Transaction type (buy/sell)
- Transaction date
- Estimated value of trade
- Filing date

---

## Workaround Options

1. **Residential proxy** — route requests through residential IPs to avoid cloud detection
2. **Third-party data provider** — use an API that aggregates Capitol Trades data (e.g., QuiverQuant)
3. **Manual polling** — fetch data from a non-cloud machine and relay to the bot
4. **Drop copy trading** — focus on the wheel + trailing stop strategies which don't need politician data

Current decision: Copy trading is dropped. See [[trading-bot-strategy-decision-2026-04-08]].

---

## Status History

| Date | Event |
|------|-------|
| 2026-04-08 | Confirmed blocked — cloud IP detected |
| 2026-04-08 | Copy trading dropped from trading bot strategy |
