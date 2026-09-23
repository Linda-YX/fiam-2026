# Elastic Net — Transparent Linear Baseline

## Question

Can a sparse, interpretable linear model extract stable cross-sectional next-month return information from the 147 supplied characteristics?

## Data and leakage controls

- Exactly 147 allow-listed characteristics entered (X).
- `ret_exc_lead1m` is the next-month outcome and never enters (X).
- Architecture research uses target months no later than December 2020.
- 2021–2026 realized returns remain protected.
- Monthly transformations are contemporaneous only.
- Fitted preprocessing parameters are estimated only from permitted training observations.

## Strict nested research-test protocol

| Test | Train for tuning | Tune | Locked refit | Test once |
|---|---|---|---|---|
| 2018 | 2015–2016 | 2017 | 2015–2017 | 2018 |
| 2019 | 2015–2017 | 2018 | 2015–2018 | 2019 |
| 2020 | 2015–2018 | 2019 | 2015–2019 | 2020 |

Hyperparameters were selected using tuning-year MSE only. Research-test Rank IC, Sharpe, returns, and portfolio diagnostics never affected selection.

## Results

| Tune → test | α | L1 ratio | Nonzero coefficients | Test OOS R² | Test Rank IC | Test Sharpe |
|---|---:|---:|---:|---:|---:|---:|
| 2017 → 2018 | 0.01 | 0.75 | 0 | −0.01109 | Invalid | Invalid |
| 2018 → 2019 | 0.01 | 0.05 | 46 | 0.00167 | 0.08450 | 0.62896 |
| 2019 → 2020 | 0.01 | 0.50 | 1 | 0.00438 | −0.00270 | 0.04548 |

The 2018 locked refit is a constant model; portfolio ranking metrics are intentionally not reported as valid evidence.

## Interpretation

The model shows that linear characteristic information exists in some periods, but it is not stable across regimes. The 2019 result is meaningful evidence of cross-sectional signal; 2020 largely loses ranking power.

This model is retained as the **transparent baseline**, not as the preferred final alpha engine.

## Decision

**APPROVED as baseline.** Do not tune it further based on 2018–2020 outcomes.
