# project overview

## Abstract
This repository implements an applied pipeline for prognostic risk modelling and uplift analysis in a clinical cohort. The workflow includes data ingestion, preprocessing (including iterative imputation and outlier analysis), prognostic model training (CatBoost) and an uplift-style analysis to quantify treatment receptivity.

## Data
- Primary tabular source (example): [data/data_sample.xlsx](data/data_sample.xlsx).  
- Visual summary of principal results: [data/main_result.png](data/main_result.png).

## Preprocessing
- Imputation and outlier handling are implemented in [notebooks/1_preprocess_data.ipynb](notebooks/1_preprocess_data.ipynb). Key routines include:
  - [`mice_impute_data`](notebooks/1_preprocess_data.ipynb) — iterative imputation pipeline (numerical + binary/categorical handling).
  - [`detect_numerical_outliers`](notebooks/1_preprocess_data.ipynb) and [`detect_categorical_outliers`](notebooks/1_preprocess_data.ipynb) — outlier detection utilities.
  - [`check_distribution_shift`](notebooks/1_preprocess_data.ipynb) — KS / chi-squared checks for train/test shifts.
- Processed training/test splits and serialized datasets are saved.

## Model development
- Model training and evaluation are implemented in [notebooks/2_boosting.ipynb](notebooks/2_boosting.ipynb). The fitted prognostic model is persisted as:
  - [models/catboost_subset_genetics_cyp.joblib](models/catboost_subset_genetics_cyp.joblib)
- Notebooks illustrate CatBoost configuration, training, AUC evaluation and SHAP-based interpretation.

## Uplift / treatment effect analysis
- Uplift-style inference and bootstrap-based shift quantification are implemented in [notebooks/2_uplift_modelling.ipynb](notebooks/2_uplift_modelling.ipynb). Relevant symbols:
  - [`scale_predictions`](notebooks/2_uplift_modelling.ipynb) — prediction scaling used for bootstrap comparators.
  - [`calculate_shift_distribution`](notebooks/2_uplift_modelling.ipynb) — routine that computes bootstrap distributions of prediction differences.
  - [`model`](notebooks/2_uplift_modelling.ipynb) — the loaded CatBoost model used for inference.

## Results
- The repository demonstrates:
  - A prognostic risk model with reported AUC (see [notebooks/2_boosting.ipynb](notebooks/2_boosting.ipynb)).
  - Bootstrap distributions of treatment-related prediction shifts and statistical comparison of subgroups (see [notebooks/2_uplift_modelling.ipynb](notebooks/2_uplift_modelling.ipynb)).
  - The principal figure is available at [data/main_result.png](data/main_result.png).

## Reproducibility
- Recommended execution order:
  1. [notebooks/1_preprocess_data.ipynb](notebooks/1_preprocess_data.ipynb) — produce and save processed datasets.
  2. [notebooks/2_boosting.ipynb](notebooks/2_boosting.ipynb) — train/save CatBoost model ([models/catboost_subset_genetics_cyp.joblib](models/catboost_subset_genetics_cyp.joblib)).
  3. [notebooks/2_uplift_modelling.ipynb](notebooks/2_uplift_modelling.ipynb) — load model and perform uplift analysis.
- Environment: the notebooks assume a Python environment with typical data-science packages (pandas, scikit-learn, catboost, joblib, scipy, tqdm, matplotlib).

## Repository contents (selected)
- [notebooks/1_preprocess_data.ipynb](notebooks/1_preprocess_data.ipynb)
- [notebooks/2_boosting.ipynb](notebooks/2_boosting.ipynb)
- [notebooks/2_uplift_modelling.ipynb](notebooks/2_uplift_modelling.ipynb)
- [data/data_sample.xlsx](data/data_sample.xlsx)
- [data/main_result.png](data/main_result.png)
- [models/catboost_subset_genetics_cyp.joblib](models/catboost_subset_genetics_cyp.joblib)

## Contact / citation
- Use this README as the canonical summary when referencing the code or results in academic outputs. For method details, consult the corresponding notebook sections and the in‑notebook function definitions listed