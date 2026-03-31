#  Prediction of Renal Recovery Trajectories in Vasopressin-Treated ICU Patients Using Machine Learning

##  Project Overview

Acute Kidney Injury (AKI) is a common and serious complication among critically ill ICU patients, particularly those receiving vasopressin for hemodynamic support. However, renal recovery trajectories vary significantly, and early prediction remains challenging.

This project develops an interpretable machine-learning pipeline to predict short-term renal recovery versus worsening in vasopressin-treated AKI patients using early ICU admission data from the **MIMIC-IV v3.1** database.

The goal is to enable early risk stratification using routinely collected clinical data while strictly preventing temporal data leakage.

---

##  Research Objective

 Can early ICU renal and hemodynamic data predict short-term renal recovery versus worsening in vasopressin-treated AKI patients?

---

##  Dataset

- **Source:** MIMIC-IV v3.1 (PhysioNet)
- **Size:** >40 GB
- **Modules Used:**
  - `hosp/` (labevents, admissions, patients)
  - `icu/` (icustays, chartevents, outputevents, inputevents)

### Cohort Construction

- Adult ICU patients (>18 years)
- AKI identified using KDIGO creatinine criteria
- Restricted to vasopressin-treated ICU stays
- Minimum two creatinine measurements required

### Final Balanced Dataset

| Class | Count |
|---|---|
| Improving / Recovered | 1,386 |
| Worsening | 1,386 |
| **Total** | **2,772 ICU stays** |

---

## Methodology Pipeline

### 1️ Data Extraction

- Chunk-based CSV processing using `pandas.read_csv(chunksize=...)`
- Linked using: `subject_id`, `hadm_id`, `stay_id`

### 2️ Feature Engineering

**Early ICU (0–24h) predictors:**

| Feature | Description |
|---|---|
| Mean Creatinine | Average creatinine in first 24h |
| First Creatinine | Admission creatinine value |
| Mean Urine Output | Average urine output in first 24h |
| Mean Arterial Pressure (MAP) | Hemodynamic measure |
| Systolic Blood Pressure (SBP) | Hemodynamic measure |
| Mean Lactate | Marker of tissue perfusion |
| Age | Patient age at admission |

**Outcome:** KDIGO creatinine change–based recovery label. Improving + Recovered classes merged for binary classification. Strict temporal separation enforced to prevent leakage.

### 3️ Exploratory Data Analysis

- Class distribution plots
- Boxplots and scatterplots
- KDE distribution plots
- Pearson correlation heatmaps
- Mann–Whitney U tests
- Welch's independent t-tests

### 4️ Preprocessing

- Removal of outcome-derived variables
- Downsampling for class balance
- Z-score standardization (training set only)

### 5️ Machine Learning Models

**Models evaluated:
- Logistic Regression
- Random Forest
- Gradient Boosting
- XGBoost

**Train-test split:** 80/20 stratified split

**Evaluation metrics:** Accuracy, Precision, Recall, F1-score, ROC-AUC, Confusion Matrix

---

##  Results

| Model | Accuracy | ROC-AUC |
|---|---|---|
| Logistic Regression | 0.798 | **0.870** |
| Random Forest | 0.787 | 0.841 |
| Gradient Boosting | 0.802 | 0.864 |

###  Best Model: Logistic Regression

- Highest ROC-AUC (0.870)
- Balanced recall and precision
- Clinically interpretable coefficients

---

##  Key Findings

- Worsening patients showed higher creatinine, lower urine output, higher lactate, and lower blood pressure compared to recovering patients.
- Early ICU features meaningfully predict short-term renal trajectory.
- Simple interpretable models performed comparably to complex tree-based models.

---

##  Technologies Used

- Python 3.x
- Pandas
- NumPy
- SciPy
- Scikit-learn
- Matplotlib
- Seaborn

---

##  Limitations

- Retrospective, single-center dataset
- Creatinine-based recovery definition only
- No external validation cohort
- Downsampling may reduce generalizability

---

##  Future Work

- External validation on other ICU datasets
- Time-series modeling (LSTM, survival models)
- SHAP-based interpretability analysis
- Integration into ICU clinical decision-support workflow

---

##  References

- Johnson et al., MIMIC-IV Database (PhysioNet)
- KDIGO Clinical Practice Guidelines for AKI
- Relevant AKI trajectory modeling literature

---

##  Authors

**Aiswarya Perumbilly · Nicholas Carlson · Sathvika Neeruddula**

*Indiana University Indianapolis*
