# Module 3 Prompts — Strategy Sourcing

Module 3 is about describing your strategy precisely enough for Claude Code to build it. There are no fixed prompts here — the output of this module is a strategy specification that you write.

---

## How to describe your strategy to Claude Code

The more precise your description, the better the code. Use this template as a starting point:

```
I want to build a trading strategy with the following rules:

Instrument: [e.g. EUR/USD, NQ1!, SPY]
Timeframe: [e.g. 15-minute bars, daily bars]

Entry conditions (long):
- [Condition 1 — e.g. 20-period EMA crosses above 50-period EMA]
- [Condition 2 — optional additional filter]

Entry conditions (short):
- [Mirror of long conditions, or leave blank if long-only]

Exit conditions:
- Stop loss: [e.g. 1% of entry price, or 47 points below entry]
- Take profit: [e.g. 2:1 reward-to-risk ratio]
- [Any additional exit conditions]

Position sizing:
- Risk [X]% of account balance per trade

Any other rules:
- [e.g. only trade during London/NY session overlap]
- [e.g. no trades on Friday afternoon]
```

---

## The five strategy sources covered in this module

**Source 1 — Your existing strategy (Section 3.2)**
Translate a manual trading strategy you already use into the template above. The key is precision: replace "when it looks like it's going up" with specific indicator conditions.

**Source 2 — GitHub open-source strategies (Section 3.3)**
Search: `https://github.com/search?q=trading+strategy+backtest+python`

Find a strategy with a clear rules description, copy the entry/exit conditions, and use the template above to describe them to Claude Code.

**Source 3 — YouTube transcript mining (Section 3.4)**
Use Apify's YouTube Transcript Scraper to extract the transcript from a trading strategy video. Paste the relevant rules section to Claude Code and ask it to extract the entry/exit conditions in a structured format.

**Source 4 — Hedge fund pitch decks (Section 3.5)**
Search SEC EDGAR (`https://www.sec.gov/cgi-bin/browse-edgar`) for 13F filings or use Google: `site:sec.gov "trading strategy" filetype:pdf`. Find a strategy description, summarise the logic, and use the template.

**Source 5 — First principles (Section 3.6)**
Start from a market observation: "EUR/USD tends to trend during the London/NY overlap and consolidate overnight." Translate that observation into concrete entry/exit rules and use the template.

---

## Checkpoint

By the end of Module 3, you should have a strategy description that answers these questions precisely:

- What instrument?
- What timeframe?
- What exact conditions trigger entry?
- What closes the trade?
- How is position size calculated?

That description becomes the input to Module 4's backtest.
