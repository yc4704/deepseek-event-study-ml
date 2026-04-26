# Finance + Text Combined Model

This folder contains the combined baseline model using both financial features and text-based features.

Goal

The goal is to test whether combining firm-level financial variables with earnings call text features can better predict whether a company is Vulnerable or Resilient after the AI shock event.

Data

Inputs:

- X_text.csv
- X_fin_pca.csv
- deepseek_Y_quantile_final.csv
- Final_Matched_Universe_Full.csv

The text features are matched with ticker information using the bridge file. Then the text features, finance features, and target labels are merged together.

Features

This model uses two groups of features.

Finance features:

- PC1 to PC8

Text features:

- FinBERT sentiment features
- LDA topic features
- AI topic loading
- AI keyword ratio

In total, the combined model uses 27 features.

I removed some highly redundant text columns, such as pct_positive, pct_negative, pct_neutral, and raw probability columns, to keep the model cleaner.

Models

I tested two baseline models:

- Logistic Regression
- Random Forest

Both models are evaluated with 5-fold stratified cross-validation.

Outputs

- model_finance_text_combined.csv
- baseline_results_finance_text_combined.csv

Result Summary

The combined model performs better than the finance-only baseline, but it does not clearly outperform the text-only model.

In this baseline test, Random Forest performs slightly better than Logistic Regression. This suggests that adding financial features may help, but most of the useful signal still seems to come from the text features.
