# Foraminifera Marine Plankton Analysis

Analysis of long-term morphological and diversity trends in planktonic foraminifera (marine
microfossils) over the last 5 million years, and their relationship to past climate change.

This was a **4-person group project** for the MSc Data Science programme at the University of
Bristol. **This repository contains only the two analysis questions I personally led** (Question 2
and Question 5), plus the shared data-preparation notebook I contributed to. It is not the full
group submission — see [Individual contribution](#individual-contribution) below.

Full group report: [`documentation/DSMP_Report.pdf`](documentation/DSMP_Report.pdf)
Original problem statement: [`documentation/ProblemStatement.pdf`](documentation/ProblemStatement.pdf)

## Background

Foraminifera are marine plankton with an exceptional fossil record, making them a valuable proxy
for reconstructing past climate and ocean conditions. The group project used morphometric (shape
and size) measurements from **over 370 deep-sea sediment samples spanning ~5 million years**, at
ODP Site 925 (Ceara Rise), alongside benthic δ18O / δ13C isotope records as an environmental proxy
for climate.

## My contribution

### Question 2 — Does species diversity change over geological time?
`notebooks/question2.ipynb`

Used unsupervised clustering as a proxy for morphological "species richness" within each time-slice
sample, then tested whether that richness trends over geological time.

- Loaded ~370 individual fossil measurement files (area, sphericity, shape factor, min/max/mean
  diameter), standardised features, and ran **KMeans clustering** on each sample independently.
- Selected the number of clusters per sample using the elbow method (WCSS) over k = 2–59.
- Mapped each sample file to its geological age (Ma) using the shared mastersheet.
- Tested the trend in cluster count (diversity proxy) vs. age using a moving average and **LOESS
  regression**, then quantified it with **Pearson and Spearman correlation**.
- Split the record into an early (2.5–5 Ma) and late (0–2.5 Ma) interval and compared diversity
  distributions between them (histogram + boxplot).
- **Finding**: a mild declining trend in morphological diversity in the more recent 2.5 million
  years.

### Question 5 — Do shape traits track environmental (climate) change?
`notebooks/question5.ipynb`

Tested whether shape parameters (sphericity, shape factor) correlate with independently measured
benthic isotope records (δ18O / δ13C), which serve as a proxy for past ocean temperature.

- Merged morphometric summary data with the environmental isotope dataset on geological age.
- Visualised sphericity/shape factor and the isotope proxies as time series, and their pairwise
  relationships with LOWESS-smoothed scatterplots.
- Computed a correlation matrix and fit **regression models** to quantify the relationship.
- **Finding**: shape traits (sphericity, shape factor) showed a clearer relationship to climate
  proxies than size-based traits did — colder conditions were associated with less spherical, more
  elongate shell shapes.

### Shared: data preparation
`notebooks/datasetwork.ipynb`

Contributed to the shared pipeline that built the group's master dataset from raw lab exports:
standardising ~850 raw sample filenames, converting file formats, filtering the mastersheet to
valid measurements, and matching individual fossil sample files to their metadata (site/hole/core/
section) using regex extraction from filenames.

## Data

The full raw dataset (~850 individual per-sample measurement files) is not included in this
repository due to size and because it was shared/owned across the group project. This repo includes
the processed, tabular outputs needed to run my two analysis notebooks end to end:

| File | Description |
|---|---|
| `data/Mastersheet.xlsx` | Master metadata table: site/hole/core/section, geological age, and per-sample morphometric summary statistics |
| `data/merged_data.xlsx` | Mastersheet joined with benthic δ18O / δ13C isotope records |
| `data/selected_features_clustering_summary.xlsx` | Per-sample KMeans cluster count (output of Question 2, stage 1) |
| `data/selected_features_clustering_summary_with_age.xlsx` | Above, with geological age mapped in |

`question2.ipynb`'s first stage (raw per-sample clustering) expects a folder of individual `.xlsx`
measurement files, which isn't included — but the notebook can be run from the
`selected_features_clustering_summary*.xlsx` stage onward using the files above.

## Tech stack

Python, pandas, NumPy, scikit-learn (KMeans), SciPy (Pearson/Spearman), statsmodels (LOESS,
regression), Matplotlib, Seaborn.

## Running locally

```bash
pip install -r requirements.txt
```

Then open the notebooks in `notebooks/` — each expects the repo's `data/` folder as a sibling
directory (paths are relative, no setup needed beyond installing dependencies).

## Individual contribution

This was originally a group submission (Deepender Deepender, Runzi Ma, Siyu Li, Shashwat Deep,
MSc Data Science, University of Bristol). The full report covers 5 research questions; this
repository contains **only the two I personally designed, coded, and wrote up (Questions 2 and 5)**,
plus my contribution to the shared data-preparation step, extracted for portfolio purposes.
