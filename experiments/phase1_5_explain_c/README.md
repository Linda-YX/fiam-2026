# Phase 1.5 — Explain Cell C

## Objective

Explain why the frozen **LambdaMART V2 + Strict B1 PM** portfolio works without trying to improve it.

The benchmark was reproduced to numerical tolerance before diagnostics. No model, score, threshold, blend weight or portfolio parameter was trained, tuned or searched.

## Fixed component removals

| Portfolio | Gross Sharpe | Net Sharpe | Net IR | Net Max DD | Turnover |
|---|---:|---:|---:|---:|---:|
| **Cell C** | **1.097** | **0.929** | **0.248** | **-4.4%** | **10.3%** |
| Remove tradeability universe | 0.265 | 0.132 | -0.042 | -46.1% | 10.6% |
| Remove 10% turnover cap | 1.076 | 0.506 | -0.203 | -5.8% | 70.0% |
| Remove dual beta neutrality | 1.691 | 1.532 | 0.902 | -2.9% | 10.1% |
| Remove sector constraints | 1.307 | 1.167 | 0.601 | -5.3% | 10.3% |
| Remove SI veto | 0.576 | 0.405 | -0.271 | -8.2% | 10.0% |
| Remove 1% position cap | -0.117 | -0.166 | -0.344 | -27.0% | 12.0% |

These are interacting counterfactuals, not causal estimates or candidate strategies.

## Turnover mechanism

Removing the turnover cap increased turnover from 10.3% to 70.0% and modeled monthly trading cost from 3.72 bp to 22.14 bp, while gross Sharpe changed only from 1.097 to 1.076.

The strict portfolio was also much more persistent:
- holdings Jaccard overlap: 85.8% vs 21.3%
- signed-weight autocorrelation: 0.900 vs 0.284
- average holding spell: 9.98 vs 1.51 months

Executed notional had stronger average score/rank conviction than suppressed notional, which is consistent with — but does not prove — implicit portfolio-level regularization.

## Interpretation

The strongest validation associations are:
1. 1% concentration cap
2. tradeability screening
3. short-interest veto
4. turnover / implementation discipline

Removing beta or sector neutrality raises sample Sharpe, but those constraints are retained because they control systematic exposures and define the intended market-neutral mandate.

## Decision

Cell C remains the current main candidate architecture. No threshold was changed after this analysis.
