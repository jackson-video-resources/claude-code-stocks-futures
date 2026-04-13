# Module 2 Prompts — Market Data & Signal Generation

---

## Section 2.1 — Connecting to Live Market Data

```
Write a Python script that connects to Alpaca's real-time market data API using a websocket. Subscribe to live trades and quotes for NQ1! — that's the continuous front-month NASDAQ futures contract. When a new price update arrives, print the timestamp, the symbol, and the latest bid and ask price to the terminal. Use the APCA_API_KEY_ID and APCA_API_SECRET_KEY environment variables for authentication. Use the paper trading base URL.
```

**Stocks traders:** Replace `NQ1!` with `SPY` — the code is identical.

**If you see a stream error:** Say this to Claude Code:

```
The symbol NQ1! isn't being recognised by this stream. Which Alpaca data stream should I use for futures contracts, and can you rewrite the script to use it?
```

---

## Section 2.2 — Building the Signal Engine

```
Build a signal engine in Python that does the following: it receives the live price feed from our Alpaca websocket (NQ1! 15-minute bars), calculates the 20-period EMA and the 50-period EMA from the bar data, and fires a BUY SIGNAL when the 20 EMA crosses above the 50 EMA, and a SELL SIGNAL when the 20 EMA crosses below the 50 EMA. When a signal fires, print it to the terminal in this format: '[BUY SIGNAL] NQ 18432 — 20 EMA crossed above 50 EMA on 15m'. Use pandas-ta or ta-lib for the EMA calculations. Maintain a rolling window of at least 100 bars so the EMAs are statistically meaningful.
```

**Note:** The engine needs at least 50 completed bars before it can calculate the 50-period EMA. For testing without waiting for live data, use this:

```
Add a backtesting mode that runs the signal engine against the last 200 bars of historical data from Alpaca instead of live data, so I can verify the signal logic is working correctly.
```
