---
title: Alpaca API
created: 2026-04-08
updated: 2026-04-08
tags: [trading, alpaca, api, broker, paper-trading]
related: [[trading-bot]], [[trading-bot-strategy-decision-2026-04-08]]
---

# Alpaca API

**Purpose:** US equity + options paper trading via Alpaca
**Status:** Paper trading active — real money NOT yet enabled
**Website:** [alpaca.markets](https://alpaca.markets)

---

## Overview

Alpaca is a commission-free broker with a free paper trading tier. The trading bot uses Alpaca for executing trailing stop orders and running the options wheel strategy. No real money is involved yet — paper trading only.

---

## Setup

1. Sign up at [alpaca.markets](https://alpaca.markets)
2. Enable paper trading mode
3. Get API key + secret from the dashboard
4. Add to trading bot env: `ALPACA_API_KEY`, `ALPACA_SECRET_KEY`

---

## API Details

| Field | Value |
|-------|-------|
| Base URL | `https://paper-api.alpaca.markets` |
| Data URL | `https://data.alpaca.markets` |
| Auth | API key + secret (HMAC) |
| Rate limit | 200 requests/minute (paper) |
| Account type | Paper trading |

---

## What the Bot Uses

- **Market orders** — for initial position entry
- **Trailing stop orders** — for exit management
- **Options** — for covered call wheel strategy (requires options-enabled account)
- **Account status** — balance, portfolio value, P&L

---

## Current Positions (Paper)

Managed by `trading-bot`. See [[trading-bot]] for live positions.

---

## Notes

- Paper trading = simulated money, no real execution
- Options trading requires separate options agreement on Alpaca
- Trading bot repo: https://github.com/suleclaw/trading-bot
