# Finance-only Model

This folder contains the baseline model using only financial features.

Goal

The goal is to test whether firm-level financial variables can predict whether a company is Vulnerable or Resilient after the AI shock event.

Data

Inputs:

- X_fin_pca.csv
- deepseek_Y_quantile_final.csv

The finance features are merged with the target label by ticker.

Features

This model uses PCA-based financial features from PC1 to PC8.

I did not use CAR, returns, volatility, or drawdown as input features because these variables are related to the target label.

Models

I tested two baseline models:

- Logistic Regression
- Random Forest

Both models are evaluated with 5-fold stratified cross-validation.

Outputs

- model_finance_only.csv
- baseline_results_finance_only.csv

Result Summary

The finance-only model gives a basic baseline. The performance is not very strong, but it helps compare whether text features or combined features improve the prediction.
