# Hypothesis Tests

This folder contains the hypothesis testing part of the project.

Goal

The goal is to test whether selected financial features and text-based features are related to market reaction after the AI shock event.

This part is different from the prediction models. Instead of training classifiers, it focuses on statistical relationships between features, CAR, and the Vulnerable / Resilient groups.

Data

Inputs:

- model_finance_text_combined.csv
- deepseek_Y_quantile_final.csv

The combined modeling dataset is merged with CAR from the target file by ticker and label.

Variables Tested

Text variables:

- mean_sentiment
- std_sentiment
- ratio_strong_positive
- ratio_strong_negative
- ai_topic_loading_sum
- ai_keyword_ratio
- topic_02_ai_semiconductor_design
- topic_09_ai_cloud_infrastructure
- topic_11_ai_saas_cybersecurity

Finance variables:

- PC1 to PC8

Tests

This script runs two types of tests.

Continuous tests against CAR:

- Pearson correlation
- Spearman correlation
- OLS regression with HC3 robust standard errors
- FDR-adjusted p-values

Group difference tests:

- Welch t-test
- Mann-Whitney U test
- Vulnerable vs Resilient mean comparison
- FDR-adjusted p-values

Outputs

- hypothesis_tests_continuous_vs_CAR.csv
- hypothesis_tests_group_diff.csv
- hypothesis_tests_summary.csv

Result Summary

This section helps connect the model results back to the original research hypotheses.

The continuous tests show whether each variable is positively or negatively associated with CAR.

The group difference tests show whether each variable is higher in the Vulnerable group or the Resilient group.

The final summary file combines both types of tests into one cleaner table for interpretation.
