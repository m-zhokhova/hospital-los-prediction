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
├── 00_data_load.ipynb # cohort construction + feature engineering
├── 01_eda.ipynb # exploratory data analysis
├── 02_modeling.ipynb # regression + classification + explainability
├── README.md
└── requirements.txt


---

## Methodology

### Data Preparation

- merged core MIMIC-III tables  
- built an ICU-level cohort  
- computed LOS in days  
- engineered:
  - disease burden features  
  - early drug burden  
  - care complexity proxies  
- removed leakage features  
- restricted features to early-admission availability  

---

### Exploratory Data Analysis

Main findings:

- LOS is highly right-skewed with a long tail  
- older patients tend to stay longer (descriptively)  
- emergency admissions are overrepresented among long stays  
- higher disease burden and treatment intensity correlate with longer LOS  

These patterns motivated:

- log-transforming LOS for regression  
- reframing LOS as classification for stability  

---

## Modeling Results

### Regression Performance (LOS in days)

| Model                     | MAE   | RMSE  | R²     |
|--------------------------|-------|-------|--------|
| Linear Regression (log)  | 3.06  | 6.00  | 0.039  |
| Gradient Boosting (log)  | 3.73  | 7.57  | -0.53  |
| Random Forest (log)      | 3.07  | 6.07  | 0.017  |

**Interpretation**

- All models fail to explain meaningful LOS variance (R² ≈ 0).  
- Tree ensembles do not outperform a simple linear baseline.  
- Errors remain large relative to typical LOS values.

**Conclusion**

Precise LOS forecasting is not viable using only early static features,  
especially on a demo-scale cohort.

---

### Classification Performance (LOS groups)

| Model                 | Accuracy | Macro F1 | Weighted F1 |
|----------------------|----------|----------|-------------|
| Logistic Regression  | 0.588    | 0.337    | 0.529       |
| Random Forest        | 0.588    | 0.185    | 0.436       |

**Interpretation**

- Accuracy is misleading due to class imbalance.  
- Logistic regression outperforms random forest on Macro F1.  
- Random forest collapses toward the majority class.

**Conclusion**

Logistic regression is the more suitable model here because it:

- generalizes better under weak signal  
- handles imbalance more gracefully  
- remains fully interpretable  
- aligns with the project’s transparency goal  

---

## Explainability

This project avoids black-box explainers and relies on **model-native interpretability**.

### Global Explainability — Coefficients & Odds Ratios

Key drivers of long LOS:

- number of diagnoses (strongest signal)  
- emergency admission  
- transfer from another hospital  
- ICU unit assignment (CCU, MICU, SICU)  
- Medicaid insurance  

These signals reflect **care complexity and care pathways** more than pure demographics.

---

### Baseline Bias — Intercept Analysis

Intercept terms reveal strong structural bias:

| Class       | Logit Score |
|-------------|-------------|
| short       | 1.13        |
| very_short  | 0.49        |
| medium      | -0.54       |
| long        | -1.08       |

Implications:

- the model is inherently biased toward predicting short stays  
- long stays require extreme feature evidence to be predicted  
- this explains low Macro F1 and under-prediction of long LOS  
- reflects both natural LOS imbalance and demo-scale sampling  

---

### Local Explainability — Per-Patient Decomposition

For a single patient, predictions are decomposed into:

- individual feature contributions to log-odds  
- final predicted probability  
- directional push toward or away from long LOS  

This makes each prediction fully traceable to concrete inputs  
without relying on unstable post-hoc explainers.

---

## Key Findings

- LOS is extremely difficult to predict precisely using early static data.  
- Risk stratification is more realistic than exact day forecasting.  
- Disease burden is the most stable and clinically plausible signal.  
- Admission pathway and ICU unit matter more than demographics.  
- Class imbalance structurally shapes model behavior.  
- Interpretability adds more value than marginal accuracy gains.

---

## Limitations

- demo-scale subset limits statistical power  
- no time-series vitals or lab trends  
- no modeling of post-admission events  
- single-center ICU dataset  
- linear effects only (for interpretability)

---

## Ethical Considerations

- not suitable for clinical decision-making  
- potential demographic and socioeconomic bias  
- LOS predictions reflect system behavior, not patient worth  
- intended strictly for educational and analytical use  

---

## Future Work

- scale to full MIMIC-III cohort  
- add time-series vitals and labs  
- model procedures and complications  
- reframe LOS as survival analysis  
- apply class-weighted or focal loss  
- validate on external ICU datasets  
- deploy as a simple decision-support demo  

---

## Reproducibility

```bash
pip install -r requirements.txt
jupyter notebook


