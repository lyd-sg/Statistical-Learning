# DSA211 — Online Shoppers Purchase Prediction

**Project:** Predict whether a browsing session results in a purchase (`Revenue=1`). The analysis follows the regression and classification methods discussed in the module.

## Project files

- [Online shoppers analysis](online_shoppers_analysis.ipynb) — Jupyter notebook, with executed tables, figures and classification results saved directly in the file.
- [Original UCI dataset](data/online_shoppers.csv) — 12,330 session records, 17 predictors plus the outcome. The source data, including identical duplicate rows, have not been altered.
- [Python dependencies](requirements.txt) — tested package versions for Python **3.12**.
- [27 October meeting notes](27%20Oct%20Meeting.md) — group discussion and unresolved modelling decisions.

## How to run the notebook

From the repository root:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

Open `online_shoppers_analysis.ipynb` and choose **Run → Run All Cells**. The notebook reads `data/online_shoppers.csv`; it also supports running with the working directory one level below the repository root.

To execute from a terminal and save all output cells into a new notebook:

```bash
OPENBLAS_NUM_THREADS=1 OMP_NUM_THREADS=1 MKL_NUM_THREADS=1 \
  jupyter nbconvert --to notebook --execute online_shoppers_analysis.ipynb \
  --ExecutePreprocessor.timeout=3000 \
  --output online_shoppers_analysis_executed.ipynb
```

On Windows PowerShell, omit the `OPENBLAS_NUM_THREADS=...` prefix and run the `jupyter nbconvert` command on one line.

The notebook creates `outputs/` locally for regenerated CSVs, figures and diagnostic records. These generated files are deliberately excluded from Git because the executed notebook already contains the main results. Do not delete or modify the `data/` CSV.

## Analysis and discussion points

The working analysis compares logistic regression, linear discriminant analysis, Gaussian naïve Bayes and Bernoulli naïve Bayes. It uses stratified 80/20 train-test partitioning and common five-fold training cross-validation, with transformations fitted inside the relevant training folds.

**Decisions for group discussion:** Candidate changes are evaluated sequentially with a conservative one-standard-error criterion. Models are ranked by mean cross-validated ROC-AUC, with average precision and fold variation as supporting evidence. A hypothetical upselling scenario illustrates relative false-positive/false-negative costs; its assumptions are **not measured from the dataset**. These choices are clearly described in the notebook rather than presented as prior group consensus.

The current analysis selects **logistic regression with log-transformed browsing activity**; mean five-fold ROC-AUC is approximately **0.7575**, while the reserved-set ROC-AUC is approximately **0.7405**. The reserved observations have been examined during earlier development, so their score should **not** be described as a pristine independent test. The reported results are prediction-only and do not show real-world upsell benefits.

All model results can be regenerated using the supplied data and environment. No files from another repository are required.
