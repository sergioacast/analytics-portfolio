# Trend-following backtest: SPY, QQQ, GLD (2005–2025)

Does a simple EMA crossover trend rule beat buy-and-hold after costs, and does an ML filter improve it?

**Answer:** No on both. On the untouched 2020–2025 test period, the tuned EMA 75/100 rule returned 7.2% a year on SPY vs 15.0% for buy-and-hold. It did cut the worst drawdown a little (-30.4% vs -33.7%). Neither ML model beat the always-up baseline, so the final rule has no filter.

## Results (test period 2020–2025, run once)

| Ticker | Rule CAGR | Rule Sharpe | Rule Max DD | B&H CAGR | B&H Sharpe | B&H Max DD |
|---|---|---|---|---|---|---|
| SPY | 7.2% | 0.52 | -30.4% | 15.0% | 0.78 | -33.7% |
| QQQ | 19.6% | 1.00 | -28.6% | 20.1% | 0.85 | -35.1% |
| GLD | 15.1% | 1.02 | -24.8% | 18.6% | 1.13 | -22.0% |

The full write-up is in `reports/research_note.md`.

## Method

- **Rule:** long when EMA(fast) > EMA(slow) on adjusted close, otherwise cash. The signal is shifted one day so every trade uses yesterday's information (no look-ahead).
- **Costs:** 0.05% per trade, included in every number.
- **Warm-up:** the first 200 trading days are skipped so the EMAs are valid.
- **Baseline (Phase 1):** EMA 50/200 over the full period: 7.9% CAGR, 0.61 Sharpe, -33.9% max drawdown. Buy-and-hold: 11.1%, 0.64, -55.2%.
- **Tuning (Phase 2):** tested a grid of 29 EMA pairs on 2005–2019 only. 75/100 was best in-sample (Sharpe 0.79). Walk-forward picked it in 8 of 10 years. It was locked before touching the test period.
- **ML filter (Phase 3):** 7 backward-looking features (RSI14, MACD histogram / price, 5-day return, 60-day return, 20-day volatility, distance from EMA200, volume ratio). Target: SPY up over the next 5 days. Trained on 2005–2015, validated on 2016–2019. Logistic regression and random forest both scored 63.5% vs a 64.1% always-up baseline, so the filter was dropped.
- **Test (2020–2025):** run once, after every choice above was locked.

## Project structure

```
01-trend_backtest/
├── notebooks/
│   ├── 01_load_data.ipynb    # download prices, load into Postgres
│   ├── 02_baseline.ipynb     # EMA 50/200 vs buy-and-hold, drawdown chart
│   ├── 03_tuning.ipynb       # EMA grid, heatmap, walk-forward
│   └── 04_ml_filter.ipynb    # features, logistic regression / random forest, final test
├── reports/
│   └── research_note.md
└── README.md
```

## How to rerun

### 1. Requirements
- Python 3.10+
- PostgreSQL running locally (built with PostgreSQL 18 on `127.0.0.1:5432`)

### 2. Set up the environment
```bash
cd 01-trend_backtest
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m ipykernel install --user --name trend_backtest
```

### 3. Create the database
```bash
createdb -h 127.0.0.1 -U postgres trading
```
Store your Postgres password in `~/.pgpass` (or edit the connection URL at the top of each notebook):
```
postgresql+psycopg2://postgres@127.0.0.1:5432/trading
```

### 4. Run the notebooks in order (kernel: `trend_backtest`)
1. `01_load_data` downloads daily prices for SPY, QQQ, and GLD (2005-01-03 to 2025-12-31) and creates the `prices` table (primary key: ticker + date). Expect 5,283 rows per ticker.
2. `02_baseline` runs the EMA 50/200 baseline and logs it to `backtest_runs`.
3. `03_tuning` runs the EMA grid on 2005–2019 plus the walk-forward.
4. `04_ml_filter` runs the ML filter test and the final 2020–2025 backtest.

## Limitations
- Testing 29 pairs invites overfitting (the multiple-testing problem). The walk-forward and the one-time test period reduce this but don't remove it.
- The costs are a flat estimate. Slippage, taxes, and cash yield aren't modeled.
- There are only three tickers, one rule family, and one test period.

## Next step
Test mean reversion: buy SPY when RSI(14) drops below 30, and sell when it recovers above 50.

## Data
The price data isn't committed. Running `01_load_data` downloads it again.