# Final Model Comparison

This folder contains the final model comparison and feature importance analysis.

This part compares three feature sets:

- Finance-only features
- Text-only features
- Finance + text combined features

The goal is to see which type of features gives the best prediction for whether a company is Vulnerable or Resilient after the AI shock event.

Data

Inputs:

- model_finance_only.csv
- model_text_only.csv
- model_finance_text_combined.csv

Feature Sets

Finance-only:

- PC1 to PC8

Text-only:

- FinBERT sentiment features
- LDA topic features
- AI topic loading
- AI keyword ratio

Combined:

- Finance PCA features
- Text-based features

Models

I tested four models for each feature set:

- L1 Logistic Regression
- L2 Logistic Regression
- Random Forest
- XGBoost

All models are evaluated with 5-fold stratified cross-validation.

Metrics

The models are compared using:

- Accuracy
- F1 score
- ROC-AUC

Outputs

- model_results.csv
- feature_importance.csv

Result Summary

The text-only models perform better than the finance-only models overall.

The best-performing model is the text-only L1 Logistic Regression model, with the strongest overall accuracy and ROC-AUC.

The combined model also performs well, but adding finance features does not clearly improve performance over the text-only model.

Feature Importance

The feature importance analysis focuses on:

- Text-only L1 Logistic Regression
- Combined Random Forest
- Text-only XGBoost

Important features include:

- topic_02_ai_semiconductor_design
- std_sentiment
- ratio_strong_negative
- ratio_strong_positive
- topic_11_ai_saas_cybersecurity
- ai_topic_loading_sum
- ai_keyword_ratio

Conclusion

The final results suggest that text features from earnings calls provide more useful predictive signals than finance-only features. Financial variables still provide some information, but the main signal comes from company language, AI-related topics, and sentiment.
