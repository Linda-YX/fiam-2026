# Numerical Results Snapshot

This page is a compact snapshot of the model results already reviewed. It is not a replacement for the experiment READMEs or raw run artifacts.

## Current research ladder

| Model | Objective | Pooled / representative ranking evidence | Portfolio evidence | Status |
|---|---|---:|---:|---|
| Elastic Net | Linear next-month return regression | 2019 Rank IC 0.0845; 2020 −0.0027 | 2019 Sharpe 0.629; 2020 0.045 | Transparent baseline |
| LightGBM | Nonlinear next-month return regression | Pooled Rank IC 0.0214 | Pooled gross Sharpe 0.163 | Frozen challenger / negative stability result |
| LambdaMART V1 | Pairwise cross-sectional ranking | Pooled Rank IC ~0.0684 | Raw Sharpe ~0.256 | Stronger ranking signal, weak raw risk profile |
| LambdaMART V2 | Same ranking architecture + stronger portfolio controls | Risk-controlled Rank IC ~0.0513 | Gross Sharpe ~0.601; competition IR ~0.46 gross | Current lead architecture |

## Important interpretation

The strongest improvement from LambdaMART V2 came from the **portfolio layer**, not from an increase in ranking accuracy. Beta and sector exposures were materially reduced while Sharpe improved.

However, 2020 remains a clear failure. Risk controls reduced unintended exposures but did not repair the underlying ranking error.

## Comparison discipline

Do not compare our 2018–2020 frozen LambdaMART research path directly with Nathan's 2021–2026 frozen composite path. There are zero overlapping months. A fair signal comparison requires a strictly locked LambdaMART OOS score path over the same period or another genuinely overlapping window.
