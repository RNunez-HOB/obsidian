---
tags:
  - project
  - project_titanic
  - Hobby
status: Ongoing
started: 2026-04-24
completed:
---

# Project — Titanic Survival Classifier

> A small worked example showing how a project note links back to subjects. Delete or replace when you create your own.

## Goal

Predict passenger survival on the Titanic dataset as a hands-on refresher of the classic supervised-learning workflow.

## Dataset

- **Source:** Kaggle Titanic
- **Size / shape:** 891 rows x 12 cols
- **Notes:** Missing Age values filled with median by title.

## Approach

1. EDA and handling of missing values.
2. Feature engineering (title extraction, family size, fare buckets).
3. Baseline logistic regression, then Random Forest and XGBoost.
4. 5-fold cross-validation; hyperparameter search with Optuna.

## Key Results

- Randomforest: ~79%

## What I Learned

- Importance of stratified CV for imbalanced targets.
- Title extraction as a strong proxy for age + class.

## Next Steps

- Try a stacked ensemble.
- Write up on my blog.

## Links

- Code:
- Write-up:
- Demo:

# Related Subjects
- [[Kaggle Competitions]]
- 