# LambdaMART Institutional Strategy V1 — Frozen Specification

## Status

**Current main research candidate.** This document records the architecture; it is not permission to retune it.

## Alpha engine

- Model: frozen LambdaMART V2
- Inputs: supplied 147-characteristic equity panel
- Objective: monthly cross-sectional learning-to-rank
- Target alignment: supplied next-month excess return; no second target shift
- No retraining or hyperparameter search was performed in Phase 1 / 1.5

## Strict institutional portfolio engine

### Eligibility
- absolute price >= $5
- market cap >= $2B
- 126-day dollar volume >= $10M
- beta_60m and betabab_1260d observed

### Portfolio constraints
- gross exposure: 200% NAV
- net exposure: 0
- beta_60m exposure: 0 at formation
- betabab_1260d exposure: 0 at formation
- sector net exposure <= 5% NAV
- sector gross exposure <= 70% NAV
- absolute name weight <= 1% NAV
- one-way turnover cap: 10% versus drifted prior holdings
- known FINRA SI/shares > 10%: ineligible for shorting
- unknown short interest: allowed
- deterministic infeasibility handling inherited from the audited B1 implementation

### Costs
Nathan B1 tiered assumptions are retained:
- >= $10B market cap: 5 bp one-way trading / 30 bp annual borrow
- $2B–$10B: 10 bp / 75 bp
- below $2B: 20 bp / 200 bp

## Research boundary

- Primary validation: 2018-01 through 2020-12
- 2021-01 through 2026-08: post-hoc only because outcomes have already been viewed
- No parameter should be selected from the post-hoc period

## Current interpretation

LambdaMART decides **which stocks rank higher or lower**. The portfolio engine decides **how much of that ranking signal is safe and economical to express**.

The Phase 1.5 ablations support four practical roles for the portfolio layer: concentration control, tradeability screening, short-side eligibility control, and turnover/cost discipline. Beta and sector neutrality are retained as mandate/risk controls, not because they maximize historical Sharpe.
