# LightGBM — Nonlinear Regression Challenger

## Question

If linear relationships are too restrictive, do nonlinear interactions among the same 147 characteristics produce a more stable cross-sectional alpha signal?

## Research protocol

The data pipeline, feature allow-list, target alignment, protected 2021+ boundary, and outer research-test years are identical to the approved Elastic Net baseline.

LightGBM hyperparameters were selected on tuning-year regression MSE only. Untouched research-test years never influenced model selection.

## Pooled untouched results

- OOS (R^2): **0.000834**
- Mean monthly Rank IC: **0.02142**
- Rank IC positive-month rate: **69.44%**
- Gross spread Sharpe: **0.163**
- Net Sharpe at 5 / 10 / 20 bp: **0.138 / 0.113 / 0.063**
- Maximum drawdown: **−45.75%**

### Year-level spread Sharpe

| Test year | Sharpe |
|---|---:|
| 2018 | 4.48 |
| 2019 | −1.65 |
| 2020 | −0.77 |

## Model diagnostics

- 99/99 tuning and multi-seed trials completed successfully.
- All folds selected the same candidate.
- Locked boosting rounds were only **7 / 2 / 4**.
- No degenerate-prediction or unstable-seed flags were triggered.
- Search-boundary flags were triggered in all folds.
- Feature-gain concentration was high.
- Leading gain features included `lti_gr1a` and `rd5_at`.

The raw portfolio also displayed meaningful size, liquidity, and beta asymmetry between long and short books, so the high 2018 Sharpe should not be interpreted as pure stock-specific alpha.

## Interpretation

LightGBM found nonlinear signal, but the signal was not repeatable. The research hypothesis that nonlinear regression would improve stability over Elastic Net is **not supported** by these three untouched research-test years.

The 2018 result is preserved as a genuine success, and the 2019–2020 failures are preserved as genuine failures.

## Decision

**Freeze as a challenger / negative result.** Do not expand the search space after inspecting research-test outcomes.
