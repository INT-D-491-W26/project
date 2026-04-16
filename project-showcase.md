---
title: Credit Card Fraud Detection
semester: 2026 Winter
team:
- Tyrone Bougiridis
- Reuben John
- Gurkeerat Kakar
- Krystal Kim
- Mohammed Ishfaq Mostain
- Elizabeth Seto
tags:
- Machine learning
- Imbalanced classification
- Fraud detection
- Vercel
- XGBoost
repo_url: https://github.com/INT-D-491-W26/project
demo_url: https://intd-w26-project.vercel.app/
poster_image: /assets/images/projects/2026-winter/credit-card-fraud-detection-poster.svg
group_image: /assets/images/projects/2026-winter/credit-card-fraud-detection-group.svg
short_abstract: Sparkov-simulated transactions, engineered features, and four binary
  classifiers in one sklearn pipeline (logistic regression, Random Forest, AdaBoost,
  XGBoost). Class weights and scale_pos_weight handle imbalance. XGBoost is exported for
  serving. Metrics include recall, precision, F1, ROC-AUC, PR-AUC, plus threshold and
  cost-based analysis. UI on Vercel, live `/predictive` inference on Render.
---
## Problem
Many fraud projects stop at offline metrics or keep deployment out of sight. This
work was built to follow a single thread from data and exploration through trained
models to something you can show in a browser. Fraud is extremely rare compared with
legitimate transactions, so accuracy alone can look strong while the model catches
almost no fraud. The real question is how to learn under imbalance, measure recall and
precision honestly, and turn probability scores into thresholds and policies when a
missed fraud and a false alarm do not cost the same.

## Data
The app draws on synthetic credit card transactions from the Sparkov Data Generation
pipeline. Rows include amount, merchant category, geography, demographics, timestamps,
and a binary `is_fraud` label, with interpretable fields rather than PCA-obscured
features. The team loads cleaned CSVs into `cleaned_data_files/`, removes identifiers
that should not appear in modelling, and engineers Haversine distance between customer
and merchant, customer spending versus historical average, and calendar features from
`trans_datetime`. The consolidated data used in the report is large and highly
imbalanced, which mirrors the needle-in-a-haystack setting fraud systems face in
practice.

## Method
Modelling uses a unified sklearn `Pipeline` for everyone: numeric features are scaled,
categoricals are one-hot encoded with unknown categories ignored at prediction time,
and the train-test split is stratified so fraud remains rare but present in both
partitions. Four supervised binary classifiers share that preprocessing: logistic
regression as a linear baseline, then Random Forest, AdaBoost, and XGBoost for
non-linear structure. Class imbalance is handled with class weights on sklearn
estimators and with `scale_pos_weight` on XGBoost derived from training counts.

After fitting, the training code selects a classification threshold on the training
split to maximize F1, then evaluates on the holdout set at that operating point. The
team also reports recall, precision, F1, ROC-AUC, and PR-AUC, and studies cost-weighted
thresholds where false negatives are penalized more heavily than false positives.
XGBoost is the model exported for serving. The repository includes `train_and_save.py`
to write `model.joblib`, and a small Flask app (`app.py`) can load that artifact for
local approve, review, or block style rules.

The public interface is deployed on Vercel. The predictive experience does not run
inference inside the edge UI. Instead the front end calls a Python service on Render
at `/predictive`, which keeps training and batch evaluation in this repo while the live
demo matches a realistic split between a static site and a long-running ML API.

## Results
The outcome is an end-to-end story you can walk through in the demo: descriptive and
diagnostic views for context, then live fraud scores from the same family of models
documented here. Reported metrics depend on which cleaned files and hyperparameters you
use, but training runs leave traces under `outputs/run-logs/` for reproducibility. The
written report connects model behaviour to the domain, including higher fraud amounts,
online categories, and late-night patterns. Taken together, the project shows how
imbalance-aware training, careful metrics, and deployment choices fit the same problem
definition from the introduction through to prescriptive policy.

## How to run
1. Clone `https://github.com/INT-D-491-W26/project`.
2. Place cleaned CSV files under `cleaned_data_files/` (see README and
   `491_cleaning.py` for the schema).
3. Install Python dependencies with `pip3 install -r requirements.txt`.
4. Run `python -m modelling.main` to train all four models.
5. Run `python train_and_save.py` to export XGBoost and write `model.joblib`.
6. Run `python app.py` after `model.joblib` exists to start the local Flask app.
7. For the hosted stack only, use the Demo and Predictive API URLs under Links (Vercel
   UI plus Render inference).

## Links
- Repository: https://github.com/INT-D-491-W26/project
- Demo: https://intd-w26-project.vercel.app/
- Predictive API (Render): https://project-wyfi.onrender.com/predictive
