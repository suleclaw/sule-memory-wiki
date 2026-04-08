---
title: Trading Bot Strategy Decision
created: 2026-04-08
updated: 2026-04-08
tags: [trading-bot, decision, trailing-stops, options-wheel]
related: [[trading-bot]], [[wheel-strategy]], [[alpaca-api]]
---

# Trading Bot Strategy Decision

**Date:** 2026-04-08
**Channel:** Discord DM with Dami

## Decision: Drop Copy Trading, Focus on Options Wheel + Trailing Stops

### What We're Keeping

**1. Trailing Stops (primary risk management)**
- 10% stop loss floor on all positions
- Trailing stop rises with stock price, never falls
- Executes automatically via Alpaca API
- Keeps losses small, locks in gains

**2. Options Wheel Strategy (income generation)**
- Sell cash-secured puts (CSP) to collect premium
- If assigned: sell covered calls against the shares
- If called away: sell another CSP — wheel keeps spinning
- Generates steady 1-3% per cycle regardless of direction
- Best in sideways/neutral markets

### What We're Dropping

**Copy Trading** — indefinitely parked
- Capitol Trades API is locked down (requires auth, blocked from most IPs)
- Simulated demo data has no real signal value
- Will revisit when: real data accessible, or alternative provider found (Quiver Quantitative, Benzinga)
- Dami will get notified when copy trading is reactivated

## Bot Operation

- Dami is NOT running bot locally anymore
- Sule (this AI) sends updates via:
  - Telegram (Dami's personal DMs)
  - Discord #trading-bot channel (1491401295960080455)
- Updates triggered on: position changes, stop losses fired, options expired/assigned, weekly summary

## Current Positions (Paper Trading)

| Symbol | Shares | Entry Price | Strategy |
|--------|--------|-------------|----------|
| NVDA | 27 | $183.46 | Trailing stop + wheel pending |
| MSFT | 13 | $384.55 | Trailing stop + wheel pending |
| TSLA | 8 | $364.03 | Trailing stop + wheel pending |
| AAPL | 15 | $259.33 | Trailing stop + wheel pending |
| GOOGL | 14 | $316.78 | Trailing stop + wheel pending |

All positions: 10% stop loss + trailing stop configured.

## Next Steps

1. Update bot README — remove copy trading references
2. Add options wheel automation to scheduler
3. Test wheel strategy on paper positions
4. Weekly position update to Discord + Telegram

## Status

- [[alpaca-api]] — ✅ connected (paper trading)
- [[wheel-strategy]] — to implement for options wheel
- [[capitol-trades]] — 🔒 parked until real data available
