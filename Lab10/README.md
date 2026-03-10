# Lab 10: Correlation, Causality, and Spurious Regression

> **Course context:** Macroeconometrics | U.S. FRED Monthly Data | Python

---

## Objective

This lab investigates a problem more dangerous than a bad model: a model that *looks* statistically strong but captures no true causal relationship. Using real U.S. macroeconomic data, I systematically built, broke, and repaired a regression — documenting at each stage *why* the apparent signal was misleading and what a more defensible specification requires.

---

## Dataset

- **Source:** Federal Reserve Economic Data (FRED) via `pandas_datareader`
- **Frequency:** Monthly, 2010–2024
- **Core variables:**

| FRED Code | Variable | Role |
|-----------|----------|------|
| CPIAUCSL | Consumer Price Index | Dependent variable |
| UNRATE | Unemployment Rate | Demand-side indicator |
| FEDFUNDS | Federal Funds Rate | Monetary policy lever |
| INDPRO | Industrial Production | Supply-side output |
| RSAFS | Retail Sales | Consumer demand proxy |
| DGS10 | 10-Year Treasury Yield | Financial conditions |
| PAYEMS | Nonfarm Payrolls | Labor market |
| M2SL | M2 Money Supply | Monetary aggregate |

- **Expanded variables (Step 8):** WTI crude oil (`DCOILWTICO`), Producer Price Index (`PPIACO`), Housing Starts (`HOUST`), Consumer Sentiment (`UMCSENT`)

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| `pandas` | Data ingestion, resampling, transformation |
| `statsmodels` | OLS estimation, VIF diagnostics |
| `seaborn` / `matplotlib` | Correlation heatmaps, specification comparison charts |
| `networkx` | DAG visualization |
| `plotly` | Interactive time-series dashboard |

---

## Methodology

### Phase 1 — The Correlation Trap
Built a raw correlation heatmap on level variables. Observed correlations of 0.90+ between trending series (CPI, payrolls, M2, retail sales). Identified this as a shared-trend artifact, not economic signal.

### Phase 2 — The Naive Regression
Estimated OLS with CPI levels as the dependent variable and seven macro predictors. Achieved R² ≈ 0.99 — a textbook teaching trap. Coefficient signs were economically implausible, a symptom of multicollinearity distorting individual estimates while overall fit remained artificially high.

### Phase 3 — VIF Forensics
Computed Variance Inflation Factors for each predictor. Most trending variables returned VIFs in the hundreds, far beyond the standard threshold of 10. Applied an iterative pruning loop — dropping the single highest-VIF predictor per round — until all remaining predictors fell below the threshold. Re-estimated the pruned model and compared coefficient stability.

### Phase 4 — Transformation
Converted level variables to Year-over-Year (YoY) percentage growth rates using `.pct_change(12) * 100`. Re-plotted the correlation heatmap. Correlations that had been 0.95+ in levels collapsed to 0.1–0.4 in growth rates, confirming that the original structure was trend-driven rather than structurally meaningful.

### Phase 5 — Causal Forensics (DAG)
A student observing a positive correlation between the federal funds rate and inflation might conclude that raising rates *causes* inflation — the opposite of the intended policy effect. I constructed a Directed Acyclic Graph (DAG) documenting three alternative causal explanations:
- **Reverse causality:** The Fed raises rates *in response to* inflation, not before it
- **Demand shock confounding:** A positive aggregate demand shock simultaneously raises inflation and triggers Fed tightening — both move together due to a common cause
- **Omitted variable (inflation expectations):** Unanchored expectations raise both actual inflation and the Fed's policy response, opening a back-door path invisible to standard OLS

### Phase 6 — Specification Comparison (Step 7)
Re-estimated the same model in three forms — raw levels, YoY growth rates, and first differences — and compared R², Adjusted R², and AIC side by side. Documented that R² collapsed across transformations and that several coefficients changed sign, confirming the levels model was fitting trend rather than economic relationships.

