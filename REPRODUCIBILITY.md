# Reproducibility Guide

This document explains how to rerun the FARS 2023 speed-related crash and injury-severity analysis.

## 1. Analysis environment

The notebook was prepared for Google Colab and uses Python. The code imports these main libraries:

- pandas
- NumPy
- Matplotlib
- SciPy
- statsmodels
- IPython display utilities
- Python standard-library modules such as os, glob, and zipfile

The notebook does not currently pin exact package versions. For a fully version-locked environment, record the package versions from the runtime used for the final analysis.

## 2. Obtain the data

1. Visit the NHTSA Fatality Analysis Reporting System (FARS) website and obtain the official 2023 national CSV data.
2. Use the official FARS 2023 national CSV archive, named `FARS2023NationalCSV.zip` in the notebook's upload instructions.
3. Do not commit the raw FARS archive or extracted person, vehicle, and accident files to this repository. Follow the data provider's terms and cite the source when using the data.

The notebook expects the ZIP archive to be uploaded into the Colab session. It extracts the archive and searches for these CSV files:
- `accident.csv`
- `person.csv`
- `vehicle.csv`

If the official archive uses a different filename or folder structure, check the extracted file names and adjust the notebook's file-discovery step.

## 3. Run the notebook in Google Colab

1. Open `FARS_2023_Traffic_Safety_Research.ipynb` in Google Colab.
2. Choose **Runtime → Run all** only after checking the data-upload cell and confirming the official data archive is available.
3. When the upload prompt appears, select `FARS2023NationalCSV.zip`.
4. Allow the notebook to extract the archive, load the CSV files, clean and combine the records, create analysis samples, fit the regression models, and generate tables and figures.
5. Review any error messages. If a cell expects an output CSV or a variable created by an earlier cell, run the notebook from the beginning in order.

The notebook currently uses Google Colab paths such as `/content/` and `/content/FARS_Research_Outputs/`. These paths are specific to Colab and may need to be changed for a local computer.

## 4. Main analysis steps

The notebook's workflow includes:

1. Upload and extract the 2023 FARS national CSV archive.
2. Load accident, person, and vehicle records.
3. Prepare occupant-level analysis data and classify speed-related status and injury severity.
4. Summarize injury severity by speed-related status and lighting conditions.
5. Fit logistic regression models, including speed-by-lighting interactions, with crash-level clustered robust standard errors.
6. Run sensitivity analyses, including a reported-travel-speed subset and a fatal-only outcome.
7. Export tables and figures for reporting.

For the main outcome, severe/fatal injury is defined using the notebook's `INJ_SEV` coding (codes 3 and 4); non-severe categories are codes 0–2. The notebook treats `SPEEDREL = 0` as not speed-related and codes 2–5 as speed-related; unknown or unclassifiable status is excluded from that comparison. Consult the notebook cells for the exact cleaning and sample-selection logic.

## 5. Output files

The notebook writes intermediate tables and figures to Colab locations including `/content/FARS_Research_Outputs/` and, in some cells, `/content/`. Download the desired outputs from the Colab session after execution. The repository's existing files under `outputs/figures/` and `outputs/tables/` are prepared outputs; rerunning the notebook may create files with different names or in different locations, so compare them before replacing repository files.

## 6. Reproducibility checks

After running the notebook, check that:

- The notebook finds the accident, person, and vehicle CSV files.
- The sample-flow audit reports the expected record counts for the same source-data release and cleaning rules.
- Model-fitting cells complete without errors.
- The generated tables and figures have the expected columns, labels, and units.
- Any differences from the repository's saved outputs are investigated rather than silently overwritten.

Exact numerical reproduction can depend on the FARS data release, package versions, and execution environment. The notebook currently does not provide a pinned environment file or automated test suite.

## 7. Interpretation and limitations

FARS is a census of fatal motor-vehicle crashes, not a dataset of all crashes. The analysis therefore describes occupant records in fatal crashes and should not be generalized to all traffic crashes or interpreted as a causal estimate of injury risk. Observational associations may reflect confounding and selection effects.

## 8. Citation

Cite the National Highway Traffic Safety Administration's Fatality Analysis Reporting System (FARS) 2023 data and the project repository when reusing the analysis. Include the access date and the exact data release/version used in your own work.
