---
title: "Psychometric Test Analysis Dashboard (CTT & IRT)"
short_title: "Psychometric Test Analysis Dashboard"
seo_title: "Psychometric Test Analysis Dashboard: Classical Test Theory and Item Response Theory in Python"
description: "A Streamlit dashboard for analysing student assessment data with Classical Test Theory (Cronbach's alpha, item difficulty, discrimination, alpha-if-deleted) and Item Response Theory (1PL/2PL/3PL, PCM, GPCM, GRM), with marginal maximum likelihood estimation via an EM algorithm implemented from scratch in NumPy and SciPy."
tagline: "Upload a spreadsheet of student responses and get a full psychometric report in seconds: reliability, item quality and IRT item characteristic curves, powered by an EM estimation engine I wrote from scratch."
summary: "Streamlit app for assessment analysis: CTT reliability and item statistics plus IRT (1PL–3PL, PCM, GPCM, GRM) via a from-scratch MML/EM estimator, with ICC plots."
order: 5
year: 2026
role: "Designer & developer"
domain: "Psychometrics · Statistical computing"
icon: "fa-check-square-o"
github: "https://github.com/Basit-Rahim/test-analysis-dashboard"
image: "/img/projects/psychometric-test-analysis-dashboard/hero.jpg"
image_alt: "Item characteristic curves for three polytomous items under the generalized partial credit model, showing the probability of each score category across ability theta"
caption: "Item characteristic curves from a GPCM calibration of 5,000 respondents × 10 items, running locally. Each curve shows the probability of scoring 0–4 points at a given ability level θ."
languages: [Python]
tags: [Psychometrics, Item Response Theory, Classical Test Theory, EM algorithm, Maximum likelihood, NumPy, SciPy, pandas, Plotly, Streamlit, Assessment analytics]
keywords: [item response theory Python, IRT dashboard, classical test theory Cronbach alpha, GPCM GRM partial credit model, EM algorithm marginal maximum likelihood, test item analysis tool]
metrics:
  - { value: "6", label: "IRT models: 1PL, 2PL, 3PL, PCM, GPCM, GRM" }
  - { value: "5,000", label: "respondents calibrated in the sample run" }
  - { value: "54", label: "EM iterations to converge on the sample data" }
  - { value: "α = 0.907", label: "Cronbach's alpha reported for the sample test" }
gallery:
  - file: irt-parameters.jpg
    alt: "Estimation diagnostics showing EM convergence in 54 iterations and a table of estimated discrimination parameters, standard errors and step thresholds for ten items"
    title: "IRT calibration with diagnostics"
    text: "After estimation the app reports convergence (iterations, final log-likelihood) and flags parameters stuck at bounds, non-convergence or sparse score categories. It then lists every item's discrimination, step thresholds and approximate standard errors, with a CSV export."
  - file: ctt-statistics.jpg
    alt: "Whole-test statistics table showing 5,000 respondents, 10 items, mean score 19.5, Cronbach's alpha 0.907 and standard error of measurement 2.74"
    title: "Classical Test Theory report"
    text: "Whole-test statistics (reliability via Cronbach's alpha, standard error of measurement, score distribution), followed by item-level difficulty, upper-lower 27% discrimination, corrected item-total correlations and alpha-if-deleted, so weak items stand out at a glance."
---

## The problem

Education researchers and assessment teams collect thousands of student answer sheets, but turning them into evidence about **test quality** usually means specialist software or hand-written R scripts. Which items are too easy? Which ones fail to separate strong and weak students? Is the test reliable enough to inform decisions?

I built this dashboard so a researcher can answer those questions by uploading a CSV or Excel file. No coding or psychometrics software licence is needed.

## Two frameworks in one tool

**Classical Test Theory (CTT)**

- Whole-test reliability (**Cronbach's alpha**), standard error of measurement and the score distribution
- Per-item **difficulty** (proportion of the maximum score, correct for polytomous items), **discrimination** (upper-lower 27%), corrected item-total correlation and **alpha-if-deleted**
- Respondent total scores, with every table downloadable as CSV

**Item Response Theory (IRT)**

| Item type | Models | Parameters |
|---|---|---|
| Dichotomous (0/1) | 1PL, 2PL, 3PL | difficulty *b*, discrimination *a*, guessing *c* |
| Polytomous (partial credit) | PCM, GPCM, GRM | discrimination *a*, step or ordered thresholds |

The app detects binary and partial-credit items automatically and handles **mixed tests** that contain both.

## The estimation engine

Instead of wrapping an existing IRT package, I implemented **marginal maximum likelihood (MML) estimation with the EM algorithm** directly in NumPy and SciPy:

- **E-step:** compute each respondent's posterior over ability θ at fixed quadrature nodes under an N(0, 1) prior. The prior identifies the θ scale, and ability is never plugged in from raw scores.
- **M-step:** maximise the expected complete-data likelihood item by item, with model-specific category probability functions for 1PL–3PL, GPCM/PCM and GRM. GRM thresholds are reparameterised (softplus increments) so they stay ordered.
- **Diagnostics:** convergence tracking, approximate standard errors from the observed-information diagonal, and warnings for parameters at bounds and for sparse score categories.

On the bundled sample of **5,000 respondents and 10 four-point items**, GPCM calibration converges in **54 EM iterations**.

## Why it matters

The same tooling supports my research at DARE-RC, where assessment data from thousands of students has to be checked for quality before anyone draws conclusions about learning. Good measurement comes first: a policy finding is only as reliable as the test behind it.

> **Running it locally:** I set up a fresh virtual environment, installed the requirements and ran `streamlit run app.py`. I then loaded the bundled `sample_responses.csv` and ran both the CTT report and a GPCM calibration, which produced the screenshots above.