### Phase 7 — AI Co-Pilot Expansion (Steps 8–10)
Extended the project under a structured co-pilot workflow. Each AI-generated code block was reviewed, verified against economic theory, and annotated before inclusion. Tasks completed:
- Added four new FRED variables with economic justification
- Designed robustness checks (lagged predictors, COVID subsample exclusion)
- Built an interactive Plotly dashboard

---

## Theoretical Question

### Why can a positive correlation between inflation and interest rates be consistent with contractionary monetary policy — and how do policy reaction functions create reverse-causality confusion in observational data?

Contractionary monetary policy works by raising the federal funds rate to suppress demand, slow credit growth, and ultimately reduce inflation. The *intended* causal arrow runs:

```
Fed Funds Rate ↑  →  Borrowing costs ↑  →  Demand ↓  →  Inflation ↓
```

This is a negative effect: higher rates should produce lower inflation, all else equal. Yet in raw observational data, the federal funds rate and inflation are frequently *positively* correlated. This is not a contradiction — it is a consequence of how monetary policy is actually conducted.

**The policy reaction function** describes the Fed's decision rule. The Fed does not set rates in a vacuum; it responds to economic conditions. When inflation rises, the Fed raises rates in response. This means that in any historical dataset, periods of high inflation will tend to coincide with high interest rates — not because rates *caused* the inflation, but because the Fed was *reacting* to it. The arrow of causality runs from inflation to the rate decision, opposite to what a naive regression assumes:

```
Inflation ↑  →  Fed observes data  →  Fed Funds Rate ↑
```

This is **reverse causality**: the outcome (inflation) causes the treatment (rate hike), not the other way around. A standard OLS regression cannot distinguish these directions from observational data alone.

The problem is compounded by **common cause confounding**. A positive aggregate demand shock — say, a fiscal stimulus program or a commodity price surge — simultaneously pushes inflation upward *and* forces the Fed to tighten. Both the rate and inflation rise together, but the underlying driver is the shock, not a causal link between rate and price level. Controlling for the shock would substantially weaken or reverse the observed correlation.

Finally, **inflation expectations** act as an omitted variable. If firms and households expect higher future inflation, they raise prices and wages preemptively — pushing current inflation up. The Fed, watching expectation surveys, raises rates in anticipation. Both series move together because they share a common informational cause that does not appear in standard macro datasets.

The combined result: in observational macro data, the Fed's own competence generates a spurious positive correlation between its policy instrument and the outcome it is trying to reduce. Identifying the true causal effect of monetary policy requires instruments, structural models, or high-frequency identification strategies — none of which are visible in a raw correlation heatmap.

---

## Key Takeaways

1. **High R² is not evidence of a good model.** In trending macro data, it is often evidence of a shared time trend being fit, not economic relationships.
2. **VIF diagnoses the symptom, not the cause.** Multicollinearity tells you predictors are redundant; it does not tell you which one to keep. That requires theory.
3. **Transformation is not cosmetic.** Converting levels to growth rates changes what question the model answers: from "do these series share a trend?" to "do changes in X coincide with changes in Y?"
4. **Correlation encodes all causal structures simultaneously.** A positive correlation between X and Y is consistent with X→Y, Y→X, Z→X and Z→Y, or pure coincidence. Only a causal model — like a DAG — can adjudicate between them.
5. **The Fed's reaction function is a confounder.** Any analysis of monetary policy using observational data must account for the fact that policy responds to the very outcomes it is trying to affect.

---

## Files

```
lab10/
├── lab10_main.ipynb        # Full annotated notebook
├── README.md               # This file
└── figures/
    ├── corr_raw.png        # Step 2: Raw correlation heatmap
    ├── corr_transformed.png # Step 5: YoY correlation heatmap
    ├── dag_causal.png      # Step 6: DAG visualization
    ├── vif_comparison.png  # Step 4: VIF table output
    └── spec_comparison.png # Step 7: R² and AIC bar charts
```
