# Predicting Hospital Length of Stay (LOS) with Explainable Machine Learning  
**MIMIC-III Demo-Scale Case Study**

## Overview

This project explores hospital Length of Stay (LOS) prediction using real-world clinical data from the MIMIC-III database.

Instead of chasing artificial performance gains, the focus is on:

- honest evaluation under weak signal  
- dual modeling formulations (regression + classification)  
- transparent, model-native explainability  
- clinical and operational plausibility  

All experiments were conducted on a **reduced demo-scale subset** of MIMIC-III for feasibility and rapid iteration.  
The goal is methodological validation and interpretability analysis — not production-grade forecasting.

---

## Problem Framing

LOS is a key operational and clinical metric that affects:

- bed allocation and capacity planning  
- staffing and ICU load  
- healthcare costs  
- patient safety and complication risk  

This project approaches LOS prediction from two complementary perspectives:

### 1) Regression — Predict LOS in Days  
**Question:**  
*How long is this patient likely to stay in the hospital?*

Models:
- Linear Regression  
- Random Forest Regressor  
- Gradient Boosting Regressor  

Use case:
- quantitative forecasting  
- workload estimation  
- discharge planning  

---

### 2) Classification — Predict LOS Risk Group  
**Question:**  
*Is this patient likely to require an unusually long hospital stay?*

LOS groups:
- very short  
- short  
- medium  
- long  

Models:
- Logistic Regression  
- Random Forest Classifier  

Use case:
- early triage  
- operational risk stratification  
- interpretable decision support  

---

## Dataset

**Source:** MIMIC-III (public ICU dataset)  
**Scope:** Adult ICU admissions (demo-scale subset)

**Features include:**
- demographics (age, gender)  
- admission metadata (type, location, insurance)  
- ICU unit assignment  
- number of diagnoses  
- early drug burden (first 48h)  
- number of hospital services  

**Targets:**
- `los_days` — continuous LOS (for regression)  
- `los_group` — categorical LOS group (for classification)

⚠️ This project uses de-identified clinical data for educational and research purposes only.

---

## Project Structure


