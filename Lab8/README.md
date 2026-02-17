## Hypothesis Testing & Causal Evidence Architecture
### *The Epistemology of Falsification: Causal Inference on the Lalonde Dataset*

---

### Objective

Most applied ML workflows optimize for estimation — minimizing loss, maximizing predictive accuracy. This project takes a deliberate step upstream, shifting the frame from *"what does the model predict?"* to *"what can the data actually prove?"*

Using the canonical **Lalonde (1986)** job-training dataset, I operationalized the scientific method as a formal causal evidence architecture — adjudicating between competing narratives of treatment effect through rigorous hypothesis falsification rather than point estimation alone.

---

### Technical Approach

- **Parametric Testing (Welch's T-Test via SciPy):** Computed the Average Treatment Effect (ATE) of vocational job training on real earnings by treating the T-statistic as a signal-to-noise ratio — quantifying how many standard errors separate the treatment and control distributions. Welch's formulation was selected over Student's T to avoid the homogeneity-of-variance assumption, which is rarely defensible on observational income data.

- **Non-Parametric Validation (Permutation Test, n=10,000 resamples via SciPy):** To stress-test the parametric result against the non-normal, right-skewed structure of earnings distributions, I implemented a full permutation test. By randomly shuffling treatment labels across 10,000 iterations and reconstructing the empirical null distribution, the observed ATE was benchmarked against a world where the intervention had no effect — without assuming any underlying distributional form.

- **Type I Error Control:** Both testing pipelines were evaluated against a pre-specified significance threshold (α = 0.05) to guard against false positives. Convergence between the parametric and non-parametric p-values was treated as the evidentiary standard for rejecting the null hypothesis — not either result in isolation.

---

### Key Findings

The analysis identified a statistically significant lift in real earnings of approximately **$1,795** attributable to job training, with the null hypothesis of zero treatment effect rejected via **Proof by Statistical Contradiction**. Critically, this finding held across both the parametric and non-parametric regimes, lending robustness to the causal claim that is absent from single-method analyses.

---

### Business Insight: Hypothesis Testing as the Safety Valve of the Algorithmic Economy

In production data environments, the risk isn't that models fail to find patterns — it's that they find too many. With sufficient features, enough resamples, or a sufficiently flexible model class, any dataset will yield *something* that looks significant. This is the mechanics of **data dredging**, and its outputs are indistinguishable from genuine signal until they fail catastrophically in deployment.

Rigorous hypothesis testing — with pre-specified null hypotheses, controlled error rates, and non-parametric cross-validation — functions as the **safety valve** of the algorithmic economy. It enforces a formal evidentiary burden before a correlation graduates into a causal claim, a causal claim graduates into a business decision, and a business decision graduates into a production system affecting real users.

The Lalonde analysis is a microcosm of this discipline: a structured proof-by-contradiction framework that asks not *"is there a number here?"* but *"is there a number here that the data couldn't have produced by chance?"* That distinction is the difference between a data scientist and a pattern-matcher.

---

*Dataset: Lalonde, R. (1986). Evaluating the Econometric Evaluations of Training Programs with Experimental Data. American Economic Review.*
