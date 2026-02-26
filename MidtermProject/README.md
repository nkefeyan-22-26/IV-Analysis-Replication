# Replication: Angrist & Evans (1998) — Children and Parental Labor Supply

## Track
**Track B: The Endogeneity & IV Strategy Track (Instrumental Variables)**

## Paper
Angrist, J. D., & Evans, W. N. (1998). Children and Their Parents' Labor Supply:
Evidence from Exogenous Variation in Family Size.
*American Economic Review*, 88(3), 450–477.

## Causal Question
Does having more children causally reduce parental labor supply (hours worked / earnings)?
Because family size is a choice (endogenous), the authors instrument for it using the
sex composition of the first two children. They claim that parents are more likely to have a third child
if the first two are the same sex, a preference unrelated to labor market outcomes.

## Data Source
Downloaded from the Angrist Data Archive at MIT Economics:
https://economics.mit.edu/people/faculty/josh-angrist/angrist-data-archive

The original file (`m_d_806.sas7bdat`) is a SAS binary format common in academic economics
research. It was loaded into Python, optimized, and saved as compressed `.csv.gz` files.
Two versions of the processed data are stored in `data/raw/` — see `data/raw/README.md`
for a full explanation of the files and how to recreate them from the original source.

## Working Repository Structure
```
data/
└── raw/
    ├── README.md                  # Data documentation
    ├── census80_slim.csv.gz       # 20 key variables — used in all notebooks
    └── census80_full.csv.gz       # All 85 variables — for reference
notebooks/
└── 01_Data_Cleaning.ipynb
```

## Environment
- Python 3.10+
- pandas, pyreadstat, numpy, jupyter
