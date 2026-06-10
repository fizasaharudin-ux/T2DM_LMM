# T2DM_LMM Practice

# Glycaemic Trajectories in Type 2 Diabetes: A Linear Mixed Effects Model of Intervention and Lifestyle Determinants

**Author:** Hafizah  
**Date:** June 2026  
**Tool:** R Markdown → Published via Posit Connect

---

## Overview

This project analyses longitudinal HbA1c trajectories in **20,000 T2DM patients** measured at three time points — pre-intervention (T1_Pre), mid-intervention (T2_Mid), and post-intervention (T3_Post).

The core question: do different intervention strengths (Weak, Standard, Intensive) produce different HbA1c trajectories over time, after adjusting for demographic, lifestyle, and clinical covariates?

A **Linear Mixed Effects Model (LMM)** is used throughout. Each patient contributes three repeated observations, making standard regression inappropriate. A null model ICC of **0.813** confirms 81.3% of HbA1c variance is attributable to stable between-patient differences — LMM is required.

---

## Data

**Source:** Rana, S. *T2DM Longitudinal Treatment Outcomes* \[Dataset\]. Kaggle, 2024.  
[https://www.kaggle.com/datasets/sahilrana034/t2dm-longitudinal-treatment-outcomes](https://www.kaggle.com/datasets/sahilrana034/t2dm-longitudinal-treatment-outcomes)

Synthetic longitudinal cohort. 60,000 rows (20,000 patients × 3 time points). No missing values.

**Outcome:** `hb_a1c` — HbA1c (%), primary measure of glycaemic control.

**Key predictors:** `intervention_strength` (Weak / Standard / Intensive), `time_step` (T1_Pre / T2_Mid / T3_Post), `age`, `gender`, `bmi`, `physical_activity_level`, `diet_score`, `sleep_hours`, `medication_adherence`, `comorbidity_level`.

---

## Analytical workflow

This analysis follows the **REAAAPP** framework:

> **R**ead → **E**DA → **A**ssumptions → **A**nalyse → **A**ssess → **P**resent → **P**ublish

### Models fitted

| Model | Purpose |
|---|---|
| Null (intercept-only) | ICC decomposition — justifies LMM |
| Time-only | Unconditional growth — overall HbA1c trend |
| Full fixed-effects (`lmm_full`) | Adjusted associations for all covariates |
| Interaction (`lmm_int`) | Time × intervention — primary research question |

Model selection used AIC and likelihood ratio tests (ML, not REML, for fixed-effects comparison). `lmm_int` had the lowest AIC (101,119) and all four interaction terms were significant (p < 0.0001).

---

## Key results

**Model fit**

| Metric | Value |
|---|---|
| ICC (null model) | 0.813 |
| R²m — fixed effects only | 0.368 |
| R²c — fixed + random effects | 0.974 |
| AIC (lmm_int, best model) | 101,119 |

**Endpoint comparisons (T3_Post, Bonferroni-corrected)**

| Contrast | Difference | p-value |
|---|---|---|
| Weak vs Standard | −0.080% | 0.004 |
| Weak vs Intensive | −0.202% | < 0.001 |
| Standard vs Intensive | −0.122% | < 0.001 |

All three intervention groups differed significantly from each other at the study endpoint.

**Strongest individual predictors of lower HbA1c** (from `lmm_full`): high physical activity (−0.52%), sleep hours (−0.41%), good medication adherence (−0.36%), diet score (−0.32%). BMI (+0.09%) and high comorbidity (+0.06%) were associated with higher HbA1c.

---

## Repository structure

```
T2DM_LMM/
├── T2DM_LMM.qmd                                   # Main analysis — Quarto Markdown source
├── T2DM-LMM.html                                  # Rendered output
├── t2dm_longitudinal_strong_60000_rows (1).csv    # Excel raw dataset (downloaded from Kaggle - link at acknowledgement)
├── style.css                                      # Custom CSS for HTML output
└── README.md                                      # This file
```

---

## How to reproduce

1. Clone this repository.
2. Download the dataset from Kaggle or from this repository and save as `t2dm_longitudinal_strong_60000_rows (1).csv` in the project root.
3. Open `T2DM_LMM.Rmd` in RStudio.
4. Run — all required packages are installed automatically via `pacman::p_load()`.

**R packages used:** `tidyverse`, `janitor`, `skimr`, `DataExplorer`, `corrplot`, `gtsummary`, `lme4`, `lmerTest`, `performance`, `MuMIn`, `emmeans`, `sjPlot`, `patchwork`, `broom.mixed`, `car`, `kableExtra`.

---

## Published report

The rendered report is published on Posit Connect and available at:

> *(Insert your Posit Connect URL here)*

---

## Acknowledgement

Dataset sourced from Kaggle, originally created for educational and analytical practice purposes.

> Rana, S. *T2DM Longitudinal Treatment Outcomes* \[Dataset\]. Kaggle, 2024. [https://www.kaggle.com/datasets/sahilrana034/t2dm-longitudinal-treatment-outcomes](https://www.kaggle.com/datasets/sahilrana034/t2dm-longitudinal-treatment-outcomes)

