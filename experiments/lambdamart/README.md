# LambdaMART — Cross-Sectional Learning-to-Rank

## Question

Our portfolio does not require an exact return forecast; it requires a useful ordering of stocks. Does directly learning cross-sectional rankings generate a more stable signal than minimizing return-prediction MSE?

## Economic hypothesis

> Because the trading decision is cross-sectional and depends on relative winners and losers, a model trained directly on stock rankings may be better aligned with the economic objective than a model trained to forecast exact return magnitudes.

## Ranking design

- One ranking query/group per month.
- Stocks from different months are never placed in the same ranking group.
- Labels are based on within-month next-month excess-return ordering.
- A pairwise learning-to-rank objective is used.
- Ranking quality, not portfolio Sharpe, drives tuning.
- The protected outer train → tune → locked refit → untouched research-test design is retained.

## V1 takeaway

LambdaMART produced the strongest pooled ranking evidence among the numerical models tested at that stage.

Approximate V1 pooled Rank IC: **0.0684**.

The raw portfolio, however, still carried material unintended beta, sector, size, and liquidity exposures. That motivated a separate V2 portfolio-construction study rather than a post-hoc retuning of the ranker.

## V2 portfolio research

The V2 work kept the ranking architecture and added a more realistic tradeability / risk-control layer.

### Raw vs risk-controlled summary

| Metric | V2 Raw | V2 Risk-Controlled |
|---|---:|---:|
| Pooled Rank IC | ~0.0630 | ~0.0513 |
| Pooled Sharpe | ~0.396 | **~0.601** |
| 5 bp net Sharpe | ~0.371 | **~0.572** |
| Mean net beta | ~−0.411 | **~−0.057** |
| Max sector exposure | ~51% | **~20%** |
| Max drawdown | ~−47.9% | **~−44.1%** |

Approximate competition headline IR for the risk-controlled version:
- Gross: **~0.46**
- 5 bp: **~0.43**
- 10 bp: **~0.40**
- 20 bp: **~0.34**

## Important interpretation

The V2 improvement comes mainly from **portfolio construction**, not from a better alpha forecast. Rank IC did not increase when the risk layer was introduced.

This supports the architecture:

> **The model decides what it likes. Mathematics decides how much risk to take.**

## Regime failure

2020 remains a clear failure even after risk controls. The short book contained names that subsequently rose strongly, so beta/sector/liquidity constraints could not rescue the underlying ranking error.

This is intentionally retained as evidence of a real regime problem rather than something to tune away after observing the outcome.

## Decision

**Current lead numerical architecture**, with one major unresolved issue: alpha stability across regimes, especially 2020.

Further research should focus on why the ranking relationship fails in some environments rather than simply adding more model complexity.
