# Stock-Bond Correlation: When Do Bonds Stop Diversifying Equities?

*Akshay Rao and Reshma Thomas*

**Business question:** Which macroeconomic conditions push the stock-bond correlation positive, and what does that do to the risk of a 60/40 portfolio?

**Headline results** (U.S. monthly data, Jan 1985 - Dec 2025; regimes split at Jan 2000)
- Five macro variables explain **71.3%** of the variation in 36-month rolling stock-bond correlation (Adj. R², M5); inflation alone explains **38.2%** (M1)
- **Credit spread is the largest single add:** +0.161 Adj. R² (M3 to M4); industrial production adds only +0.017 and is insignificant in every model
- 60/40 volatility fell from **10.4%** (1985-1999, correlation +0.27) to **9.3%** (2000-2025, negative correlation): bonds hedged equities only in the negative-correlation regime
- Rolling 60/40 volatility compressed to roughly 6-8% post-2000, then rose back above 12% in 2022 when stocks and bonds fell together
- Risk-adjusted return is a different story: 60/40 Sharpe was **0.405** pre-2000 vs **0.210** post-2000, because equity returns were much stronger in 1985-1999. The diversification benefit shows up in volatility, not in Sharpe

## Approach
| Phase | What was done | Owner |
|---|---|---|
| 1. Data | S&P 500 (Yahoo Finance); 10Y yield, CPI, industrial production, credit spread (BAA-10Y), WTI oil, T-bill (FRED) | Reshma Thomas |
| 2. Regimes | 36-month rolling Spearman correlation; pre-2000 vs post-2000 | Reshma Thomas |
| 3. Portfolio | 60/40 construction, volatility, Sharpe ratios, rolling diversification benefit | Reshma Thomas |
| 4. Regression | Five nested OLS models, Newey-West HAC (35 lags), interpretation, visuals | Akshay Rao |

## Regression results
| Model | Variables added | Adj. R² | Gain |
|---|---|---|---|
| M1 | Inflation (CPI YoY) | 0.382 | - |
| M2 | + Real rate | 0.498 | +0.116 |
| M3 | + Industrial production | 0.515 | +0.017 |
| M4 | + Credit spread | 0.676 | +0.161 |
| M5 | + Oil (WTI YoY) | 0.713 | +0.037 |

All regressors are 36-month rolling averages, matched to the rolling-correlation dependent variable. Inflation is significant at 1% in all five models.

## What it means for allocation
1. A 60/40 portfolio's risk reduction depends on the inflation and monetary-policy regime; it is not a permanent property of holding bonds.
2. Watch inflation, real rates and credit spreads as indicators for when the bond hedge weakens.
3. The Sharpe comparison is dominated by the return environment of each period, so judge the hedge by volatility and drawdown behavior.

## Data and stack
FRED (`pandas_datareader`) and Yahoo Finance (`yfinance`). Python, pandas, statsmodels, scipy, matplotlib. Data is downloaded live; no local files needed.

## Run it
```
pip install -r requirements.txt
jupyter notebook "Stock-Bond Correlation_Final.ipynb"
```

## Limitations
- Bond returns are approximated from changes in the 10Y yield, not a total-return index
- Rolling-window regressors overlap heavily; Newey-West errors help but the effective sample is much smaller than the observation count
- Sharpe ratios use fixed risk-free assumptions by regime (about 5% pre-2000, 2% post-2000)
- Regimes are split at a single date (Jan 2000) rather than estimated
