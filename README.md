# Portfolio Optimisation & Value-at-Risk (Financial Modelling, MSc FinTech and Policy)

Individual coursework project for MANM525 (Financial Modelling), University of Surrey. Builds a maximum-Sharpe-ratio portfolio across four single stocks, a currency-converted European equity index, a bond ETF and a risk-free cash leg, then quantifies its downside risk with a one-month, 95% Value-at-Risk under both a parametric Normal approach and a GARCH(1,1) conditional-volatility approach.

## What's in here

- `Financial_Modelling_Project.ipynb` -- the analysis, fully executed (all charts and results are baked into the notebook, so it renders on GitHub without needing to run it).
- Seven raw Bloomberg Terminal CSV exports the notebook loads, alongside it in this repo (GOOGL, AMZN, TSLA, GS, DAX Total Return, EUR/USD, BNDX).
- `requirements.txt` -- Python dependencies.

## Method

1. **Data**: weekly (Friday) closing prices from Bloomberg (`PX_LAST` / `TOT_RETURN_INDEX_GROSS_DVDS`) for six assets, 2020-2025; BNDX only available as one year of daily data, resampled to weekly.
2. **Returns**: the DAX leg is converted from EUR to USD returns before entering the model, so every series is on a common currency basis.
3. **Optimisation**: mean-variance (Markowitz) optimisation via `PyPortfolioOpt`, solving for the no-short-selling, maximum-Sharpe-ratio portfolio.
4. **Risk**: one-month (4-week) 95%/99% VaR via a Normal approximation and a GARCH(1,1) conditional-volatility model, cross-checked with a weekly VaR backtest against realised returns.

## Key results

| | |
|---|---|
| Optimal weights | DAX (USD) 66.3%, Goldman Sachs 11.8%, cash 11.1%, Tesla 10.8% |
| Expected annual return | 19.8% |
| Annual volatility | 17.4% |
| Sharpe ratio | 0.94 |
| 1-month 95% VaR (Normal / GARCH) | 6.25% / 6.17% |

## Limitations (see notebook, Section 9, for the full discussion)

The usable sample is capped at ~1 year (~51 weekly observations) because BNDX was only sourced as one year of daily data versus five years for the other six assets -- this limits both the precision of the mean-variance estimates and the GARCH(1,1) fit's ability to detect genuine volatility clustering. Extending BNDX to a matching five-year weekly history is the natural next step.

## Running it

```
pip install -r requirements.txt
jupyter notebook Financial_Modelling_Project.ipynb
```

