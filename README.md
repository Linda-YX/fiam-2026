# McGill-FIAM Asset Management Hackathon 2026

Private team research repository for the 2026 McGill-FIAM Asset Management Hackathon.

## Research objective

Build a reproducible, market-neutral U.S. equity strategy using the supplied monthly stock-characteristics panel while enforcing strict no-look-ahead research practices.

The workflow separates:

1. **Alpha generation** — which stocks should rank above or below others?
2. **Portfolio construction** — how much of that signal can be expressed after tradeability, concentration, short-risk, beta, sector, turnover and cost controls?

> **Current principle:** LambdaMART decides what the strategy likes; the institutional portfolio engine decides how much risk is safe and economical to take.

## Start here

- **[Team Briefing](docs/TEAM_BRIEFING.md)** — headline results and year-by-year metrics for the current candidate
- **[Research Journey](docs/RESEARCH_JOURNEY.md)** — chronological story of what we tried, what failed and why the strategy changed
- **[Current Strategy Specification](docs/STRATEGY_SPEC.md)** — frozen LambdaMART Institutional Strategy V1
- [Master Experiment Table](docs/EXPERIMENTS.md)
- [Research Tracker](docs/RESEARCH.md)
- [Results Snapshot](docs/RESULTS_SNAPSHOT.md)

## Current candidate

### LambdaMART Institutional Strategy V1

**Frozen LambdaMART V2 signal + Strict B1-style institutional portfolio construction**

2018–2020 validation:

| Metric | Result |
|---|---:|
| Gross Sharpe | **1.097** |
| Net Sharpe after modeled costs | **0.929** |
| Gross competition IR | **0.415** |
| Net competition IR | **0.248** |
| Net max drawdown | **-4.4%** |
| Average one-way turnover | **10.3%** |
| Realized CAPM beta | **0.015** |
| Average positions | **234** |

The alpha model was not retrained to obtain these results. Phase 1 and Phase 1.5 instead tested whether a stricter institutional portfolio layer could express the frozen LambdaMART signal more credibly.

## Research path

Elastic Net -> LightGBM -> LambdaMART V1 -> V2 risk controls -> 8-K text (not adopted) -> Nathan replication -> common-engine signal comparison -> Phase 1 -> Phase 1.5 -> LambdaMART Institutional Strategy V1

Negative and inconclusive branches remain in the repository deliberately.

## Experiment folders

- [Elastic Net](experiments/elastic_net/README.md)
- [LightGBM](experiments/lightgbm/README.md)
- [LambdaMART](experiments/lambdamart/README.md)
- [LambdaMART + 8-K text](experiments/lambdamart_text/README.md)
- [Phase 1: Nathan B1 vs LambdaMART](experiments/phase1_signal_comparison/README.md)
- [Phase 1.5: Explain Cell C](experiments/phase1_5_explain_c/README.md)

## Data / leakage policy

- Raw competition Parquet datasets are **not committed to GitHub**.
- Target: supplied next-month excess return ret_exc_lead1m; it is never used as a predictor and is never shifted again.
- Architecture research is restricted to pre-2021 target months.
- 2021–2026 outcomes have already been viewed and are treated as **post-hoc documentation only**, not as a tuning set.
- Splits are defined by target month.

## Competition / portfolio discipline

The current candidate uses 200% gross, dollar neutrality, dual-beta neutrality at formation, sector exposure controls, a 1% name cap, institutional price/size/liquidity eligibility, a 10% one-way turnover budget, a FINRA short-interest veto, and explicit trading/borrow cost assumptions.

See [STRATEGY_SPEC.md](docs/STRATEGY_SPEC.md) for the exact frozen definition.

## Repository philosophy

This repository is an audit trail, not a highlight reel. A higher historical Sharpe is not enough to adopt a branch. Signal evidence, portfolio evidence, implementation realism and research independence are recorded separately.
