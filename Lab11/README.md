# Data Wrangling & Engineering Pipeline

## Objective
To systematically engineer structural features and impute missing values across a high-entropy HR economics dataset, producing a clean, collinearity-free design matrix suitable for downstream econometric estimation.

---

## Methodology

- **Missingness Auditing (MAR Diagnosis):** Leveraged `missingno` to visualize and map missingness patterns across variables. Confirmed that missing data followed a Missing At Random (MAR) structure — conditioned on observed covariates — justifying principled imputation over listwise deletion.

- **Categorical Encoding & Dummy Variable Trap Avoidance:** Applied one-hot encoding to nominal categorical variables via `pandas`, with deliberate reference class dropping to eliminate perfect multicollinearity. This ensured full-rank design matrices compliant with the Gauss-Markov assumptions required for OLS identification.

- **Target Encoding for High-Cardinality Geographies:** Deployed `category_encoders`' Target Encoder to compress high-cardinality geographic features — replacing sparse indicator columns with continuous, outcome-informed representations. This reduced dimensionality while preserving the predictive signal embedded in regional variation.

- **Pipeline Integration:** Orchestrated all transformation steps using `pandas` and `statsmodels` within a reproducible preprocessing pipeline, ensuring consistent feature engineering across training and evaluation sets.

---

## Key Findings

Systematic preprocessing resolved three structurally distinct data quality problems inherent to the raw dataset:

1. **MAR Missingness Confirmed:** Missingness was not random noise — it was conditioned on observable variables. Recognizing this structure prevented biased imputation and preserved the integrity of subsequent model estimates.

2. **Multicollinearity Neutralized:** Explicit reference class exclusion during dummy encoding successfully bypassed the Dummy Variable Trap, yielding an invertible regressor matrix and numerically stable coefficient estimates.

3. **Dimensionality Reduced Without Information Loss:** Target Encoding collapsed geographic cardinality into a compact continuous feature, retaining cross-regional economic signal that sparse one-hot representations would have obscured or inflated.

The resulting dataset is structurally sound, econometrically identified, and ready for regression modeling.

---

*Stack: Python · pandas · statsmodels · missingno · category_encoders*
