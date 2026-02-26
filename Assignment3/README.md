# SwiftCart Logistics — Causal Inference & Non-Parametric Statistical Analysis

An end-to-end applied econometrics pipeline built for a multinational on-demand delivery platform. This project tackles three real-world problems where standard parametric statistics fail: zero-inflated compensation data, heteroscedastic A/B test outcomes, and selection bias in loyalty program ROI.

---

## The Problem Space

The executive board faced three contradictory data narratives:

1. **Is the "median driver tip" claim defensible?** Tip data is zero-inflated and right-skewed — the Central Limit Theorem does not apply at audit-scale sample sizes.
2. **Does the new batch-routing algorithm actually reduce delivery times?** An A/B test is confounded by software crash-loop outliers that invalidate the homoscedasticity assumption of a t-test.
3. **Is the SwiftPass loyalty program generating a true 31% revenue lift?** Or is that figure a statistical artefact of high-volume power users self-selecting into the program?

---

## Methodology

### Phase 1 — Bootstrap Inference on Zero-Inflated Tip Distribution

**Data:** 250-observation audit sample — 100 zero tips + 150 draws from Exponential(scale=5.0).

A manual bootstrap engine (10,000 resamples, no `scipy.stats.bootstrap`) was used to construct an empirical confidence interval on the median — the only robust central tendency measure under zero-inflation.

| Statistic | Value |
|---|---|
| Observed Median | $0.76 |
| 95% Bootstrap CI | ($0.26, $1.30) |

**Finding:** The CI is asymmetric and anchored near zero, exposing the PR claim of generous median compensation as statistically indefensible. The entire interval sits below $1.30 — a direct consequence of 40% of drivers receiving no tip at all.

---

### Phase 2 — Permutation Test on A/B Routing Experiment

**Data:** 500 Control deliveries ~ Normal(μ=35, σ=5); 500 Treatment deliveries ~ LogNormal(μ=3.4, σ=0.4).

A manual permutation engine (5,000 iterations, no `scipy.stats.permutation_test`) was used to test whether the observed mean difference could arise by chance, without assuming equal variance across groups.

| Statistic | Value |
|---|---|
| Control Mean | 35.03 min |
| Treatment Mean | 32.77 min |
| Observed Difference | 2.27 min |
| Empirical p-value | 0.0004 |

**Finding:** The result is statistically significant. Only 0.04% of random permutations produced a gap this large, confirming the batch-routing algorithm genuinely reduces delivery times. The log-normal treatment distribution — driven by crash-loop outliers — inflates the treatment mean, meaning the true median improvement is likely even larger.

---

### Phase 3 — Propensity Score Matching & Causal ATT Estimation

**Data:** `swiftcart_loyalty.csv` — pre-treatment covariates (`pre_spend`, `account_age`, `support_tickets`) and post-treatment spending, with subscriber treatment indicator.

A PSM pipeline was constructed using logistic regression to estimate propensity scores and 1-nearest-neighbour matching to pair each subscriber to a statistically identical non-subscriber.

| Metric | Value |
|---|---|
| Subscribers Mean | $74.04 |
| Non-Subscribers Mean | $56.47 |
| **Naive SDO** | **$17.57 (31.1% lift)** |
| Matched Control Mean | $64.13 |
| **ATT (Causal Estimate)** | **$9.91 (15.5% lift)** |
| Selection Bias Component | $7.66 |

**Finding:** The marketing team's 31.1% lift figure is nearly double the true causal effect. The $7.66 gap between SDO and ATT is pure selection bias — high-spending users were always going to spend more, regardless of SwiftPass. The defensible ROI figure for budget decisions is **15.5%**.

---

### Phase 4 — Love Plot: Covariate Balance Diagnostics

A Love Plot of Standardized Mean Differences (SMD) was generated to visually validate that PSM successfully eliminated pre-treatment covariate imbalance.

![Love Plot](love_plot.png)

**Key observations:**
- `pre_spend` had the largest pre-matching SMD (~0.68) — confirming it as the primary driver of self-selection, exactly as the economic theory predicted.
- `account_age` showed a moderate imbalance (~0.33) before matching.
- `support_tickets` was negatively imbalanced (~-0.18) before matching.
- **All three covariates fall within |SMD| < 0.1 after matching**, satisfying the standard econometric threshold for covariate balance.

The Love Plot is the visual proof that the ATT of $9.91 is causally clean.

---

## Stack

- `numpy` — manual bootstrap and permutation engines (no high-level wrappers)
- `pandas` — data wrangling
- `scikit-learn` — logistic regression (propensity scores), nearest-neighbour matching
- `matplotlib` / `seaborn` — Love Plot visualisation

---

## Key Takeaways

| Claim | Naive Reading | Causal Reality |
|---|---|---|
| Median driver tip is generous | — | $0.76, CI touches $0.26 |
| Routing algorithm is inconclusive | t-test invalid | p = 0.0004, 2.27 min reduction |
| SwiftPass drives 31% revenue lift | SDO = $17.57 | ATT = $9.91 (15.5%) |

> *"The naive SDO is not the program's effect — it is a portrait of the users who chose it."*
