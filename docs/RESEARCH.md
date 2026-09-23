# RESEARCH.md — FIAM Numerical Research Tracker

This document records the numerical model research performed before opening the protected 2021–2026 competition test period. The purpose is to preserve hypotheses, evaluation design, negative results, and reasons for model decisions.

## Shared research protocol

**Protected boundary.** Model-selection research uses only target months through December 2020. No 2021–2026 realized returns are loaded or inspected during architecture selection.

**Target alignment.** A characteristic row in month (t) uses the already-supplied `ret_exc_lead1m` as the realized outcome for month (t+1). The target is never shifted again.

**Features.** Exactly the 147 allow-listed characteristics are eligible for the numerical models. Targets, identifiers, future returns, labels, and auxiliary evaluation columns do not enter (X).

**Outer evaluation design.**

| Research test year | Training for tuning | Tuning year | Locked refit | Untouched research test |
|---|---|---|---|---|
| 2018 | 2015–2016 | 2017 | 2015–2017 | 2018 |
| 2019 | 2015–2017 | 2018 | 2015–2018 | 2019 |
| 2020 | 2015–2018 | 2019 | 2015–2019 | 2020 |

Research-test metrics never enter hyperparameter selection.

## Model tracker

| Model | Research question | Key untouched result | Decision | Main caveat |
|---|---|---|---|---|
| Elastic Net | Is a transparent linear characteristic-return map sufficient? | 2019 Rank IC 0.0845 / Sharpe 0.629; 2020 Rank IC −0.0027; 2018 refit degenerated to a constant model | Keep as transparent baseline | Signal is highly unstable across years |
| LightGBM | Do nonlinear interactions make the signal more stable? | Pooled Rank IC 0.0214; gross spread Sharpe 0.163; 2018 Sharpe 4.48, 2019 −1.65, 2020 −0.77 | Freeze as negative/challenger result | Strong regime dependence, boundary winners, concentrated feature gain |
| LambdaMART V1 | Does direct cross-sectional learning-to-rank better match the trading objective? | Pooled Rank IC ~0.0684; positive IC months ~77.8% | Promote to primary ranking candidate for further portfolio research | Raw portfolio still carried unintended beta/sector/liquidity exposures |
| LambdaMART V2 + risk controls | Can a stronger portfolio layer make the ranking signal more investable? | Risk-controlled pooled Sharpe ~0.601; gross competition IR ~0.46; beta ~−0.057; max sector exposure ~20% | Current lead architecture | 2020 remains a clear ranking/regime failure; controls do not solve weak alpha |

## Elastic Net

The strict nested rerun is the approved linear baseline.

- **2018:** tuning on 2017 selected a specification whose locked refit produced zero nonzero coefficients. Rank IC and portfolio metrics are therefore invalid for that year.
- **2019:** OOS (R^2) 0.00167, Rank IC 0.08450, research Sharpe 0.62896.
- **2020:** OOS (R^2) 0.00438, Rank IC −0.00270, research Sharpe 0.04548.

Interpretation: there is some linear cross-sectional information, but it is not stable enough to serve as a strong standalone alpha engine.

See [experiments/elastic_net/README.md](../experiments/elastic_net/README.md).

## LightGBM

LightGBM used the same outer research-test design and tuned only on tuning-year regression MSE.

Pooled untouched results:
- OOS (R^2): 0.000834
- Mean monthly Rank IC: 0.02142
- Positive Rank IC months: 69.44%
- Gross spread Sharpe: 0.163
- Net Sharpe at 5 / 10 / 20 bp: 0.138 / 0.113 / 0.063
- Maximum drawdown: −45.75%

Year-level Sharpe:
- 2018: 4.48
- 2019: −1.65
- 2020: −0.77

Important diagnostics:
- all folds selected the same candidate,
- locked boosting rounds were only 7 / 2 / 4,
- search-boundary flags fired,
- feature gain was heavily concentrated, particularly in `lti_gr1a` and `rd5_at`,
- the raw short book tended toward smaller, less liquid, higher-beta names.

Interpretation: nonlinear flexibility did not deliver stable improvement over the linear baseline.

See [experiments/lightgbm/README.md](../experiments/lightgbm/README.md).

## LambdaMART

The ranking experiment changed the prediction objective rather than merely changing model family.

Design:
- monthly query/group,
- within-month next-month return relevance labels,
- pairwise learning-to-rank,
- tuning based on ranking quality rather than portfolio Sharpe,
- same protected outer research-test framework.

The V1 model produced stronger pooled ranking evidence than the prior regression models. V2 then added a more realistic liquidity and risk-control layer.

Current V2 risk-controlled summary:
- pooled Rank IC: ~0.0513 after risk-controlled portfolio filtering
- pooled gross spread Sharpe: ~0.601
- approximate gross competition IR: ~0.46
- approximate 10 bp competition IR: ~0.40
- net beta mean: ~−0.057
- maximum sector exposure: ~20%
- maximum drawdown: ~−44%

The improvement from raw V2 to risk-controlled V2 came mainly from portfolio construction rather than a more accurate alpha model. 2020 remained negative, which is treated as a genuine regime/ranking failure rather than something to tune away after the fact.

See [experiments/lambdamart/README.md](../experiments/lambdamart/README.md).

## Research principles retained for the next stage

1. Do not select a model by repeatedly inspecting protected test years.
2. Keep **alpha generation** and **portfolio construction** conceptually separate.
3. Prefer repeatability and economic interpretability over a single spectacular year.
4. Treat strong short-side results cautiously when they depend on small, illiquid, or hard-to-borrow names.
5. Preserve failed experiments. Negative results are useful evidence about what does not generalize.
6. When comparing an external strategy with ours, separate **signal quality** from **portfolio-layer quality** using a common evaluation harness.

## Next planned comparison

A useful next diagnostic is a common-harness comparison between:
- our LambdaMART score, and
- Nathan's frozen large-cap 7-group / 18-factor composite score,

under matched portfolio construction assumptions. This should clarify whether performance differences come from the alpha signal or from the optimizer / tradeability layer.
