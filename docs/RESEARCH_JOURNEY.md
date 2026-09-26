# Research Journey

This document records how the strategy evolved, including failed branches and the reasoning behind each change. It is intentionally a research history rather than a polished retrospective.

## 1. Start with transparent baselines

We began with Elastic Net as a simple linear baseline. It showed that the characteristic panel contains some cross-sectional information, but the signal was unstable across research-test years. LightGBM was then used to test whether nonlinear regression could improve stability. It did not: performance was strongly regime-dependent and feature gain was concentrated.

**Lesson:** adding nonlinear flexibility is not enough. The learning objective should match the portfolio problem.

## 2. Move from return regression to ranking

LambdaMART changed the objective from predicting the exact magnitude of next-month returns to learning the cross-sectional ordering of stocks. This was a better match for a long-short portfolio built from relative stock selection.

LambdaMART V1 produced stronger ranking evidence, but the raw portfolio carried unwanted beta, sector and implementation exposures. V2 added portfolio controls. This improved investability, but the 2018–2020 risk-controlled portfolio still had high turnover and a large drawdown.

**Lesson:** prediction quality and portfolio quality are separate research problems.

## 3. Test 8-K text — and keep the negative result

We tested whether 8-K-derived text features added incremental information to the 147-characteristic LambdaMART signal. Portfolio metrics initially looked better, but the locked validation Rank IC test did not improve. Follow-up diagnostics showed that the apparent improvement was concentrated in a small number of extreme observations and was not supported by a stable event-risk mechanism.

The text branch was therefore **not adopted**.

**Lesson:** a better backtest is not enough when the signal-level validation test fails.

## 4. Study an economics-first external benchmark

We reproduced Nathan B1 / the Nathan factor architecture to understand a very different research philosophy: economically signed factor groups, institutional tradeability rules, explicit short-interest controls, beta and sector neutrality, position limits, turnover control and realistic implementation costs.

This was useful for two separate questions:

1. Is Nathan's economic signal complementary to LambdaMART?
2. Is Nathan's portfolio-construction discipline useful even if we keep our own signal?

A common-engine 50/50 signal blend did not provide validation support. Low signal correlation alone was not enough reason to blend.

**Lesson:** diversification between signals is useful only when both parents have defensible validation evidence.

## 5. Phase 1 — signal versus portfolio construction

We froze the signals and compared Nathan B1 versus LambdaMART V2 under the same Strict B1 portfolio engine and one pre-specified relaxed engine.

The key result was Cell C:

> **Frozen LambdaMART V2 signal + Strict B1 portfolio construction**

On 2018–2020 validation, Cell C produced:

- gross Sharpe: **1.097**
- net Sharpe after modeled costs: **0.929**
- net competition IR: **0.248**
- net max drawdown: **-4.4%**
- average one-way turnover: **10.3%**
- realized CAPM beta: **0.015**
- average positions: **234**

The relaxed LambdaMART portfolio raised turnover to roughly 71.5% and reduced net Sharpe to roughly 0.21. This was evidence against the idea that the institutional constraints were simply suppressing LambdaMART alpha.

**Decision:** do not start signal fusion. Keep LambdaMART as the alpha engine and investigate why the strict portfolio layer works.

## 6. Phase 1.5 — explain Cell C, do not improve it

No LambdaMART retraining, threshold search or parameter tuning was allowed. Starting from the frozen Cell C benchmark, one portfolio component was removed at a time for attribution.

The strongest deterioration in validation net Sharpe occurred when removing:

1. the **1% position cap**: 0.929 -> -0.166
2. the **tradeability universe**: 0.929 -> 0.132
3. the **short-interest veto**: 0.929 -> 0.405
4. the **10% turnover cap**: 0.929 -> 0.506

Removing beta neutrality or sector constraints increased sample Sharpe, but those controls are retained for exposure discipline rather than treated as alpha generators.

The turnover diagnostic was especially informative. Removing the cap changed turnover from about 10.3% to 70.0% and monthly trading cost from 3.72 bp to 22.14 bp, while gross Sharpe changed only from 1.097 to 1.076. The strict portfolio also had much higher holdings persistence and longer holding periods.

**Interpretation:** the portfolio layer is doing more than risk reporting. It prevents the ranker from turning small score changes into excessive capital reallocation, limits concentration, removes difficult-to-trade names and controls crowded shorts.

## 7. Current candidate

The current research candidate is:

> **LambdaMART Institutional Strategy V1 = Frozen LambdaMART V2 alpha signal + Strict B1-style institutional portfolio construction**

The signal and portfolio layers remain conceptually separate. We are not claiming that every portfolio constraint increases alpha. Beta and sector controls are retained because the mandate requires a defensible market-neutral portfolio.

## 8. What remains unresolved

- The short leg remains weaker than the long leg.
- 2019 validation is positive but materially weaker than 2018 and 2020.
- Average Rank IC is modest even when portfolio performance is useful.
- 2021–2026 outcomes have already been viewed and are treated as post-hoc documentation, not a tuning set.
- The 2018–2020 sample is only 36 months; the current candidate should not be over-interpreted from a single Sharpe estimate.

The next stage should protect the frozen candidate and focus only on genuinely decision-relevant weaknesses rather than searching for a higher historical Sharpe.
