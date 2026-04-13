# Module 1 Prompts — Setup & Foundation

These are the Claude Code prompts used in Module 1. Use them in order as you follow the course.

---

## Section 1.3 — Setting Up the Project Structure

After creating your `~/trading-system/` directory and `CLAUDE.md`, start Claude Code and run this to orient it to the project:

```
What is this project and what are the file structure conventions?
```

---

## Section 1.4 — Connecting to Alpaca

Once your `.env` file is created with your Alpaca API keys:

```
Write a Python script called alpaca-test.py that reads ALPACA_API_KEY, ALPACA_SECRET_KEY, and ALPACA_BASE_URL from the .env file using python-dotenv, connects to the Alpaca paper trading API, and prints the account balance.
```

**What to expect:** Claude Code will create `alpaca-test.py`. Run it with `python3 alpaca-test.py`. If you see your paper account balance printed, Module 1 is complete.

**If you get an error:** Paste the error message back into Claude Code — it will diagnose and fix it. Common causes: spaces around `=` in `.env`, wrong `BASE_URL` (needs `paper-` prefix), or missing `alpaca-py` package.
