# The Hidden Cost of Parenthood on Women's Careers

Causal Evidence on the Labor Market Impact of Family Size — Replication & Extension of Angrist & Evans (1998)
**Date:** March 2026

---

Having a third child causally reduces a mother's weeks worked by about 16.6 weeks per year (equal to four months of lost work), while having virtually zero measurable effect on fathers. This penalty is the largest for White mothers (-17.5 weeks) and about half as large for Black (-10.6 weeks) and Other (-9.9 weeks) mothers. This reveals that the burden of parenthood on careers is uneven depending on race and gender.

---

## Methodology

The biggest challenge in measuring the effect of children on careers is that family size is a choice. Women who choose to have more children may differ in ways that also affect their careers. They might be more ambitious, riskier, or have different financial or personal values. If we just compared mothers with two versus three children, the effect of the child would be confounded by these differences.

To solve this, we used an **Instrumental Variables (IV)** design, testing a random aspect: the sexes of the first two children. Parents who have two boys or two girls are significantly more likely to have a third child than parents who already have one of each. This shows families' preferences for mixed-sex households. Whether your first two children happen to be the same sex is random, so it has no direct effect on a mother's career trajectory.

This instrument is valid because:

1. Same-sex siblings strongly predict having a third child (F-statistic = 23,961, far above the accepted threshold of 10)
2. The sex of your first two children has no plausible direct effect on how many weeks per year you work, except through the channel of having an additional child

---

## Figure 1: IV-2SLS Estimates of the Child Penalty on Weeks Worked by Race and Parent Gender
![Child Penalty by Race: Mothers vs. Fathers](data/raw/notebooks/forest_plot_race.png.png)
![Child Penalty by Race: Mothers vs. Fathers](data/raw/notebooks/forest_plot_mothers_vs_fathers.png.png)

*Each dot represents the causal effect of having a third child on annual weeks worked for a given race-gender group. The instrument is the same-sex sibling composition. The lines show 95% confidence intervals. The red-dashed line marks zero (no effect).
Data: 1980 U.S. Census extract; Angrist & Evans (1998)*

The left panel shows that the child penalty in weeks reduced for mothers is large and significant across all three race groups, as confidence intervals are below zero. The right panel shows that fathers experience no statistically significant change in weeks worked, regardless of race (all confidence intervals are above zero).

---

## Policy Implications

1. **Parental leave policies are not gender neutral in practice** — the burden of additional children falls almost entirely on mothers. Assuming a shared burden between mothers and fathers is misaligned with this data.

2. **The racial gradient exposes a potential targeted intervention** — White mothers face a penalty nearly twice the size of Black and Other mothers, likely reflecting a structural difference. White mothers in this era were more able to exit the workforce entirely after a third child, while Black mothers had higher baseline labor force participation due to economic necessity. Workplace retention programs that focus on senior or high-earning women (skewed White) might have a harder time succeeding due to White women facing the highest penalty for a third child.

3. **The career tax on mothers is a structural outcome** — the relationship is causal rather than just correlational, telling us that the child causes the labor penalty and nothing else. This is addressable through policies like subsidized childcare, flexible scheduling, and onboarding programs for returning mothers. The expected return is a measurable positive effect on recovered labor supply.


# Replication: Angrist & Evans (1998) — Children and Parental Labor Supply

## Track
**Track B: The Endogeneity & IV Strategy Track (Instrumental Variables)**

## Paper
Angrist, J. D., & Evans, W. N. (1998). Children and Their Parents' Labor Supply:
Evidence from Exogenous Variation in Family Size.
*American Economic Review*, 88(3), 450–477.

## Causal Question
Does having more children causally reduce parental labor supply (hours worked / earnings)?
Because family size is a choice (endogenous), the authors instrument for it using the sex
composition of the first two children — parents are more likely to have a third child if the
first two are the same sex, a preference unrelated to labor market outcomes.

## Key Results

### Core IV Estimates
| Method | Coefficient on `morekids` | Interpretation |
|---|---|---|
| OLS (Biased) | — | Endogenous — not reliable |
| First Stage F-stat | 23,961 | Instrument is strong (>> 10) |
| Manual 2SLS | -16.59 weeks | Correct coef, wrong SEs |
| IV-2SLS (linearmodels) | -16.59 weeks | Correct coef + robust SEs |
| Wald Estimator | -14.58 weeks | No-controls raw IV estimate |

Having more than two children causally reduces a mother's weeks worked by approximately
16.6 weeks per year, identified using same-sex sibling composition as an instrument.

### Heterogeneous Treatment Effects by Race
| Race | Mothers (weeks) | Fathers (weeks) | Fathers Significant? |
|---|---|---|---|
| White | -17.47 | +2.07 | No |
| Black | -10.63 | +2.05 | No |
| Other | -9.88 | +1.38 | No |

The child penalty is large and statistically significant for mothers across all race groups,
but effectively zero for fathers regardless of race. White mothers face nearly twice the
penalty of Black and Other mothers, consistent with documented differences in labor force
attachment in 1980. The asymmetry between mothers and fathers holds universally.

## Data Source
Downloaded from the Angrist Data Archive at MIT Economics:
https://economics.mit.edu/people/faculty/josh-angrist/angrist-data-archive

The original file (`m_d_806.sas7bdat`) is a SAS binary format common in academic economics
research. It was loaded into Python, decoded from byte-string encoding, optimized, and saved
as compressed `.csv.gz` files. See `data/raw/README.md` for full documentation.

## Repository Structure
```
data/
└── raw/
    ├── README.md                          # Data documentation
    ├── census80_slim.csv.gz               # 20 key variables — used in all notebooks
    ├── census80_full.csv.gz               # All 85 variables — for reference
    └── notebooks/
        ├── 01_Data_Cleaning.ipynb         # Data ingestion, decoding, and processing
        ├── 02_Replication_Analysis.ipynb  # IV replication: First Stage, 2SLS, Wald
        └── 03_Extension_Analysis.ipynb    # HTE by race, mothers vs. fathers forest plots
```

## Reproducing the Analysis
1. Clone this repository
2. Download `m_d_806.sas7bdat` from the Angrist Data Archive and place it in `data/raw/`
3. Run `01_Data_Cleaning.ipynb` to reproduce the processed `.csv.gz` files
4. Run `02_Replication_Analysis.ipynb` to reproduce all IV results

## AI Tools Acknowledgement
The heterogeneous treatment effects extension in `03_Extension_Analysis.ipynb`, including the IV-2SLS subgroup models by race and gender, the forest plot visualizations, and the written interpretation of results, were developed with the assistance of Claude (Anthropic, claude.ai). Claude was used as a collaborative coding and writing tool to design the interaction model specifications, generate the forest plots, and draft the methods and results documentation. All analytical decisions and interpretations were reviewed and directed by the project author.

## Environment
- Python 3.10+
- pandas, numpy, statsmodels, linearmodels, matplotlib, jupyter
```
