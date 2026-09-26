# Team Briefing — Current Strategy Results

**Current candidate:** Frozen LambdaMART V2 signal + Strict B1 institutional portfolio construction (Cell C)

**Primary research period:** 2018–2020 validation.  
**2021–2026:** already viewed; post-hoc documentation only and not used for tuning.

## Headline validation result

| Metric | 2018–2020 |
|---|---:|
| Gross annualized Sharpe | **1.097** |
| Net annualized Sharpe after modeled costs | **0.929** |
| Gross competition IR | **0.415** |
| Net competition IR | **0.248** |
| Net max drawdown | **-4.4%** |
| Average one-way turnover | **10.3%** |
| Realized CAPM beta | **0.015** |
| Rank IC | **0.0078** |
| Average long positions | **118.5** |
| Average short positions | **115.9** |
| Average total positions | **234.3** |
| Net compounded return | **17.2%** |

## Year-by-year validation metrics

These are the annual metrics available from the frozen Phase 1 comparison for the current candidate.

| Year | Gross Sharpe | Net Sharpe | Gross Competition IR | Net Competition IR | Net Max DD | Avg. One-Way Turnover | Realized Beta | Rank IC | Net Return |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 2018 | 1.071 | **0.825** | 0.215 | -0.024 | -3.1% | 10.9% | 0.001 | 0.0028 | 3.9% |
| 2019 | 0.453 | **0.262** | -0.396 | -0.587 | -2.8% | 10.0% | -0.034 | 0.0300 | 1.1% |
| 2020 | 1.562 | **1.446** | 1.049 | 0.932 | -4.4% | 10.0% | 0.031 | -0.0094 | 11.6% |

### How to read this table

- **Sharpe** measures total risk-adjusted return.
- **Competition IR** uses the competition benchmark convention; it can be negative even when Sharpe is positive if returns do not clear the benchmark hurdle.
- **Turnover** is one-way turnover as a fraction of gross exposure.
- **Net** metrics subtract the documented transaction-cost and borrow-cost model.
- 2020 is the strongest validation year, but net Sharpe is positive in all three years.

## Portfolio architecture

The current candidate uses the frozen LambdaMART V2 score with:

- price >= $5
- market cap >= $2B
- 126-day dollar volume >= $10M
- both beta measures observed
- 200% gross / dollar neutral
- exact formation neutrality to beta_60m and betabab_1260d
- sector net <= 5% NAV
- sector gross <= 70% NAV
- 1% absolute name cap
- 10% one-way turnover cap versus the drifted prior book
- no shorts where known SI/shares > 10%
- Nathan B1 tiered trading and borrow-cost assumptions

## Why the portfolio layer matters

Fixed one-at-a-time removals were used for explanation only, not parameter tuning.

| Diagnostic | Net Sharpe | Net Max DD | Turnover | Interpretation |
|---|---:|---:|---:|---|
| **Cell C** | **0.929** | **-4.4%** | **10.3%** | Frozen benchmark |
| Remove 1% position cap | -0.166 | -27.0% | 12.0% | Concentration control is critical |
| Remove tradeability screen | 0.132 | -46.1% | 10.6% | Institutional universe matters |
| Remove SI short veto | 0.405 | -8.2% | 10.0% | Short-side eligibility matters |
| Remove turnover cap | 0.506 | -5.8% | 70.0% | Extra trading adds little gross Sharpe but large cost |
| Remove beta neutrality | 1.532 | -2.9% | 10.1% | Higher sample Sharpe, but loses intended systematic-risk control |
| Remove sector constraints | 1.167 | -5.3% | 10.3% | Higher sample Sharpe, but permits large sector bets |

## One-sentence team takeaway

> **The current strategy keeps LambdaMART as the stock-ranking engine but uses institutional portfolio constraints to prevent concentration, difficult shorts and excessive rebalancing; the strongest 2018–2020 validation result is net Sharpe 0.93 with 10.3% turnover, -4.4% max drawdown and near-zero realized beta.**

## Important caveats

The 2018–2020 validation window contains only 36 months. The short leg remains weaker than the long leg, 2019 is relatively weak, and the modest average Rank IC means portfolio results should not be interpreted as proof of broad stock-level forecasting accuracy. No threshold was tuned from the Phase 1.5 ablations.
