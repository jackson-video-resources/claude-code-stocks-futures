# Module 5 Prompts — Paper Trading Execution

---

## Section 5.2 — Building the Execution Layer

```
Build an execution layer that:
- Listens for buy and sell signals from the strategy
- When a buy signal fires for NQ1!, place a market order to Alpaca
- Position size is calculated to risk exactly 1% of my account balance
- Use the stop loss distance to calculate position size — if I have a $10,000 account and a 47-point stop loss on NQ, each NQ contract is worth $20 per point, so how many contracts can I trade to risk exactly 1%, which is $100?
- Set take profit automatically at a 2:1 reward-to-risk ratio
- Return order confirmation or error message
```

**Adjust for your instrument:** Replace `NQ1!`, the contract value per point (`$20`), and the stop distance with your own values. The position sizing logic is the same regardless of instrument.

---

## What the execution layer does

The flow, step by step:

1. Signal fires from the strategy module
2. Execution layer checks account balance via Alpaca API
3. Calculates position size: `(account_balance × risk_pct) / (stop_distance × point_value)`
4. Places a bracket order: entry (market), stop loss, take profit
5. Returns order ID and confirmation

**Before going live** — verify the maths manually:

- Account: $10,000
- Risk per trade: 1% = $100
- Stop distance: 47 points
- Point value: $20
- Max contracts: $100 / (47 × $20) = $100 / $940 = 0.1 contracts → round down to 0 for standard NQ, or use MNQ (micro contracts, $2/point)

For retail account sizes, MNQ (Micro NASDAQ) is the practical choice. Adjust the point value accordingly.
