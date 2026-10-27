# 27 Oct Meeting

**Course:** DSA211 Regression and Classification  
**Project:** Topic 2 — Online shoppers' purchasing behaviour prediction  
**Date:** 27 October 2026  
**Source:** Group Zoom meeting transcript (approximately 84 minutes)

## 1. Meeting objective

Align the group's exploratory data analysis (EDA), baseline and revised models, validation procedure, model-selection criteria, classification threshold, and presentation format.

## 2. Discussion and conclusions

| Topic | Discussion and conclusion | Status |
| --- | --- | --- |
| Project objective | Focus on **prediction**, not statistical inference. Assess models primarily on predictive performance. | **Agreed** |
| EDA | Cover both **numerical and categorical** variables. Use the findings to justify transformations and revisions. Retain plots that support downstream decisions. | **Agreed** |
| Baseline models | Start with **all 17 original predictors** without substantive feature transformations. Include the three required classification families: logistic regression, LDA and Naive Bayes. | **Agreed** |
| Data preprocessing | Consider log transformations for skewed numeric variables, binary indicators for zero-heavy numeric measures, and one-hot encoding for nominal categorical predictors. Categorical one-hot encoding and zero/non-zero binarisation address different problems. | **Agreed in principle** |
| Revised models | Two approaches were discussed: (a) separately evaluate many variants (about 14 in one notebook), or (b) compare fewer consolidated revisions (about 7 in another notebook) incorporating several EDA-supported changes. | **Open — professor clarification needed** |
| Regularisation | Discussed Ridge (L2), LASSO (L1) and Elastic Net from Lab 9. The group favoured considering relevant taught techniques, without exhaustively testing every combination. | **Provisional** |
| Validation workflow | Reserve an untouched **20% test set**. Use **five-fold cross-validation on the 80% training split** to compare candidates, select a winner, refit it on all training observations, then score it once on the held-out test set. | **Agreed** |
| Evaluation metrics | Discussed mean cross-validated cost, ROC-AUC and PR-AUC / average precision. Given class imbalance, the group leaned towards PR-AUC as important evidence, with ROC-AUC as a secondary metric. | **Provisional — primary selection rule unresolved** |
| Threshold and business costs | Considered the class-prevalence threshold, sensitivity–specificity crossover, 0.5, and a business-cost-derived threshold. The lack of reliable cost data makes the business justification difficult. | **Open — professor clarification needed** |
| Presentation | The group queried whether slides are necessary or the Jupyter notebook can be presented directly. A 9 am slot was discussed but was not clearly confirmed in the recording. | **Open — professor clarification needed** |

### Clarified workflow

1. Set aside a stratified 20% test partition and do not use it for model selection.
2. Conduct EDA and establish baseline models on the training data.
3. Propose revised candidates with EDA-based reasoning.
4. Compare candidates using five-fold cross-validation on the remaining 80%, applying preprocessing within the training folds where appropriate.
5. Select the winning model using a pre-specified evaluation criterion.
6. Refit the selected model on the full 80% training set.
7. Evaluate **only the selected model** on the held-out 20% test set once.
8. If needed for deployment, a subsequent refit on all available observations is separate from the independent held-out evaluation.

## 3. Open issues for the professor

### 3.1 Model revisions and reporting

**Question:** After building the full-predictor baselines, should each change (e.g. log transformation, binarisation or regularisation) be compared separately, or may several EDA-supported changes be combined into a single revised model? Is it necessary to test/report every permutation, or is a smaller justified set sufficient?

**Why it matters:** The existing notebooks use different numbers of candidate models and different ways of grouping revisions.

**Transcript:** 29:48–31:45; 1:14:49–1:15:25.

### 3.2 Business costs and choice of classification threshold

**Question:** In the absence of reliable company-level costs, may the group specify transparent hypothetical false-positive and false-negative costs, derive a cost-based classification threshold and analyse sensitivity to those assumptions? Alternatively, is a statistically motivated threshold (such as class prevalence or the sensitivity–specificity crossover) acceptable if the business trade-off is explained qualitatively?

**Related metric question:** Can the group use PR-AUC as the primary CV ranking metric, with ROC-AUC as supporting evidence, while addressing relative false-positive/false-negative costs separately? Or should the primary selection rule be an explicit cost-based metric?

**Why it matters:** The project brief asks for a final model justified by both predictive performance and relative classification-error costs, but the team has not established defensible numerical costs.

**Transcript:** 1:06:05–1:14:41; 1:21:00–1:21:45.

### 3.3 Presentation requirements

**Question:** Must the group prepare presentation slides, or can it present the project directly from the Jupyter notebook?

**Transcript:** 1:24:07–1:24:25.

## 4. Internal issue to resolve after clarification

**One common model-selection criterion.** One notebook selected the lowest **mean CV cost**, while another focused on **CV ROC-AUC**, and the group discussed **PR-AUC** for the imbalanced target. The group needs a single declared primary criterion, supported by secondary diagnostic metrics, and a consistent threshold strategy. This was discussed but **not finally decided** during the meeting.

## 5. Action items

| Action | Responsibility | Status |
| --- | --- | --- |
| Combine the existing numerical and categorical EDA into a coherent section. | Group | To do |
| Align full-predictor baseline models and comparable results tables. | Group | To do |
| Clarify whether to test individual changes or consolidated revised models. | Ask professor | Open |
| Clarify hypothetical business costs, threshold choice and the primary selection metric. | Ask professor | Open |
| Confirm whether slides are required and verify the presentation slot. | Ask professor / group | Open |
| Agree on the final validation and model-selection workflow, then consolidate notebooks. | Group | After clarification |

---

*Minutes summarise the group's discussion and tentative agreements. They do not establish that unconfirmed choices are required by the professor. The Zoom transcript attributed speech to one recording account, so individual statements and action owners have not been assigned.*
