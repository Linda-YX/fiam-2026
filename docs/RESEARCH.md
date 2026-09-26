# RESEARCH.md — FIAM Numerical Research Tracker

This document records the major model decisions and the research protocol. For the chronological story, see **[RESEARCH_JOURNEY.md](RESEARCH_JOURNEY.md)**.

## Research protocol

**Primary boundary.** Architecture-selection research uses target months through December 2020. 2021–2026 outcomes have already been viewed and are post-hoc only.

**Target alignment.** A characteristic row in month t uses the supplied ret_exc_lead1m as the realized next-month outcome. The target is never shifted again.

**Principle.** Alpha generation and portfolio construction are evaluated separately.

## Model evolution

| Model / branch | What we learned | Decision |
|---|---|---|
| Elastic Net | Linear information exists but is unstable | Baseline |
| LightGBM | Nonlinear regression did not solve stability | Frozen challenger |
| LambdaMART | Ranking objective better matches cross-sectional selection | Primary alpha architecture |
| LambdaMART + text | Better portfolio headline did not survive signal-level validation | Not adopted |
| Nathan / economic-factor research | Valuable institutional PM discipline; simple signal blend not supported | Borrow PM ideas, not signal |
| Phase 1 Cell C | Frozen LambdaMART survives Strict B1 institutional controls | Promote |
| Phase 1.5 | Concentration, tradeability, SI and turnover controls explain much of portfolio improvement | Freeze current candidate |

## Current candidate

**LambdaMART Institutional Strategy V1**

Frozen LambdaMART V2 score plus:
- price >= $5
- market cap >= $2B
- 126-day dollar volume >= $10M
- observed beta_60m and betabab_1260d
- 200% gross, dollar neutral
- dual-beta neutrality at formation
- sector net <= 5% NAV; sector gross <= 70% NAV
- 1% name cap
- 10% one-way turnover cap versus drifted holdings
- FINRA SI/shares >10% cannot be shorted when SI is known
- tiered trading and borrow cost assumptions

2018–2020 validation: gross Sharpe 1.097, net Sharpe 0.929, net competition IR 0.248, net max drawdown -4.4%, turnover 10.3%, realized beta 0.015.

See [STRATEGY_SPEC.md](STRATEGY_SPEC.md) for the frozen definition and [TEAM_BRIEFING.md](TEAM_BRIEFING.md) for yearly metrics.

## Research principles retained

1. A better backtest does not automatically mean a better signal.
2. A ranking score should not automatically be interpreted as sizing conviction.
3. Implementation constraints can improve portfolio quality without changing the alpha model.
4. Beta and sector controls are risk controls even when removing them raises sample Sharpe.
5. Short-side evidence deserves extra scrutiny.
6. Preserve negative results and do not optimize from already-viewed 2021–2026 outcomes.
