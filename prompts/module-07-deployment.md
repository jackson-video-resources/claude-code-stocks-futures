# Module 7 Prompts — Automation & Resilience

---

## Section 7.2 — PM2 Setup

PM2 is installed with: `npm install -g pm2`

Key commands (no Claude Code prompt needed — these are terminal commands):

```bash
# Start your trading system
pm2 start trading-system.py --name trading-system --interpreter python3

# Check status
pm2 status

# Save process list (survives reboots)
pm2 save

# Generate startup script
pm2 startup

# View live logs
pm2 logs trading-system

# Kill switch
pm2 stop trading-system

# Restart after code changes
pm2 restart trading-system
```

---

## Section 7.3 — Telegram Alerts

**Step 1:** Create a bot via `@BotFather` in Telegram. Send `/newbot`, follow the prompts, copy the token.

**Step 2:** Get your chat ID by messaging your bot "hello", then visiting:
`https://api.telegram.org/bot<your-token>/getUpdates`

Find the `"id"` field inside `"chat"`.

**Step 3:** Add to Claude Code:

```
Add Telegram notifications to my trading system. I have a bot token and chat ID.
Send a notification when:
- A trade is opened (include instrument, direction, size, entry price)
- A trade is closed (include P&L in dollars and pips)
- A stop loss is hit
- Any unhandled error occurs

Use the Telegram Bot API directly with fetch. No external libraries.
Store the token and chat ID in .env as TELEGRAM_BOT_TOKEN and TELEGRAM_CHAT_ID.
```

**Add a daily summary** (optional, run after the system is stable):

```
Add a daily summary notification: at market close (4pm ET), send a Telegram message with today's trade count, total P&L, and system status. Use a cron job inside the trading system.
```

---

## Section 7.4 — Error Handling & Reconnection Logic

```
Add error handling and automatic reconnection to the trading system:

1. If the Alpaca websocket connection drops, automatically attempt to reconnect every 30 seconds. Log each reconnection attempt.

2. If an order placement fails, retry once after 5 seconds. If it fails again, send a Telegram alert and skip the trade — do not retry indefinitely.

3. Wrap the main trading loop in a try/except. If an unhandled exception occurs, log the full stack trace, send a Telegram alert with the error message, and restart the loop after 60 seconds.

4. Log all events (signals, orders, errors, reconnections) to a file called trading-system.log with timestamps.
```
