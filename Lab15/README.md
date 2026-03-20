# Forecasting Architecture and the Bias-Variance Tradeoff

## Objective
Empirically diagnose the Bias-Variance Tradeoff by engineering a high-variance polynomial forecasting model on real corporate financial data, exposing its out-of-sample failure, and quantifying true operational risk through K-Fold Cross-Validation.

---

## Data
**NVIDIA Quarterly Corporate Revenue (2024–2026)**
Real-world time-series financial data used to simulate a production forecasting environment, where model generalization directly governs decision quality.

---

## Tech Stack
- **Language:** Python
- **Libraries:** `pandas`, `numpy`, `scikit-learn` (`PolynomialFeatures`, `LinearRegression`, `cross_val_score`), `matplotlib`

---

## Methodology

- **Baseline Model Construction:** Fit a standard linear regression model to NVIDIA's quarterly revenue figures as an in-sample performance benchmark.
- **Polynomial Feature Expansion:** Engineered a 7th-degree polynomial transformation of the input features using `PolynomialFeatures`, deliberately constructing a high-variance model capable of interpolating the training set with near-zero error.
- **In-Sample vs. Out-of-Sample Evaluation:** Compared training MSE against extrapolated next-quarter predictions to surface the gap between apparent model performance and real-world forecasting reliability.
- **Overfitting Diagnosis:** Demonstrated that a near-perfect training fit produced economically incoherent out-of-sample predictions — so-called "hallucinated" forecasts driven by unconstrained variance.
- **K-Fold Cross-Validation:** Applied K-Fold CV via `cross_val_score` to produce a statistically rigorous estimate of true generalization error, revealing the magnitude of operational risk concealed by in-sample metrics alone.
- **Regularization Motivation:** Used the cross-validated error as empirical justification for the necessity of algorithmic regularization techniques (e.g., Ridge, Lasso) to impose a bias-variance tradeoff that is operationally viable.

---

## Key Findings

The 7th-degree polynomial model achieved a near-zero training MSE, superficially suggesting an excellent fit. However, extrapolation to the next quarterly period produced **catastrophically unrealistic revenue projections**, exposing the model's complete failure to generalize beyond the observed data range.

K-Fold Cross-Validation quantified the true scale of this failure: the cross-validated MSE was **orders of magnitude larger** than the training MSE, confirming that in-sample performance was a misleading signal of model quality. This divergence is the empirical signature of high variance — a model that has memorized noise rather than learned signal.

The results underscore a foundational principle in applied econometrics and machine learning: **optimizing for training error without constraining model complexity is not a forecasting strategy — it is a risk exposure.** Robust production forecasting architectures must incorporate regularization or cross-validated model selection to ensure that reported performance reflects deployable, out-of-sample accuracy.

---

## Skills Demonstrated
`Econometrics` · `Feature Engineering` · `Polynomial Regression` · `Cross-Validation` · `Model Diagnostics` · `Overfitting Analysis` · `scikit-learn` · `Financial Time-Series`
