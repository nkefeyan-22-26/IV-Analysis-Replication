# Data Directory

## Two Data Files

This folder contains two versions of the Angrist & Evans (1998) Census extract:

- **`census80_slim.csv.gz`** — Contains 20 key variables selected for the replication analysis. This is the working file used in all notebooks. It is faster to load and easier to work with.
- **`census80_full.csv.gz`** — Contains all 85 original variables from the raw extract. This file is provided for transparency and reproducibility, in case any additional variables are needed for robustness checks or extensions.

## Data Processing

The original data was sourced as a `.sas7bdat` file, a binary format used by SAS, which is common in academic economics research. It was loaded into Python using `pandas.read_sas()`, optimized by downcasting numeric columns and converting low-cardinality columns to categorical dtype to reduce memory usage, and then saved as a `.csv.gz` file. The `.csv.gz` format is a standard CSV compressed with gzip, which reduces file size significantly while remaining natively readable by pandas with no extra steps (`pd.read_csv("file.csv.gz")` works out of the box).

## Accessing the Original Data

The original raw file can be downloaded directly from the Angrist Data Archive hosted by MIT Economics:

**https://economics.mit.edu/people/faculty/josh-angrist/angrist-data-archive**

Download the file associated with Angrist & Evans (1998) and place it in this folder as `m_d_806.sas7bdat` to reproduce the processing steps in `01_Data_Cleaning.ipynb`.
