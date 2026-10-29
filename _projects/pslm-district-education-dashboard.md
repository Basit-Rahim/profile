---
title: "Out-of-School Children in Pakistan: A District ML Dashboard"
short_title: "PSLM District Education Dashboard"
seo_title: "Machine Learning Analysis of Out-of-School Children in Pakistan (PSLM 2019-20)"
description: "An interactive Streamlit dashboard and reproducible machine learning pipeline that explains school attendance for 30,000+ children across nine districts of Pakistan using PSLM 2019-20, with leakage-audited features, household-grouped cross-validation, SHAP explanations and formal significance tests."
tagline: "Which factors keep children aged 5–16 out of school, and do they differ between Lahore and Kohistan? A leakage-audited ML study of nine districts, delivered as an interactive dashboard for policymakers."
summary: "Leakage-audited ML pipeline and Streamlit dashboard explaining school attendance for 30,592 children in nine districts (PSLM 2019-20), with grouped CV, SHAP and significance tests."
order: 2
year: 2026
role: "Lead ML researcher & developer"
domain: "Machine learning · Education policy"
icon: "fa-bar-chart"
github: "https://github.com/Basit-Rahim/district_analysis"
image: "/img/projects/pslm-district-education-dashboard/hero.jpg"
image_alt: "Dashboard for Lahore showing 7,956 children, 3,524 households, 83.6% attendance and Random Forest AUC 0.830, with key findings about data leakage"
caption: "The Lahore view, running locally. Headline numbers sit above automatically generated key points, including how much data leakage had inflated the original model's AUC (1.000 → 0.830 after fixes)."
languages: [Python]
tags: [Machine Learning, scikit-learn, SHAP, Random Forest, Neural Networks, Logistic Regression, Streamlit, Plotly, statsmodels, Survey data, Education policy]
keywords: [out of school children Pakistan, PSLM 2019-20 analysis, school attendance machine learning, SHAP education, data leakage machine learning, Streamlit dashboard]
metrics:
  - { value: "30,592", label: "children aged 5–16 in 11,992 households" }
  - { value: "9", label: "districts, from Islamabad to Dera Bugti" }
  - { value: "0.77–0.92", label: "honest Random Forest AUC range across districts" }
  - { value: "25", label: "held-out test sets per model (5-fold × 5 repeats)" }
gallery:
  - file: shap.jpg
    alt: "SHAP bar chart and beeswarm plot ranking predictors of school attendance for Lahore"
    title: "What drives attendance? SHAP explanations"
    text: "Mean absolute SHAP values and a beeswarm plot rank predictors such as the child's age, the most-educated adult's schooling, household assets and child labour. Each child is explained by the model that never saw them in training."
  - file: model-performance.jpg
    alt: "Table of significance tests between models plus AUC box plots and ROC curves for logistic regression, random forest and neural network"
    title: "Is one model really better? Proper significance tests"
    text: "Every model is scored on the same 25 test sets. Differences are tested with the Nadeau–Bengio corrected resampled t-test and Holm adjustment, rather than by eyeballing a single split."
  - file: district-comparison.jpg
    alt: "Forest plot of AUC with 95% confidence intervals for three models across nine districts, followed by a summary table"
    title: "Nine districts, one honest comparison"
    text: "AUC with 95% confidence intervals for all three models in every district, plus attendance by gender. In Kohistan, for example, 67% of boys attend school compared with 10% of girls."
---

## The problem

Pakistan has one of the world's largest populations of out-of-school children, and the reasons differ sharply between a Lahore neighbourhood and a village in Tharparkar. Policymakers need district-level evidence about **which household and child characteristics predict non-attendance**, and they need numbers they can trust.

The first version of this analysis reported near-perfect predictive accuracy. An audit showed that accuracy came from **data leakage**, not insight, so I rebuilt the pipeline from scratch.

## What the audit found, and how I fixed it

| Issue | Fix |
|---|---|
| **Outcome leakage.** Several features exist only *because* a child is in school, such as literacy, schooling costs and distance to school. | Removed. They're kept only in an ablation that measures how much they inflated AUC. |
| **Non-independent test sets.** Siblings were split across train and test. | `StratifiedGroupKFold` on household, 5 folds × 5 repeats, with corrected 95% CIs. |
| **Broken income variable.** It was one child's own earnings and about 98% missing. | Summed across all members and sources, distinguishing true zero from missing. |
| **Wrong sample.** Non-household members were coded as "not attending". | Restricted to usual household members. |

In Lahore, Random Forest AUC drops from **1.000** with the original specification to **0.830** after these fixes, and sibling dependence alone inflated it by 0.046. Only the corrected figure should be reported, and the dashboard says so explicitly.

## The pipeline

1. **Dataset construction** from the raw PSLM Stata modules into a child-level analytic file with a documented feature dictionary.
2. **Three model families:** logistic regression as the interpretable baseline, Random Forest as the main model, and a feed-forward neural network (64-32 MLP).
3. **Nested, household-grouped cross-validation**, with hyperparameters tuned in an inner grouped CV so test households never influence tuning.
4. **Inference, not just prediction:** SHAP and permutation importance, cluster-robust logistic regression, the corrected resampled t-test between models, and Holm and Benjamini–Hochberg corrections across predictors.
5. **Delivery:** a Streamlit and Plotly dashboard that reads only about 20 MB of precomputed results, so it deploys anywhere without the raw survey data.

## Key results

- Random Forest has the highest AUC in **9 of 9 districts** and is significantly better than logistic regression in **8**.
- Corrected AUC ranges from **0.77 to 0.92** (median 0.83). With the original leaky specification it was 1.00 everywhere.
- The most consistent significant predictors are **child age**, **child labour in the last month**, **being female** and **household assets**.
- The largest gender gap is in **Kohistan**, where 67% of boys attend school compared with 10% of girls.

> **Running it locally:** I created a fresh virtual environment, installed the pinned dashboard requirements and launched `streamlit run app.py`. Every screenshot here comes from that local session.
