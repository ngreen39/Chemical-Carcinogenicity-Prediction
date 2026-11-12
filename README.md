# Chemical Carcinogenicity Prediction

A machine learning benchmark comparing supervised classification models to determine which algorithm most accurately predicts known or suspected carcinogenic chemicals using experimental outcomes from the Carcinogenic Potency Database (CPDB).

---

## Project Purpose & Research Question

> **Research Question:** How accurately can supervised machine learning algorithms classify chemical compounds as carcinogenic or non-carcinogenic based on experimental exposure parameters (dose, route, duration, species, strain, and sex) and molecular descriptors, using the author’s reported carcinogenic opinions from the CPDB as target variables?

### Key Sub-Questions
- Which model achieves the highest predictive accuracy, recall (sensitivity), and ROC-AUC on unseen chemicals?
- Which dose parameters and exposure conditions are the strongest predictors of a chemical's carcinogenicity?
- How does model accuracy vary across different animal species, exposure routes, and tissue-specific vs. author-labeled outcomes?

---

## Dataset & Pipeline
- **Data:** [Carcinogenic Potency Database (CPDB) Archived Literature Data](https://datadiscovery.nlm.nih.gov/Literature/Carcinogenic-Potency-Database-CPDB-Archived-Data/s5hf-hv94/about_data) (`cpdb.lit.xls`, `cpdb.chemname.xls`, `cpdb.litdose.xls`)
- **Target Variable (`y`):** Binary classification derived from CPDB author opinions at the experiment level (`1` = carcinogenic if any tissue row has a `+` opinion; `0` = non-carcinogenic if only `-` opinions; `0`-only opinions excluded or tested in sensitivity analysis).
- **Input Features (`X`):**
  - *Experimental Parameters:* Log maximum dose, number of dose groups, dosing duration, experiment duration, route, species, sex, strain, and mutagenicity status.
  - *Molecular Descriptors (via PubChem REST API):* Molecular weight, XLogP, polar surface area, hydrogen-bond donor/acceptor counts, rotatable bonds, and heavy atom count matched via CAS number.
- **Train/Test Split:** Grouped by chemical ID so no chemical appears in both training and test sets, ensuring models are evaluated on genuinely unseen compounds.

---

## Models Evaluated
All models are trained and tested on the exact same chemical-grouped split (`X_train`, `X_test`, `y_train`, `y_test`) and compared using **ROC-AUC** (target ≥ 0.80), **Recall**, **Precision**, and **Accuracy**:

| Model | Purpose |
| :--- | :--- |
| **??** | Benchmark floor (always predicts the most common class) |
| **Logistic Regression** | Linear baseline for binary classification |
| **K-Nearest Neighbors (KNN)** | Distance-based classification across chemical and dose features |
| **Decision Tree** | Interpretable rule-based model to inspect feature splits |
| **Random Forest** | Ensemble tree model compared directly against the single Decision Tree |

---