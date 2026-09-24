# McGill-FIAM Asset Management Hackathon 2026

Private team research repository for the 2026 McGill-FIAM Asset Management Hackathon.

## Research objective

Build a reproducible, market-neutral U.S. equity strategy using the supplied monthly stock-characteristics panel while enforcing strict no-look-ahead research practices.

The numerical research workflow is intentionally separated into two layers:

1. **Alpha model** — learns which stocks should rank above or below others.
2. **Portfolio construction** — converts model scores into positions while controlling beta, sector, liquidity, concentration, and turnover.

> **Research principle:** the model decides what it likes; portfolio construction decides how much risk to take.

## Repository map

- [Master experiment table](docs/EXPERIMENTS.md)
- [Research tracker](docs/RESEARCH.md)
- [Results snapshot](docs/RESULTS_SNAPSHOT.md)
- [Elastic Net baseline](experiments/elastic_net/README.md)
- [LightGBM](experiments/lightgbm/README.md)
- [LambdaMART](experiments/lambdamart/README.md)
- [LambdaMART text ablation](experiments/lambdamart_text/README.md)

## Current status

| Model / branch | Role | Current research conclusion |
|---|---|---|
| Elastic Net | Transparent linear baseline | Some cross-sectional signal, but unstable across research-test years |
| LightGBM | Nonlinear regression challenger | Strongly regime-dependent; nonlinear flexibility did not improve stability |
| LambdaMART V2 | Learning-to-rank + risk-controlled portfolio | Current lead numerical architecture; 2020 remains a clear regime failure |
| LambdaMART V2 + text | Incremental 8-K text branch | Portfolio metrics improved, but validation Rank IC did not; **not validation-approved** pending mechanism diagnostics |

## Data / leakage policy

- The supplied raw Parquet datasets are **not committed to GitHub**.
- Target: next-month excess return already supplied as `ret_exc_lead1m`; it is never used as a predictor and is never shifted again.
- Architecture research is restricted to pre-2021 target months.
- 2021–2026 competition OOS returns remain protected during model development and cannot be used to retune a frozen branch.
- All splits are defined by **target month**, not feature month.

## Competition constraints

Final strategy work will respect the challenge requirements, including:
- 100–500 total positions
- gross exposure ≤ 200%
- net exposure between −50% and +50%
- near-zero realized market beta
- explicit reporting of turnover, drawdown, alpha, information ratio, long/short legs, and risk exposures

## Notes

This repository is a research log, not a claim that every experiment is production-ready. Negative results and failed hypotheses are retained deliberately so the final process is auditable.
