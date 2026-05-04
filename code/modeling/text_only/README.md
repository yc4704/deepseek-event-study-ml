# Text-only Model

This folder contains the baseline model using only text-based features.

Goal

The goal is to test whether earnings call text features can predict whether a company is Vulnerable or Resilient after the AI shock event.

Data

Inputs:

- X_text.csv
- deepseek_Y_quantile_final.csv
- Final_Matched_Universe_Full.csv

The text features are first matched with ticker information using the bridge file. Then they are merged with the target label.

Features

This model uses text-based features, including:

- FinBERT sentiment features
- LDA topic features
- AI topic loading
- AI keyword ratio

Some highly redundant sentiment columns are removed to keep the baseline model cleaner.

Models

I tested two baseline models:

- Logistic Regression
- Random Forest

Both models are evaluated with 5-fold stratified cross-validation.

Outputs

- model_text_only.csv
- baseline_results_text_only.csv

Result Summary

The text-only model performs better than the finance-only baseline. This suggests that earnings call language, AI-related topics, and sentiment features provide useful signals for predicting market reaction after the AI shock event.
