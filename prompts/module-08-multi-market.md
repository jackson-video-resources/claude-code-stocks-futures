# Module 8 Prompts — Expansion & Advanced Moves

---

## Section 8.1 — Running Multiple Markets in Parallel

**Step 1:** Parameterise the instrument so the same script can run for any market:

```
The current trading system runs for a single instrument passed as a parameter. I want to extend it so that: (1) the instrument is accepted as a command-line argument rather than being hardcoded, (2) the trade log filename includes the instrument name so EUR/USD and NQ logs don't overwrite each other, (3) the PM2 process name also includes the instrument. Show me the changes needed.
```

**Step 2:** Add a second instrument to PM2:

```
Add a second PM2 process entry to the ecosystem config for NQ1! futures. Name it trend-nq. Everything else is identical to the EUR/USD process.
```

Reload PM2: `pm2 restart ecosystem.config.js --update-env`

---

## Section 8.2 — Adding a Second Strategy (Mean Reversion)

```
Build a mean reversion strategy for NQ1! futures. Entry conditions: buy when RSI on the 15-minute chart drops below 30 AND price is more than 1 ATR below the 20-period EMA. Exit conditions: close the long when RSI crosses back above 50, or stop loss at 1.5 ATR below entry. Short-side mirror: sell when RSI is above 70 AND price is more than 1 ATR above the 20 EMA, exit when RSI crosses below 50. Run it using the same market data connection and execution framework we've already built.
```

Add to PM2:

```
Add a third PM2 process entry for mean-rev-nq.py. Name it mean-rev-nq. Keep it in the same format as the other entries.
```

**If mean-rev-nq shows an error at startup:** Check `pm2 logs mean-rev-nq --lines 20`. Most common causes: symbol format (`NQ1!` vs `NQ/USD`) or missing RSI library import.

---

## What's running after Module 8

Three PM2 processes:
- `trend-eur-usd` — EMA crossover trend strategy, EUR/USD
- `trend-nq` — same strategy, NQ futures
- `mean-rev-nq` — RSI + ATR mean reversion, NQ futures

Two strategies with genuinely different edges: trend-following wins when markets are moving directionally; mean reversion wins when they're oscillating. Their bad periods don't overlap the same way.

---

## Going further (Section 8.3)

Ideas to extend the system — all achievable with Claude Code:

**Sentiment layer:** Add a news API or Apify scraper. Ask Claude Code to score headlines for your instrument. Use the sentiment score to adjust position size or filter signals.

**Multi-timeframe confirmation:** Run the signal engine on both a higher timeframe (1-hour trend direction) and a lower timeframe (5-minute entry). Only take entries when both timeframes agree.

**Portfolio-level risk management:** Add a rule: if total open exposure across all processes exceeds 5% of account value, pause new entries across all strategies until exposure reduces.

All of these are descriptions you give to Claude Code. The architecture is already in place.
