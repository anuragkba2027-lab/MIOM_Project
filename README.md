# SECOM Sensor Analysis and Anomaly Detection

Exploratory analysis of semiconductor process measurements using missing-data
diagnostics, PCA, KMeans and DBSCAN, with timestamp-based inspection of anomaly
patterns.

**Project type:** collaborative academic research. This repository is Anurag
Kumar's fork of [KarthikAshtekar/MIOM_Project](https://github.com/KarthikAshtekar/MIOM_Project).
The upstream contributors and commit history retain their attribution.

## Research question

Can structure in high-dimensional manufacturing sensor measurements identify
unusual observations, and how do those patterns relate to recorded pass/fail
outcomes over time?

An unusual observation is not automatically a defective product. This is an
exploratory notebook, not a production failure-prediction system.

## Start here

- [Main notebook](notebooks/MIOM_standardized.ipynb)
- [Detailed pipeline report](Project_Pipeline_Detailed_Report.txt)
- [Python dependencies](requirements.txt)
- [Original SECOM dataset and documentation](https://archive.ics.uci.edu/dataset/179/secom)

The earlier README referred to `MIOM_fixed.ipynb`, which is not present in this
repository. The notebook linked above is the available implementation.

## Workflow

1. Load the SECOM measurements and inspect missingness.
2. Remove heavily missing features; add missingness indicators and median-impute
   retained sensor features.
3. Remove constant features, standardize measurements and apply PCA.
4. Inspect cluster diagnostics and compare KMeans with density-based DBSCAN noise
   flags.
5. Examine timestamp patterns and descriptive associations with pass/fail labels.

The notebook writes intermediate tables under `data/processed/`. Reports and
existing outputs are research artifacts, not independently certified findings.

## Local setup

From the repository root:

```bash
python -m venv .venv
# Windows PowerShell: .venv\Scripts\Activate.ps1
# macOS/Linux: source .venv/bin/activate
python -m pip install -r requirements.txt
python -m jupyter notebook notebooks/MIOM_standardized.ipynb
```

Obtain the source archive from UCI and place it at `data/raw/secom.zip` before
running the notebook. The loader expects `secom.data` inside that archive.
Label-loading alternatives are specified in the notebook; retain the source
record order when aligning labels with measurements.

The raw archive is not currently present in the repository's `data/raw/`
directory. A fresh clone therefore needs this data-download step.

## Validation status and known limitations

This documentation update inspected source structure; it did not rerun the
scientific workflow or certify historical notebook outputs.

- **Label polarity needs correction before interpreting failure metrics.** UCI
  defines `-1 = pass` and `+1 = fail`. The current notebook's `time_analysis`
  instead marks `true_label == -1` as `is_failure`. Existing precision/recall
  values based on that mapping must not be advertised as failure-detection
  performance.
- Imputation, scaling, PCA and clustering currently use the full sample. They
  support descriptive exploration, not an untouched temporal prediction test.
- A KMeans cluster's numeric label is arbitrary; cluster 0 is not intrinsically
  the anomaly or failure class.
- The next timestamp-sorted row is not necessarily the next observation from
  the same production entity or a fixed prediction horizon.
- DBSCAN noise flags and silhouette scores do not establish useful defect
  prediction or causality.
- The dependency file uses version ranges rather than a fully pinned environment.

Before making a predictive-performance claim, correct label semantics, define a
meaningful target horizon, reserve chronological evaluation data, fit all
preprocessing on training data only and regenerate the evidence.

## Collaboration and attribution

The repository documents a group workflow. Repository ownership does not imply
sole authorship of the underlying analysis. Personal contribution statements
should be supported by the team's agreed roles and commit history; no individual
role is invented in this README.

Dataset citation: McCann, M. and Johnston, A. (2008), *SECOM*, UCI Machine Learning
Repository, [doi:10.24432/C54305](https://doi.org/10.24432/C54305).
Dataset licence: CC BY 4.0, as stated by UCI.
