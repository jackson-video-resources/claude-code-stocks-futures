# Module 6 Prompts — Going Live

Module 6 is less about typing prompts into Claude Code and more about configuration and decisions. The key actions are described below.

---

## Section 6.1 — Risk Management Rules

Tell Claude Code your risk parameters and ask it to enforce them in the system:

```
Add the following risk management rules to the trading system:

1. Position sizing: risk exactly 1.5% of current account balance per trade. Read the account balance from Alpaca before each trade.

2. Daily loss cap: if total realised losses since market open exceed 5% of opening account value, block all new entries for the rest of the day. Log "Daily loss cap reached. New trades blocked." when this triggers.

3. Kill switch: add a command-line flag --stop-trading that halts all new order placement immediately. Open positions are left to their stops.
```

Adjust the percentages to match your own risk tolerance.

---

## Section 6.2 — Switching to Live Credentials

No new Claude Code prompt needed. The change is in your `.env` file:

```bash
# Paper trading (Modules 1-5)
ALPACA_BASE_URL=https://paper-api.alpaca.markets

# Live trading (Module 6)
ALPACA_BASE_URL=https://api.alpaca.markets
```

Also update your API keys to your live account keys (different from paper keys — find them in the Alpaca dashboard under your live account).

**Checklist before switching:**
- [ ] Identity verification completed and approved in Alpaca
- [ ] Risk management rules in place (daily loss cap, kill switch)
- [ ] System tested on paper with at least 10 automated trades
- [ ] Starting with smallest possible position size
- [ ] You know how to use the kill switch

---

## Section 6.3 — First Live Trade

The system logic is identical to paper trading. The only change is the API endpoint and credentials. Watch the first few trades manually from the Alpaca live dashboard before leaving it unattended.
