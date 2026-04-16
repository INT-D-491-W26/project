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
short_abstract: This project uses simulated credit card transactions from the Sparkov
  generator, adds engineered features, and compares several supervised classifiers for
  binary fraud detection under extreme imbalance. Approaches include logistic
  regression, Random Forest, AdaBoost, and gradient boosting with imbalance-aware
  training. Evaluation emphasizes recall, precision, F1, ROC-AUC, and PR-AUC, together
  with threshold and cost-based analysis when false negatives are more expensive than
  false positives. The public interface is hosted on Vercel, and live fraud scoring for
  the predictive experience runs on a dedicated backend service on Render.
---
## Problem
Many fraud analytics efforts stop at offline metrics or never connect models to anything
visitors can use. This project follows one path from prepared data and exploratory
views through trained models to a public demo. Fraud is extremely rare next to normal
traffic, so accuracy alone can look strong while the system catches almost no fraud.
The work focuses on learning under imbalance, measuring recall and precision honestly,
and relating model scores to thresholds and policies when missed fraud and false alarms
carry different costs.

## Data
Transactions are simulated with the Sparkov Data Generation tool, which yields
interpretable attributes rather than anonymized principal components. Each transaction
includes amount, merchant category, geography, demographics, time information, and a
fraud indicator. Sensitive identifiers are removed before modelling. Engineered inputs
include distance between customer and merchant, spending relative to the customer’s
typical behaviour, and calendar features from the transaction time. The full
consolidated dataset is large and highly imbalanced, which reflects the needle-in-a-haystack
challenge that real fraud systems face.

## Method
All models share the same preprocessing and evaluation design. Numeric inputs are
scaled, categorical fields are encoded with rules for unseen categories at prediction
time, and the data are split so that both training and evaluation sets keep the same
rare fraud rate. The team compares a linear baseline with ensemble methods: random
forests, adaptive boosting, and gradient-boosted decision trees, using class weighting
and related techniques so the minority fraud class is not ignored during training.

Models are assessed on held-out data using confusion-based metrics, the area under the
ROC and precision–recall curves, and analyses that vary the decision threshold. A
separate cost-style view gives higher weight to false negatives than to false positives,
which matches the intuition that undetected fraud is often more damaging than extra
review of legitimate transactions. The strongest boosted model is used for deployment.

The user-facing experience is hosted on Vercel. Fraud scoring for the interactive
predictive section is not executed in the browser. It is provided by a Python
inference service on Render, which keeps heavy model work on a backend built for that
purpose.

## Results
The deliverable is an end-to-end story: descriptive and diagnostic views for context,
then live fraud risk scores in the predictive flow, and prescriptive framing informed
by the same modelling ideas. Quantitative performance depends on data scope and modelling
choices. Exploratory work in the project aligns with the trained models on several
patterns, including higher typical fraud amounts, stronger risk in certain online
merchant categories, and elevated fraud rates during late-night hours. The online demo
obtains predictive scores from the hosted backend so what you see in the browser matches
the deployed inference path.

## How to run
Open the demo link below to use the live application. Move through descriptive and
diagnostic views, then open the predictive experience. Fraud scores there are returned
from the Render-hosted inference service. Use the repository link for the team’s source
materials if you need to set up a local environment or dig into technical detail.

## Links
- Repository: https://github.com/INT-D-491-W26/project
- Demo: https://intd-w26-project.vercel.app/
- Predictive API (Render): https://project-wyfi.onrender.com/predictive
