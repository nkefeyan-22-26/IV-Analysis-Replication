# AI Capex Diagnostic Modeling

## Objective
Diagnose and correct structural failures in an OLS regression model predicting AI software revenue from Nvidia's 2026 capital expenditure and deployment data, with a focus on identifying heteroscedasticity-induced inference distortions and restoring statistical validity through HC3-robust estimation.

---

## Methodology

- **Data Sourcing & Preparation:** Ingested 2026 Nvidia AI capital expenditure and deployment figures using `pandas`, performing exploratory inspection to characterize the distribution of capex tiers and deployment metrics prior to modeling.

- **Naive OLS Estimation:** Constructed a baseline Ordinary Least Squares model via `statsmodels` to establish initial coefficient estimates and p-values for AI software revenue as a function of capital expenditure and deployment activity.

- **Heteroscedasticity Diagnostics:** Conducted residual analysis using `matplotlib` and `seaborn` to visualize error variance across fitted values. Applied the Breusch-Pagan test to formally test the null hypothesis of homoscedasticity, confirming its rejection at conventional significance thresholds.

- **Multicollinearity Assessment:** Computed Variance Inflation Factors (VIF) across all predictors to detect linear dependencies within the regressor matrix, assessing the degree to which collinearity inflated coefficient standard errors independent of the heteroscedasticity problem.

- **HC3 Robust Correction:** Re-estimated the model using MacKinnon-White HC3 heteroscedasticity-consistent standard errors — the preferred small-sample robust estimator — to produce asymptotically valid inference without altering point estimates.

- **Comparative Inference Analysis:** Benchmarked naive versus robust model output side-by-side, quantifying the divergence in standard errors, t-statistics, and p-values attributable to uncorrected error variance misspecification.

---

## Key Findings

The naive OLS model exhibited **severe heteroscedasticity concentrated at high capital expenditure tiers**, where residual variance expanded systematically with fitted values — a pattern consistent with multiplicative error structure in high-growth technology investment data. This violated the Gauss-Markov assumption of spherical disturbances and rendered the model's standard errors artificially compressed, producing **anti-conservative p-values** that overstated the statistical significance of several deployment metrics.

Applying HC3-robust standard errors appropriately widened the confidence intervals for the affected coefficients, reclassifying predictors whose significance had been inflated by the misspecification. The corrected model reveals that **naive OLS, when applied uncritically to heteroscedastic capex data, risks treating noise as signal** — a consequential error in any policy or investment inference context. The findings underscore the necessity of routine diagnostic validation before drawing econometric conclusions from high-variance financial datasets.

---

## Tech Stack
`Python` · `pandas` · `statsmodels` · `matplotlib` · `seaborn`

---

## Data
2026 Nvidia AI Capital Expenditure & Deployment Dataset
