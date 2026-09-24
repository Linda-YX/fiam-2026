# LambdaMART V2 — Text-Feature Ablation

## Question

Do 8-K-derived text features add incremental information to the frozen 147-characteristic LambdaMART V2 ranking architecture?

This experiment separates two questions:

1. **Signal question:** does text improve cross-sectional next-month return ranking?
2. **Portfolio question:** does text improve the realized risk/return profile after the ranking is converted into positions?

These are not treated as equivalent.

## Arms

- **Baseline:** LambdaMART V2 with 147 stock characteristics.
- **txt_v1:** baseline + first text-feature layer.
- **txt_v1 + txt_v2:** baseline + both text-feature layers.

The pre-specified validation approval rule is based on paired monthly Rank IC improvement, not portfolio Sharpe.

## Locked decision rule

For the full text arm to be validation-approved:

- paired mean ΔIC versus the 147-feature baseline must be positive, and
- paired ΔIC t-stat must be at least 2.

The 2021–2026 final test is not used to override this rule.

## Validation — pooled 2018–2020

| Arm | Rank IC | IC t-stat | Net 10 bp IR | Sharpe | Max DD | Beta |
|---|---:|---:|---:|---:|---:|---:|
| V2 147 | 0.0513 | 3.798 | 0.3986 | 0.6012 | -44.08% | -0.204 |
| + txt_v1 | 0.0440 | 3.430 | 0.4868 | 0.6752 | -35.79% | -0.031 |
| + txt_v1 + txt_v2 | 0.0453 | 3.575 | 0.6638 | 0.8640 | -26.54% | 0.025 |

Paired monthly IC evidence:

| Comparison | Mean ΔIC | t-stat | Positive Δ months |
|---|---:|---:|---:|
| txt_v1 vs baseline | -0.0073 | -1.29 | 36.1% |
| full text vs baseline | -0.0060 | -1.36 | 33.3% |
| txt_v2 incremental to txt_v1 | +0.0013 | 0.34 | 55.6% |

**Validation decision: not approved.** Text did not improve broad ranking accuracy under the locked rule, even though portfolio metrics improved.

## Frozen final test — pooled 2021–2026

These results are reported for confirmation/documentation only.

| Arm | Rank IC | IC t-stat | Net 10 bp IR | Sharpe | Max DD |
|---|---:|---:|---:|---:|---:|
| V2 147 | 0.0925 | 7.018 | 2.8791 | 3.0360 | -37.17% |
| + txt_v1 | 0.1082 | 8.248 | 3.2783 | 3.4439 | -21.04% |
| + txt_v1 + txt_v2 | 0.0973 | 7.601 | 3.3871 | 3.5642 | -17.77% |

Important paired evidence on the frozen test:

- txt_v1 vs baseline: ΔIC +0.0157, t = 3.47.
- full text vs baseline: ΔIC +0.0049, t = 1.08.
- txt_v2 incremental to txt_v1: ΔIC -0.0108, t = -2.95.

The favorable final-test portfolio metrics do not convert the failed validation rule into an adoption decision.

## Feature-importance snapshot

The most prominent text features include `txt_item_2_06`, `txt_n_amendments`, `txt_item_4_02`, filing-frequency variables, and `txt_days_since_last_filing`. Feature importance is predictive attribution, not causal evidence.

## Current interpretation

The key empirical pattern is:

> **Overall validation Rank IC fell, while net IR, Sharpe, and drawdown improved.**

Therefore the current evidence does not support the claim that text broadly improves return ranking. Two narrower mechanisms remain plausible and must be tested directly:

- **H1 — tail-ranking mechanism:** text improves identification of the stocks that actually enter the long/short tails while worsening or not improving the middle of the cross-section.
- **H2 — risk mechanism:** text identifies downside/idiosyncratic risk and changes portfolio composition in a way that improves the return distribution without improving full-universe return ranking.

Neither mechanism is established by the ablation alone.

## Next frozen diagnostic

Use 2018–2020 only for inference and do not retune LambdaMART.

1. Tail ranking quality: top/bottom 100 realized returns, tail precision, and relevant tail Rank IC.
2. Portfolio migration: names entering/exiting long and short books because of text.
3. Downside/idiosyncratic-risk test: bottom-5% outcomes, negative-tail magnitude, and residual risk if already available.
4. Incremental P&L attribution: long leg, short leg, extreme names/months.

After the diagnostic specification is frozen, apply the exact same analysis once to 2021–2026 as confirmation only.

## Decision

**Not validation-approved; retain as a documented research branch.**

The text layer is interesting because portfolio outcomes improve despite weaker broad validation Rank IC. The next step is mechanism diagnosis, not additional text-layer tuning.
