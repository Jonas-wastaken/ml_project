# Breast Cancer Classification University Project

## Overview

This project addresses the binary classification of breast cancer using clinical features derived from cell nuclei images. The primary objective is to maximize recall (minimize false negatives), which is critical in medical diagnostics. The workflow covers data exploration, preprocessing, feature selection, model development, hyperparameter optimization, and systematic comparison of multiple machine learning models.

---

## Task Description

The task is to develop robust machine learning models that can accurately classify breast cancer cases as positive or negative based on a set of engineered features. The dataset is sourced from the [UCI Breast Cancer Wisconsin Diagnostic Dataset](https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic).

**Key requirements:**

- **Maximize recall** to minimize false negatives.
- Systematically compare preprocessing, feature selection, and modeling strategies.
- Track all experiments for reproducibility.

---

## Project Structure

```text
├── README.md
├── requirements.txt
├── artifacts/
│   ├── models/
│   ├── notebooks/
│   └── preprocessors/
├── data/
│   ├── raw_data.csv
│   └── processed_data/
├── mlruns/
├── notebooks/
│   ├── exploration.ipynb
│   ├── logistic_regression.ipynb
│   ├── neural_net.ipynb
│   ├── preprocessing.ipynb
└── └── svc.ipynb
```

- **notebooks/**: Jupyter notebooks for data exploration, preprocessing, and modeling.
- **data/**: Raw and processed datasets.
- **artifacts/**: Saved models, preprocessors, and PDF exports of notebooks.
- **mlruns/**: MLflow tracking directory.
- **requirements.txt**: Python dependencies.

---

## Setup

1. **Install dependencies**  
    Create a virtual environment and install requirements:

    ```sh
    python -m venv .venv
    source .venv/bin/activate
    pip install -r requirements.txt
    ```

2. **Data**  
    Ensure `data/raw_data.csv` is present. Processed datasets will be generated by running the preprocessing notebook.

3. **MLflow Tracking**  
    MLflow is used for experiment tracking. The default tracking URI is set to the `mlruns/` directory.

---

## Workflow & Findings

### 1. Data Exploration (`notebooks/exploration.ipynb`)

- **Integrity:** No missing values, negligible duplicates.
- **Feature Analysis:** Most features are right-skewed with outliers. Size-related features (`radius2`, `perimeter2`, `area2`) show strong predictive power.
- **Feature Selection:** `compactness2` is excluded due to redundancy.

### 2. Preprocessing & Feature Selection (`notebooks/preprocessing.ipynb`)

- **Scaling Methods:** `StandardScaler`, `MinMaxScaler`, `PowerTransformer`, `QuantileTransformer`.
- **Feature Selection:** Mutual Information (MI) and Sequential Feature Selection (SFS) are compared.
- **Best Pipeline:** `PowerTransformer + SFS` and `QuantileTransformer + MI` yield the best feature sets for modeling.

### 3. Model Development & Optimization

#### Logistic Regression (`notebooks/logistic_regression.ipynb`)

- **Best Preprocessing:** `PowerTransformer + SFS`
- **Hyperparameter Tuning:** Three-stage search (random, grid, exhaustive)
- **Best Model:**
  - Recall: 0.96 (test)
  - ROC AUC: 0.97 (test)
  - F1: 0.83 (test)

#### Support Vector Classifier (`notebooks/svc.ipynb`)

- **Best Preprocessing:** `QuantileTransformer + MI`
- **Hyperparameter Tuning:** Focused on `C` and `kernel`
- **Best Model:**
  - Recall: 0.93 (test)
  - ROC AUC: 0.96 (test)
  - F1: 0.89 (test)

#### Neural Network (`notebooks/neural_net.ipynb`)

- **Best Preprocessing:** `PowerTransformer + SFS`
- **Architecture:** Compact FCNN (e.g., `[16, 8]` or `[8, 6, 4]` hidden units, minimal dropout)
- **Best Model:**
  - Recall: 0.95 (test, best run)
  - ROC AUC: 0.90 (test)
  - F1: 0.66 (test)
- **Notes:** Simpler architectures generalize better; overfitting is a concern with deeper/wider networks.

---

## Model Comparison

| Model                   | Preprocessing      | Recall | ROC AUC | F1   | Precision | Accuracy |
|-------------------------|-------------------|--------|---------|------|-----------|----------|
| Logistic Regression     | PT + SFS          | 0.96   | 0.97    | 0.83 | 0.74      | 0.86     |
| SVC                     | QT + MI           | 0.93   | 0.96    | 0.89 | 0.86      | 0.92     |
| Neural Network (MLP)    | PT + SFS          | 0.95   | 0.90    | 0.66 | 0.81      | 0.76     |

- **Logistic Regression:** Highest recall and ROC AUC, robust and interpretable.
- **SVC:** Strong recall and F1, best overall accuracy, robust to class imbalance.
- **Neural Network:** High recall possible, but more prone to overfitting and less stable on small datasets.

---

## Recommendations

- For deployment or clinical use, **Logistic Regression** or **SVC** with the specified preprocessing pipelines are recommended due to their high recall, interpretability, and robustness.
- Neural Networks may be considered if more data becomes available, but require careful regularization.

---

## Reproducibility

- All experiments are tracked with MLflow (`mlruns/`).
- Best models and preprocessors are saved in `artifacts/`.
- To reproduce results, run the notebooks in order:
  1. `exploration.ipynb`
  2. `preprocessing.ipynb`
  3. Model notebooks (`logistic_regression.ipynb`, `svc.ipynb`, `neural_net.ipynb`)

---

## References

- [UCI Breast Cancer Wisconsin Diagnostic Dataset](https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic)
