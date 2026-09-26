# EXPERIMENTS.md — Master Research Tracking Table

This is the top-level index for the FIAM numerical research program. Negative and inconclusive results are retained deliberately.

## Shared research frame

- **Target:** supplied next-month excess return ret_exc_lead1m, never shifted again.
- **Primary research boundary:** target months through 2020-12.
- **2021–2026:** already viewed; post-hoc documentation only, not a tuning set.
- **Model vs portfolio:** alpha models produce scores/rankings; portfolio construction controls implementation and risk.
- **Primary signal diagnostic:** monthly cross-sectional Rank IC.
- **Portfolio diagnostics:** competition IR, Sharpe, drawdown, turnover, beta, costs and long/short attribution.

## Experiment ledger

| Stage | Research question | Main evidence | Decision |
|---|---|---|---|
| [Elastic Net](../experiments/elastic_net/README.md) | Is a transparent linear map sufficient? | Some signal, unstable by year | Keep as baseline |
| [LightGBM](../experiments/lightgbm/README.md) | Does nonlinear regression improve stability? | Pooled Rank IC ~0.021; gross Sharpe ~0.16; strong regime dependence | Freeze as challenger / negative stability result |
| [LambdaMART](../experiments/lambdamart/README.md) | Does learning-to-rank better match cross-sectional trading? | Stronger ranking evidence; V2 RC Sharpe ~0.60 but high drawdown/turnover | Promote ranking architecture |
| [LambdaMART + text](../experiments/lambdamart_text/README.md) | Do 8-K text features add stable incremental information? | Portfolio improved but paired validation IC did not; follow-ups showed extreme-observation concentration | **Not adopted** |
| Nathan replication / common-engine work | Is an economics-first signal or institutional PM complementary? | Nathan signal validation-negative under matched Phase 1 harness; simple signal blend lacked validation support | Do not blend yet; retain PM ideas |
| [Phase 1](../experiments/phase1_signal_comparison/README.md) | Nathan B1 vs frozen LambdaMART under matched strict/relaxed PM | LambdaMART + Strict B1 PM: net Sharpe 0.929, net DD -4.4%, turnover 10.3% | Promote Cell C |
| [Phase 1.5](../experiments/phase1_5_explain_c/README.md) | Why does Cell C work? | Position cap, tradeability, SI veto and turnover discipline most associated with improvement | Freeze current candidate |

## Current candidate

**LambdaMART Institutional Strategy V1 = frozen LambdaMART V2 alpha signal + Strict B1-style institutional portfolio construction.**

See:
- [Team briefing](TEAM_BRIEFING.md)
- [Research journey](RESEARCH_JOURNEY.md)
- [Frozen strategy specification](STRATEGY_SPEC.md)

## Research discipline

1. Do not choose features, thresholds or architectures from 2021–2026 outcomes.
2. Do not equate higher portfolio Sharpe with a better alpha model.
3. Keep prediction evidence separate from portfolio-construction evidence.
4. Preserve failed and inconclusive experiments.
5. Treat ablations as interacting counterfactuals, not isolated causal estimates.
6. Do not tune a parameter simply because removing it changed historical Sharpe.
