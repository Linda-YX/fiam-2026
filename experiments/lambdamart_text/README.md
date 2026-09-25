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


## Validation-only mechanism diagnostic — completed

A frozen diagnostic was run using only the saved 2018–2020 validation predictions, holdings, and monthly returns. No model was retrained, no threshold or feature was selected, and no 2021–2026 outcome was read.

### Executive findings

| Question | Full text vs baseline | Interpretation |
|---|---:|---|
| Tail spread | +0.23% / month, t = 0.267, 95% bootstrap CI [-1.33%, 2.00%] | No reliable tail-ranking improvement |
| Top-decile precision | -0.83%, t = -1.018 | No evidence of better winner identification |
| Bottom-decile precision | -1.67%, t = -2.712 | Worse bottom-decile membership precision |
| Union-tail Rank IC | -0.014, t = -1.123 | Tail ordering did not improve |
| Long replacement benefit | +0.01%, t = 0.015 | Essentially zero average migration benefit |
| Short replacement benefit | +0.01%, t = 0.007 | Essentially zero average migration benefit |
| Down-moved minus up-moved future bottom-5% rate | -0.52%, t = -2.049 | Opposite sign to the downside-demotion hypothesis |
| Rank change vs future bottom-5% indicator | +0.009, t = 2.469 | Opposite sign to the downside-demotion hypothesis |
| Net 10 bp P&L improvement | +0.64% / month, t = 0.880 | Positive but weak average monthly evidence |

The saved artifacts did not contain a future residual-return or idiosyncratic-volatility outcome, so the narrower claim that text predicts idiosyncratic volatility remains untested.

### Extreme-observation concentration

The strongest diagnostic finding is concentration rather than broad ranking improvement:

- the five largest positive name-month contributions equal **112.31%** of aggregate gross full-text improvement;
- the three best incremental months equal **134.70%** of aggregate net improvement;
- shares above 100% mean these extreme gains offset negative incremental contributions elsewhere.

Net 10 bp counterfactual:

| Scenario | Sharpe | IR vs 4% hurdle | Max DD |
|---|---:|---:|---:|
| Baseline | 0.543 | 0.399 | -44.72% |
| Full text | 0.806 | 0.664 | -27.39% |
| Full text without top 5 positive name-months | 0.506 | 0.362 | -36.33% |
| Full text without top 3 incremental months | 0.470 | 0.318 | -41.01% |

Several of the largest positive contributions came from avoiding shorts in stocks that subsequently experienced extreme positive returns, including WKHS (+601%), MCRB (+608%), CODX (+306%), AR (+318%), and QEP (+195%). Other large contributions came from long entries or short-to-long transitions in extreme winners.

### Updated mechanism conclusion

The validation evidence does **not** support H1 (systematically better traded-tail ranking). Realized-downside diagnostics also do not support the simple version of H2 in which text systematically demotes future bottom-tail losers.

The best-supported description is currently:

> **Text changes a small number of economically important positions, and the observed portfolio improvement is heavily concentrated in a handful of extreme stock-months.**

This does not yet distinguish genuine sparse event information from luck. The next research question is therefore whether the extreme movers share ex-ante text/event characteristics that were available before the return realization.

### Sparse event-risk diagnostic — completed

The follow-up test used only frozen 2018–2020 validation predictions/holdings and existing ex-ante txt_v1/txt_v2 features. It did not retrain LambdaMART, tune thresholds, create new text features, or inspect 2021–2026 outcomes.

#### What the extreme observations looked like

Some successful text-induced position changes had conspicuous ex-ante filing/text characteristics. Examples include AR (abrupt-exit / novel-distress features) and CODX (financing / Item 1.01 / Item 8.01 features). However, there was no common signature: **6/10** largest positive and **7/10** largest negative incremental-P&L observations had no current-month filing.

#### Broader matched-population test

Candidate text patterns discovered from the extreme observations were then tested across all comparable validation stock-months, with controls matched within target month × size tercile × liquidity tercile.

- Filing intensity: Δ top-5% event probability **+0.73%**, t = **2.248**, BH q = **0.104**.
- Novelty: Δ top-5% event probability **+0.86%**, t = **2.155**, BH q = **0.104**.
- No candidate pattern had a positive top-5% event-probability difference with BH q < 0.10.
- Transactions/financing produced a significant result in the opposite direction: Δ top-5% probability **-1.42%**, t = **-4.499**, q = **0.001**.

These tests are exploratory because candidate families were motivated by selected extreme winners/losers. The near-threshold filing-intensity and novelty results are retained as possible hypotheses for an independent future sample, not as validated signals.

#### Catastrophic-short hypothesis

The most-upgraded text rank-change decile versus the most-downgraded decile showed:

- Δ top-5% future-return event rate: **+0.58%**, t = **0.982**;
- Δ top-2% event rate: **+0.25%**, t = **0.816**;
- cross-decile monotonicity for top-5% events: **0.024**.

Within the baseline short book, text-avoided names versus retained shorts also did not show a reliable catastrophic-upside pattern. Therefore the evidence does not establish text as a systematic short-squeeze / catastrophic-short detector.

#### Robustness to extreme returns

The original full-text minus baseline improvement was **+0.64% net return per month**. That advantage was highly sensitive to extreme observations:

| Scenario | Δ mean net return / month | t-stat | Δ Sharpe | Δ competition IR |
|---|---:|---:|---:|---:|
| Original | +0.64% | 0.880 | +0.263 | +0.265 |
| Monthly 1/99 winsorization | -0.13% | -0.266 | -0.110 | -0.106 |
| Exclude 5 largest absolute held stock-month returns | +0.13% | 0.204 | +0.050 | +0.052 |
| Exclude 10 largest absolute held stock-month returns | +0.08% | 0.121 | +0.028 | +0.029 |
| Exclude 2020 | +0.16% | 0.232 | +0.294 | +0.273 |

The incremental effect was concentrated in the **short side (83.09%)**, **small stocks (78.02%)**, and **2020 (83.49%)**. The no-current-month-filing category accounted for **141.17%** of aggregate improvement because its gains offset losses elsewhere.

### Final text-layer interpretation

The text layer is **not adopted into the core strategy**.

The correct conclusion is not that text is useless. It did make several economically valuable position changes in validation, including avoiding shorts before a handful of extreme positive returns. However, the follow-up tests did not show that those successes arise from a stable, repeatable 8-K/event pattern.

Current status:

> **Interesting experimental signal, but not validated. Portfolio improvement is real in-sample/validation history, yet it is highly dependent on a small number of extreme stock-months and no systematic text mechanism has been established.**

Accordingly:

- do **not** claim that text broadly improves return ranking;
- do **not** claim that text is a validated downside-risk or catastrophic-short detector;
- retain the text code, features, results, and diagnostics as a documented research branch;
- retain filing intensity and novelty only as exploratory hypotheses for genuinely independent future testing;
- keep **LambdaMART V2 with the 147 characteristics** as the core model while its unusually strong performance is subjected to separate robustness / tradeability stress tests.

## Decision

**NOT ADOPTED / NOT VALIDATION-APPROVED — documented experimental branch.**

The sparse-event follow-up does not validate the remaining event-risk mechanism. The more defensible interpretation is that the observed portfolio improvement is dominated by a handful of extreme observations; whether those particular successful position changes contained genuine information cannot be ruled out, but it has not been shown to be repeatable.
