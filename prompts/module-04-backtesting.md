# Module 4 Prompts — Backtesting & ML Training Loop

---

## Section 4.3 — Running Your First Backtest

Replace the EMA periods and logic with your own strategy description from Module 3. The structure of the prompt stays the same:

```
I want to backtest a trading strategy. Please write a Python script using yfinance to pull 24 months of daily OHLCV data for EUR/USD. The strategy is: buy when the 10-period EMA crosses above the 30-period EMA. Sell when the 10-period EMA crosses below the 30-period EMA. Use a fixed stop loss of 1% of position value. Use a fixed position size of 100% of capital per trade. Run on £10,000 starting capital.

Output: print a stats block with total return, Sharpe ratio, max drawdown, win rate, average win, average loss, expectancy in R, and total trade count. Also save an equity curve chart as equity_curve.png.
```

**What to record (your baseline):**
```
test_number, ema_fast, ema_slow, stop_pct, timeframe, sharpe, expectancy, max_dd, win_rate, trade_count
1, 10, 30, 1.0%, daily, [your result], [your result], [your result], [your result], [your result]
```

---

## Section 4.4 — The Iteration Loop

Make one change at a time. Examples from the course:

**Iteration: change EMA periods**
```
In backtest.py, change the fast EMA from 10 to 8 and the slow EMA from 30 to 21. Keep all other parameters the same. Run the backtest.
```

**Iteration: add RSI filter**
```
In backtest.py, add an RSI filter: only take long trades when the 14-period RSI is above 50 at the time of the EMA crossover. Keep all other parameters unchanged. Run the backtest.
```

**Rule:** If Sharpe drops, trade count falls below 50, or drawdown rises — revert immediately. One variable at a time.

---

## Section 4.5 — Building the Automated ML Training Loop (Parameter Sweep)

```
Build a parameter sweep system in Python. Using the backtest logic from backtest.py, run all combinations of these parameters: fast EMA periods [6, 8, 10, 12, 15], slow EMA periods [18, 21, 25, 30, 35], stop loss percentages [0.5, 0.75, 1.0, 1.25]. Skip combinations where the fast EMA period is greater than or equal to the slow EMA period.

For each combination, record the results in a SQLite database called backtest_results.db. The table should have columns: run_id, fast_ema, slow_ema, stop_pct, sharpe, expectancy, max_drawdown, win_rate, trade_count, total_return, run_timestamp.

After all combinations complete, print a ranked results table sorted by Sharpe ratio, highest first. Show the top 20 results.

Use yfinance for data, same instrument and date range as backtest.py. Print a progress indicator as it runs.
```

**Out-of-sample validation** — run this after the sweep to check for overfitting:

```
Modify sweep.py to use only the first 16 months of data for parameter optimisation (in-sample). Then take the top 10 parameter combinations from the in-sample results and run them on the remaining 8 months of data (out-of-sample). Report the in-sample Sharpe and the out-of-sample Sharpe for each of the top 10.
```

---

## Section 4.6 — The Automated Learning Loop

This builds the full self-improving research loop: paper trade → log results → retrain → update parameters.

```
Build a trading system learning loop in Python with four components:

1. Trade logger: a function that appends completed paper trades to the trade_log table in backtest_results.db. Accept these fields: trade_id, timestamp, instrument, direction, entry_price, exit_price, pnl_r, fast_ema, slow_ema, stop_pct.

2. Parameter store: functions to read and write the current active parameters, and to log every parameter update to the parameter_history table.

3. Retrain trigger: a scheduler using APScheduler that fires either when the trade_log reaches a multiple of 100 new trades since the last retrain, or every Sunday at midnight UTC — whichever comes first. When it fires, it calls sweep.py with the current trade_log as supplementary data.

4. Result comparator: after each sweep, compare the top out-of-sample Sharpe from the sweep against the current active Sharpe from parameter_history. If the new Sharpe is more than 5% higher, update the parameter store. Log every comparison to retrain_log regardless of outcome.

Wire all four components into a single script called learning_loop.py. It should start with the current active parameters from parameter_history and log to backtest_results.db.
```

**Query the results database** (run anytime):
```
Show me all retrain attempts this week and whether parameters were updated.
```
