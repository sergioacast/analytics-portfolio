# Sergio Castillo — Data Analytics Portfolio

Data analyst working in Python and SQL/PostgreSQL.

**Live site:** https://sergioacast.github.io/analytics-portfolio/

## Projects

### [Trend Backtest](projects/trend-backtest/)

**Question:** Does a simple EMA crossover trend rule on SPY, QQQ and GLD beat buy-and-hold after costs (2005–2025), and does an ML filter improve it?

**Tools:** Python, pandas, NumPy, yfinance, PostgreSQL, SQLAlchemy, scikit-learn, matplotlib

- On the untouched 2020–2025 test, the tuned EMA 75/100 rule returned **7.2%** a year on SPY vs **15.0%** for buy-and-hold (Sharpe 0.52 vs 0.78), with a slightly smaller max drawdown (-30.4% vs -33.7%).
- EMA 75/100 was chosen from 29 pairs tested on 2005–2019 only (in-sample Sharpe 0.79); walk-forward picked it in 8 of 10 years.
- ML filter: logistic regression and random forest both scored **63.5%** vs a 64.1% always-up baseline, so the filter was dropped.

[Project README](projects/trend-backtest/README.md) · [Research note](projects/trend-backtest/reports/research_note.md) · [Notebooks](projects/trend-backtest/notebooks/) · [Live page](https://sergioacast.github.io/analytics-portfolio/projects/trend-backtest/)

### [Late Deliveries (Olist)](projects/late-deliveries/)

**Question:** Which origin→destination routes and seller areas miss the promised delivery window most, and is lateness getting worse?

**Tools:** Python, pandas, PostgreSQL, SQLAlchemy, SQL (CTEs, FILTER, BOOL_OR), matplotlib

- Overall late rate: **8.11%** (7826 of 96470 late-eligible delivered orders); pandas and SQL agree.
- Monthly late rate peaked at **21.36%** in March 2018 (15.99% in February 2018, 14.31% in November 2017).
- In the Feb–Mar 2018 spike, **SP→CE** went from 9.43% late (normal) to **48.95%**.

[Project README](projects/late-deliveries/README.md) · [Notebook (.ipynb)](projects/late-deliveries/late_deliveries.ipynb) · [Live page](https://sergioacast.github.io/analytics-portfolio/projects/late-deliveries/)

### [NYC Airbnb 2019](projects/nyc-airbnb/)

**Question:** How are NYC Airbnb listing prices distributed, and how do they vary by borough, room type, reviews, availability and minimum nights?

**Tools:** Python, pandas, NumPy, matplotlib

- Prices range from $0 to $10,000; mean 152.720687 vs median 106.0.
- “Expensive typical” cell: **Manhattan Entire home**, median ~$191 on 13199 listings. Staten Island Entire’s high mean is a luxury-tail story (median ~$100).
- availability_365 == 0 for **17,533** listings (~36%), kept rather than silently dropped.

[Project README](projects/nyc-airbnb/README.md) · [Notebook (.ipynb)](projects/nyc-airbnb/nyc_airbnb.ipynb) · [Live page](https://sergioacast.github.io/analytics-portfolio/projects/nyc-airbnb/)

### [Superstore EDA](projects/superstore/)

**Question:** Where does Superstore lose profit, how much of it is driven by discounting, and what would a 20% discount cap change?

**Tools:** Python, pandas, matplotlib

- Discounts above 20% lose money: 21–30% bin −10369.2774, 31–50% −48447.7273, 51–80% −76559.0513 total profit.
- Worst cell: **Binders at 51–80% discount**, −38510.4964 profit on 613 lines. Tables lose −17725.4811 overall.
- A 20% discount cap (1393 lines affected) moves modelled profit from 286397.0217 to **505588.0159**.

[Project README](projects/superstore/README.md) · [Notebook (.ipynb)](projects/superstore/superstore_eda.ipynb) · [Live page](https://sergioacast.github.io/analytics-portfolio/projects/superstore/)

## Repository layout

```
index.html            home page (GitHub Pages)
style.css             shared stylesheet
projects/<slug>/      project page, README, rendered notebook(s), .ipynb, charts
```

Datasets are not included (see each project’s Kaggle link); `*.csv` is git-ignored.
