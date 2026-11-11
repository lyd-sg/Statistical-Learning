# DSA211 — Predicting Online Purchases

**Research question:** How accurately can browsing-session predictors rank the likelihood of a purchase?

Prediction-focused classification analysis using the [UCI Online Shoppers Purchasing Intention dataset](https://archive.ics.uci.edu/dataset/468/online+shoppers+purchasing+intention+dataset). The dataset comprises **12,330 browsing sessions**, **17 original predictors** and a binary target, `Revenue` (purchase / no purchase). Purchases make up **15.47%** of the observations.

## Repository files

- [`online_shoppers_analysis.ipynb`](online_shoppers_analysis.ipynb) — complete, executable analysis: data checks, EDA, model development, comparison, evaluation and conclusions.
- [`data/online_shoppers.csv`](data/online_shoppers.csv) — original input data.
- [`requirements.txt`](requirements.txt) — pinned Python 3.12 dependencies.
- [`.github/workflows/execute-notebook.yml`](.github/workflows/execute-notebook.yml) — notebook execution and validation workflow.

## Analysis in the notebook

| Stage | What is done |
|---|---|
| 1. Data overview | Check completeness and class prevalence; make a stratified 80/20 split (`random_state=42`). |
| 2–3. Training-only EDA | Examine skewness, zeros, rare categorical levels, positive browsing activity, purchase-rate patterns, correlations and class-conditional distributions. |
| 4. Decision cost | Pre-specify a hypothetical business threshold before model fitting. |
| 5. All-17 comparison | Compare four classifiers and full-grown Gini/Entropy trees using all 17 predictors. |
| 6. Operational baselines | Refit all six baselines without `PageValues` (16 predictors). |
| 7. Sequential improvements | Use EDA-guided transformations and feature choices for Logistic Regression, LDA, Gaussian NB and Bernoulli NB. |
| 7B. Classification trees | Compare full versus cost-complexity-pruned Gini and Entropy trees. |
| 8–9. Model choice | Rank six finalists by training-fold average precision, corroborate with ROC-AUC, and inspect the winner's out-of-fold probability scores. |
| 10. Decision threshold | Apply the pre-declared 0.20 threshold; examine assumption sensitivity and compare all six finalists at that threshold. |
| 11. Evaluation and refit | Evaluate the chosen training model on the 20% test partition, then refit its selected specification on all 12,330 sessions. |
| 12. Interpretation | Summarise results and limitations. |

**Data handling.** There are no missing values or blank categorical markers. The notebook retains the source rows, including identical full-row records, and performs exploratory work on the training partition. The split contains **9,864 training** and **2,466 test** observations. `PageValues` is included in the initial diagnostic comparison but excluded from operational models because its availability at the intended prediction time is uncertain; its strong association with purchases makes the timing issue material.

**Modelling design.** Four classifier families are compared: Logistic Regression, linear discriminant analysis (LDA), Gaussian Naive Bayes and Bernoulli Naive Bayes, alongside Gini and Entropy classification trees. Classifier presentation tables contain **L1–L7, D1–D7, G1–G7 and B1–B7** (28 displayed specifications). The underlying sequential evaluation accounts for **43 planned candidate checks, 38 scored and five inapplicable**. Model transformations and category handling are fitted inside training folds. Five-fold cross-validation and a sequential **one-standard-error complexity rule** guide the within-family choices.

## Results

Because only approximately 15.5% of sessions end in purchases, **mean five-fold average precision (AP / PR-AUC)** is the primary selection metric; ROC-AUC provides a secondary ranking check.

| Finalist | Mean five-fold AP | Mean five-fold ROC-AUC |
|---|---:|---:|
| **L6 Logistic Regression — selected** | **0.34207** | **0.75096** |
| Pruned Entropy tree | 0.32105 | 0.74397 |
| D7 LDA | 0.31852 | 0.73028 |
| Pruned Gini tree | 0.31748 | 0.73603 |
| B7 Bernoulli Naive Bayes | 0.31406 | 0.74402 |
| G7 Gaussian Naive Bayes | 0.25332 | 0.70276 |

Pruning improved tree AP substantially: **Gini 0.19015 → 0.31748** and **Entropy 0.19288 → 0.32105**. The chosen L6 model has the highest mean training-fold AP and ROC-AUC of the six finalists.

### Hypothetical contact decision

With an assumed **S$3** contact cost, **S$50** incremental gross margin and **30%** upsell success conditional on a buyer, the decision-cost setup gives `C_FP = 3`, `C_FN = 12` and a pre-declared probability threshold of **0.20**. At that threshold, the winner's training out-of-fold sensitivity is **0.5714**, specificity **0.7571** and precision **0.3010**. The pruned Entropy tree gives a **S$318 lower hypothetical relative loss** than L6 on the same training out-of-fold comparison, but L6 remains selected because AP is the primary criterion.

### Test-partition evaluation and final fit

Applying L6 to the 2,466-session 20% test partition gives **AP 0.32534** and **ROC-AUC 0.73804**. At the 0.20 threshold, sensitivity is **0.5916**, specificity **0.7490** and precision **0.3017**. The final step fits the unchanged selected L6 recipe on the full **12,330-session** dataset.

**Interpretation limits.** This is a predictive rather than causal study. The decision-cost parameters are hypothetical, not measured intervention effects. Model rankings and test-partition results do not establish calibrated probabilities, causal uplift, deployable profitability or performance on future browsing sessions.

## Reproduce the analysis

Use Python **3.12** from the repository root:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

Open `online_shoppers_analysis.ipynb` and run the cells from top to bottom. The notebook reads `data/online_shoppers.csv` directly and renders its own figures and tables. The GitHub Actions workflow independently executes and checks the notebook using the pinned environment; on `analysis/**` branches it can also commit the executed notebook outputs for review.
