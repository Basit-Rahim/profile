---
title: "GIQ: Modelling Household School-Enrolment Decisions"
short_title: "GIQ School-Enrolment Model"
seo_title: "GIQ: Neural Network and Ensemble Model of School Enrolment in Pakistan"
description: "A two-stage machine learning model of household school-enrolment decisions in Lahore, Pakistan. Neural networks predict parental perceptions, an autoencoder compresses them, and a stacked XGBoost/Random Forest ensemble predicts enrolment, validated out of sample on 3,512 PSLM 2019-20 households with fully reproducible code."
tagline: "Can a model trained on 356 surveyed households predict school enrolment across an entire city? A two-stage neural and ensemble model, externally validated on the national PSLM survey, with every number in the paper reproducible from one command."
summary: "Two-stage model (neural nets → autoencoder → stacked XGBoost/RF ensemble) of household enrolment decisions, externally validated on 3,512 PSLM households; fully reproducible."
order: 3
year: 2026
role: "Researcher & ML engineer"
domain: "Computational social science"
icon: "fa-sitemap"
github: "https://github.com/Basit-Rahim/GIQ"
image: "/img/projects/giq-school-enrolment-model/hero.jpg"
image_alt: "Bar charts comparing observed PSLM enrolment of 83.8% with the model's estimate of 86.7%, and model versus actual enrolment by income quintile and household-head literacy"
caption: "External validation on PSLM 2019-20 (Lahore), regenerated from the repository's outputs after I re-ran the pipeline locally. The model estimates 86.7% enrolment against 83.8% observed, and it reproduces the real income gradient."
languages: [Python]
tags: [Neural Networks, Autoencoder, XGBoost, Random Forest, Stacked ensemble, scikit-learn, Survey research, External validation, Reproducible research, pytest]
keywords: [school enrolment model Pakistan, household survey machine learning, stacked ensemble XGBoost, autoencoder latent factors, PSLM validation, reproducible research code]
metrics:
  - { value: "356", label: "surveyed households (706 children) for training" }
  - { value: "3,512", label: "independent PSLM households for out-of-sample testing" }
  - { value: "+2.9 pts", label: "model vs observed city-wide enrolment (86.7% vs 83.8%)" }
  - { value: "0.67", label: "household-level AUC on the national survey" }
gallery:
  - file: architecture.jpg
    alt: "Diagram of the GIQ pipeline: household inputs, five Stage 1 neural networks, an autoencoder, a Stage 2 stacked ensemble, a route-safety adjustment and the enrolment output"
    title: "Two-stage architecture"
    text: "Stage 1 models how parents perceive cost, distance, safety, facilities and stipends. An autoencoder compresses those perceptions into three latent dimensions. Stage 2, a stacked ensemble, turns them into the share of the household's children enrolled."
---

## The question

Why do some households in Pakistan send all their children to school while others don't? Economic models often treat enrolment as a direct function of income and distance. Parents, however, decide based on **how they perceive** cost, safety, school facilities and the value of stipends.

GIQ models that decision explicitly. It first predicts parental perceptions from observable household facts, then predicts enrolment from those perceptions.

## The model

```
household inputs (income, distance, journey time)
  → Stage 1     five neural networks, one per parental perception
  → autoencoder five perception scores → three latent dimensions
  → Stage 2     stacked ensemble (XGBoost, Random Forest, linear; linear meta-learner)
                → share of school-age children enrolled
  → route safety: probability × 0.8 if the route to school is unsafe (stated assumption)
```

The training data is an anonymised field survey of **356 urban households with 706 school-age children**. It contains no names, identifiers, coordinates or place names.

## Validating it honestly

Within the survey, 98.7% of children were enrolled and only two households had fewer than half their children in school. With so few negatives, no within-sample metric can say much, and the repository reports exactly that, comparing every model against a mean baseline.

The real test is **external**. I applied the trained model to an independent dataset, the **PSLM 2019-20** national survey, covering **3,512 Lahore households and 7,929 children**:

| | Enrolment rate |
|---|---|
| PSLM observed | **83.8%** |
| Model, routes 50% unsafe (reported) | **86.7%** |
| Model, all routes safe / all unsafe | 96.6% / 76.7% |

The model lands **within three percentage points** of the observed city-wide rate, reaches a household-level **AUC of 0.67** on data it never saw, and reproduces the real gradients by income quintile and by household-head literacy and numeracy. It is also clear about its limits: distance was recorded only for enrolled children, so the model **can't** estimate a deterrent effect of distance, and the README says so.

## Reproducibility as a feature

Every number cited in the paper maps to a script and an output file:

```bash
python scripts/train_model.py     # cross-validation + final model
python scripts/build_pslm.py      # PSLM download → Lahore household table
python scripts/validate_pslm.py   # out-of-sample validation
python scripts/summarise.py       # every number cited in the paper
python -m pytest tests -q
```

Seeds are fixed in one config file, so reruns produce identical outputs. I checked this by cloning the repo into a fresh Python 3.12 virtual environment, retraining the model and running the test suite (**4/4 passing**). The regenerated cross-validation metrics were byte-for-byte identical to the committed ones. The PSLM steps need the raw survey download from the Pakistan Bureau of Statistics.
