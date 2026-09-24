# EXPERIMENTS.md — Master Research Tracking Table

This is the top-level index for the FIAM numerical research program. Each row points to the experiment README that contains the methodology, frozen results, and caveats. Negative results are retained deliberately.

## Shared research frame

- **Target:** next-month excess return, using the supplied `ret_exc_lead1m` without shifting it again.
- **Architecture-selection window:** target months through 2020-12 only.
- **Protected competition test:** 2021-01 through 2026-08. Test outcomes are documentation/confirmation, not a reason to retune an already-frozen model.
- **Model vs portfolio:** alpha models produce scores/rankings; portfolio construction controls beta, sector, liquidity, concentration, and turnover.
- **Primary ranking diagnostic:** monthly cross-sectional Rank IC.
- **Portfolio diagnostics:** competition IR vs TB3M + 4%, Sharpe, alpha/beta, drawdown, turnover, and long/short attribution.

## 1. Baselines and model research

| Experiment | Objective | Key result | Status | Main caveat |
|---|---|---|---|---|
| [Elastic Net](../experiments/elastic_net/README.md) | Transparent linear characteristic-return baseline | 2019 Rank IC 0.0845 / Sharpe 0.629; 2020 Rank IC -0.0027 | Baseline | Unstable across years; 2018 locked refit degenerated |
| [LightGBM](../experiments/lightgbm/README.md) | Test whether nonlinear regression improves stability | Pooled Rank IC 0.0214; gross Sharpe 0.163 | Frozen negative/challenger | Strong regime dependence and concentrated feature gain |
| [LambdaMART](../experiments/lambdamart/README.md) | Align the learning objective with cross-sectional stock ranking | V1 pooled Rank IC ~0.0684; V2 risk-controlled Rank IC ~0.0513, Sharpe ~0.601, gross competition IR ~0.46 | Current lead numerical architecture | 2020 remains a genuine regime/ranking failure |

## 2. Text-layer research

| Experiment | Objective | Validation result (2018–2020) | Frozen final test (2021–2026) | Status |
|---|---|---|---|---|
| [LambdaMART V2 + text ablation](../experiments/lambdamart_text/README.md) | Test whether 8-K-derived txt_v1 / txt_v2 add incremental information to the 147-characteristic ranker | Full text Rank IC 0.0453 vs 0.0513 baseline; paired ΔIC -0.0060, t -1.36. Net IR 0.664 vs 0.399; Sharpe 0.864 vs 0.601; max DD -26.5% vs -44.1% | Full text Rank IC 0.0973 vs 0.0925; net IR 3.387 vs 2.879; Sharpe 3.564 vs 3.036; max DD -17.8% vs -37.2% | **Not validation-approved**. Favorable portfolio outcomes do not override the locked signal rule |

### Text-layer interpretation

The current evidence does **not** establish that text improves broad cross-sectional return ranking. On validation, both text variants reduced overall Rank IC. At the same time, portfolio metrics improved materially. This creates a specific unresolved diagnostic question rather than an adoption decision:

> Does text improve selection specifically in the traded tails, or does it mainly identify downside/idiosyncratic risk that changes portfolio outcomes without improving full-universe Rank IC?

The next diagnostic should therefore examine tail ranking quality, long/short migration, downside-risk prediction, and incremental P&L attribution using 2018–2020 only for inference. The exact frozen diagnostic may then be applied once to 2021–2026 for confirmation.

## 3. Current research ladder

```
147 characteristics
        |
        +--> Elastic Net -------- transparent linear floor
        |
        +--> LightGBM ----------- nonlinear regression challenger
        |
        +--> LambdaMART V1 ------ direct learning-to-rank
                  |
                  +--> V2 portfolio controls
                           |
                           +--> txt_v1
                           |
                           +--> txt_v1 + txt_v2
                                      |
                                      +--> text mechanism diagnostic (pending)
```

## Research discipline

1. Do not choose features, thresholds, or architectures from 2021–2026 outcomes.
2. Do not equate a higher portfolio Sharpe with a better alpha model.
3. Report prediction/ranking evidence separately from portfolio construction evidence.
4. Preserve failed and inconclusive experiments.
5. Treat unusually strong protected-test performance as something to stress-test, not as permission to retune.
