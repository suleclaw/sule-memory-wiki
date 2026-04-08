---
title: Wheel Strategy
created: 2026-04-08
updated: 2026-04-08
tags: [trading, options, wheel-strategy, income]
related: [[trading-bot]], [[alpaca-api]]
---

# Options Wheel Strategy

**Type:** Passive income options strategy
**Implementation:** `wheel_strategy.py` in the trading bot

---

## Overview

The wheel strategy (also called the "theta gang" strategy) generates income by selling covered calls and cash-secured puts on stocks you own or are willing to own. It's the primary active strategy in the trading bot alongside trailing stops.

---

## How It Works

### Step 1: Sell a Cash-Secured Put (CSP)
- Sell a put option at a strike price you're comfortable owning the stock at
- Collect a premium upfront
- If assigned: you buy the stock at that price (at a discount due to the premium collected)
- If not assigned: you keep the premium and repeat

### Step 2: Sell Covered Calls (CC)
- Once assigned (you now own the stock), sell covered calls above your cost basis
- Collect premium while waiting for the stock to rise
- If called away: you sell the stock at the strike price, locking in a gain
- If not called: you keep the stock and collect more premium

### Step 3: Repeat
- After selling a covered call (stock called away), sell another CSP on the same or new stock
- The wheel keeps spinning — generates income on the way up and the way down

---

## Bot Implementation

The `wheel_strategy.py` module:
1. Identifies eligible stocks with sufficient premium
2. Sells CSPs on stocks you want to accumulate
3. Rolls or exercises covered calls
4. Spins the wheel across the portfolio

---

## Current Status

Active on all 5 positions: NVDA, MSFT, TSLA, AAPL, GOOGL. Covered calls opened on all positions as of 2026-04-08.
