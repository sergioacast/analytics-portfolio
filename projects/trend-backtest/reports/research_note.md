Question: Does a trend-following rule (EMA 75/100, long or cash) on SPY beat buy-and-hold after costs, and does an ML filter improve it out of sample?

Answer: No on both. On the untouched 2020–2025 test, the tuned rule returned 7.2% a year vs 15.0% for buy-and-hold, with a Sharpe of 0.52 vs 0.78. Neither ML model (logistic regression or random forest) beat the 64.1% accuracy of simply predicting "up" every week.

Key table: final test, 2020–2025, costs included.

CAGR	Sharpe	Max drawdown
SPY rule	7.2%	0.52	-30.4%
SPY buy-and-hold	15.0%	0.78	-33.7%
QQQ rule	19.6%	1.00	-28.6%
QQQ buy-and-hold	20.1%	0.85	-35.1%
GLD rule	15.1%	1.02	-24.8%
GLD buy-and-hold	18.6%	1.13	-22.0%

What the rule is good for: protection in slow bear markets. In 2008, it limited the worst drop to about 11% while buy-and-hold fell 55%. It fails in fast, V-shaped crashes like 2020, where it rode the market down about 33% and bought back in late.

Biggest risk that a result is fake: the rule beat buy-and-hold on a risk-adjusted basis only on QQQ, which is 1 of 3 tickers. We also tried 29 parameter pairs to find the setup, so that win could be luck.

Would I paper-trade it? No. It doesn't beat a simple buy-and-hold on SPY after costs. I'd rather develop a different strategy that can beat the benchmark.