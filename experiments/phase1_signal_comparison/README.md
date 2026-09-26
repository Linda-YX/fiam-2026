# Phase 1 — Nathan B1 vs LambdaMART V2

## Question

Under the same institutional portfolio engine, which frozen signal has stronger validation evidence: Nathan B1 or LambdaMART V2?

No signal blending, LambdaMART retraining, threshold search or 2021+ tuning was allowed.

## Design

Four fixed cells were compared:

| | Strict B1 PM | Pre-specified Relaxed B1 PM |
|---|---|---|
| Nathan B1 signal | A | B |
| LambdaMART V2 signal | **C** | D |

The relaxed specification used a $500M market-cap floor, retained the $5 price and $10M ADV floors, and removed the turnover cap while keeping beta, sector, name-cap, SI and cost controls. No alternative relaxed threshold was tested.

## Validation result, 2018–2020

| Cell | Signal / PM | Rank IC | Net Sharpe | Net IR | Net Max DD | Turnover |
|---|---|---:|---:|---:|---:|---:|
| A | Nathan / Strict | -0.018 | -1.260 | -1.794 | -26.6% | 10.3% |
| B | Nathan / Relaxed | -0.013 | -0.766 | -1.281 | -24.5% | 38.0% |
| **C** | **LambdaMART / Strict** | **0.008** | **0.929** | **0.248** | **-4.4%** | **10.3%** |
| D | LambdaMART / Relaxed | 0.009 | 0.208 | -0.377 | -9.9% | 71.5% |

## Decision

**Phase 2 signal fusion was not justified.** LambdaMART survived the strict institutional controls and provided the strongest validation evidence, while Nathan B1 was validation-negative. The next step was therefore to explain Cell C rather than blend the two signals.

See [Team Briefing](../../docs/TEAM_BRIEFING.md) and [Phase 1.5](../phase1_5_explain_c/README.md).
