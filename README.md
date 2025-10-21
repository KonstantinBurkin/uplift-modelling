# project overview

## Abstract
This repository implements an applied pipeline for prognostic risk modelling and uplift analysis in a clinical cohort. The workflow includes data ingestion, preprocessing (including iterative imputation and outlier analysis), prognostic model training (CatBoost) and an uplift-style analysis to quantify treatment receptivity.

## Data
- Primary tabular source (example): [data/data_sample.xlsx](data/data_sample.xlsx).  

## Preprocessing
(in `notebooks/1_preprocess_data.ipynb` or `notebooks/2_select_models.ipynb`)
- Imputation and outlier handling. Key routines include:
  - `mice_impute_data` — iterative imputation pipeline (numerical + binary/categorical handling).
  - `detect_numerical_outliers` and `detect_categorical_outliers` — outlier detection utilities.
  - `check_distribution_shift` — KS / chi-squared checks for train/test shifts.
- Processed training/test splits and serialized datasets are saved.

## Model development
- Model training and evaluation are implemented in [notebooks/2_boosting.ipynb](notebooks/2_boosting.ipynb) and [2_select_models](notebooks/2_select_models.ipynb). The fitted prognostic model is persisted as:
  - [models/catboost_subset_genetics_cyp.joblib](models/catboost_subset_genetics_cyp.joblib)
- Notebooks illustrate CatBoost configuration, training, AUC evaluation and SHAP-based interpretation.

## Uplift / treatment effect analysis
- Uplift-style inference and bootstrap-based shift quantification are implemented in [notebooks/2_uplift_modelling.ipynb](notebooks/2_uplift_modelling.ipynb). Relevant symbols:
  - `scale_predictions` — prediction scaling used for bootstrap comparators.
  - `calculate_shift_distribution` — routine that computes bootstrap distributions of prediction differences.
  - `model` — the loaded CatBoost model used for inference.

## Results
- The repository demonstrates:
  - A prognostic risk model with reported AUC.
  - Bootstrap distributions of treatment-related prediction shifts and statistical comparison of subgroups.

## Reproducibility
- Recommended execution order:
  1. [notebooks/1_preprocess_data.ipynb](notebooks/1_preprocess_data.ipynb) — produce and save processed datasets.
  2. [notebooks/2_select_models.ipynb](notebooks/2_select_models.ipynb) — select the best performing model.
  3. [notebooks/2_boosting.ipynb](notebooks/2_boosting.ipynb) — train/save CatBoost model.
  4. [notebooks/2_uplift_modelling.ipynb](notebooks/2_uplift_modelling.ipynb) — load model and perform uplift analysis.
- Environment: the notebooks assume a Python environment with typical data-science packages (pandas, scikit-learn, catboost, joblib, scipy, tqdm, matplotlib).

## Contact / citation
- Use this README as the canonical summary when referencing the code or results in academic outputs. For method details, consult the corresponding notebook sections and the in‑notebook function definitions listed