# Numerical Results Snapshot

For the fastest team-facing view, use **[TEAM_BRIEFING.md](TEAM_BRIEFING.md)**.

## Current candidate

**LambdaMART Institutional Strategy V1**  
Frozen LambdaMART V2 signal + Strict B1 institutional portfolio construction.

### 2018–2020 validation

| Metric | Result |
|---|---:|
| Rank IC | 0.0078 |
| Gross annualized Sharpe | **1.097** |
| Net annualized Sharpe after modeled costs | **0.929** |
| Gross competition IR | **0.415** |
| Net competition IR | **0.248** |
| Net max drawdown | **-4.4%** |
| Average one-way turnover | **10.3%** |
| Realized CAPM beta | **0.015** |
| Average positions | **234.3** |
| Net compounded return | **17.2%** |

### Year-by-year validation

| Year | Gross Sharpe | Net Sharpe | Gross IR | Net IR | Net Max DD | Turnover | Realized Beta | Net Return |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| 2018 | 1.071 | 0.825 | 0.215 | -0.024 | -3.1% | 10.9% | 0.001 | 3.9% |
| 2019 | 0.453 | 0.262 | -0.396 | -0.587 | -2.8% | 10.0% | -0.034 | 1.1% |
| 2020 | 1.562 | 1.446 | 1.049 | 0.932 | -4.4% | 10.0% | 0.031 | 11.6% |

## Why this replaced the earlier LambdaMART portfolio as the main candidate

The LambdaMART signal itself was frozen. The improvement came from the portfolio architecture.

Phase 1.5 fixed-removal diagnostics:

| Removed component | Net Sharpe | Net DD | Turnover |
|---|---:|---:|---:|
| None — Cell C | **0.929** | **-4.4%** | **10.3%** |
| 1% position cap | -0.166 | -27.0% | 12.0% |
| Tradeability screen | 0.132 | -46.1% | 10.6% |
| SI short veto | 0.405 | -8.2% | 10.0% |
| 10% turnover cap | 0.506 | -5.8% | 70.0% |

Removing beta or sector neutrality increased sample Sharpe, but those constraints remain part of the strategy's systematic-risk discipline and are not treated as alpha generators.

## Interpretation boundary

The validation sample contains only 36 months. The short leg remains weaker than the long leg, 2019 is relatively weak, and average Rank IC is modest. 2021–2026 is post-hoc only and must not be used to retune the current candidate.
