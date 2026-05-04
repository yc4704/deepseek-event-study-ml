# DeepSeek Event Study: ML Analysis of Firm-Level Reaction to the AI Shock

This project studies how U.S.-listed companies reacted to the DeepSeek R1 release in late January 2025. We use financial fundamentals together with NLP features extracted from pre-event earnings call transcripts to classify firms as Vulnerable or Resilient based on their abnormal returns, and to identify which signals best explain the post-event reaction.

## Research Question

Can firm-level financial characteristics and the language executives used in earnings calls *prior* to the DeepSeek R1 shock jointly predict the firm's market reaction to the shock?

We construct a binary label (Vulnerable vs Resilient) from cumulative abnormal returns (CAR) over the event window, and benchmark three feature sets (finance-only, text-only, and combined) across multiple classifiers.

## Data

| Source | Content |
| :---- | :---- |
| WRDS S&P Capital IQ | Earnings call transcripts (Presenter Speech + Q&A answers) |
| WRDS daily prices | Adjusted close for ~400 firms and SPY |
| SEC company tickers | Initial firm universe |

The final universe is 400 firms, matched by S&P CIQ `companyid`. For each firm we keep the latest pre-event earnings call.

## Methodology

**Y construction (event study).** Firm-level cumulative abnormal returns are computed against SPY over the post-event window. We provide two labelling schemes, clustering-based and quantile-based, and use the quantile version (top vs bottom tercile of CAR) as the primary target.

**Financial features.** Pre-event balance-sheet and income-statement variables are PCA-compressed into 8 components (`PC1`–`PC8`). Return-related variables are excluded to prevent label leakage.

**Text features.**

- *FinBERT sentiment*: sentence-level scoring aggregated per firm into 11 features (mean and std sentiment, strong-positive and strong-negative ratios, etc.).
- *LDA topic modelling*: K=12 topics on the per-firm document corpus; topics 2, 3, 8, 9, 11 are flagged as AI-related and summed into `ai_topic_loading_sum`.
- *AI keyword ratio*: dictionary-based AI mention rate over total tokens.

After dropping highly collinear sentiment columns, the merged matrix `X_text.csv` is 400 × 28 (2 IDs + 11 FinBERT + 15 LDA).

**Models.** L1 / L2 Logistic Regression, Random Forest, and XGBoost, all evaluated with 5-fold stratified cross-validation on Accuracy, F1, and ROC-AUC.

**Statistical validation.** In addition to prediction, we run Pearson / Spearman correlations and Welch / Mann–Whitney U tests of each feature against CAR and the V / R groups, with FDR-adjusted p-values.

## Repository Structure

```
deepseek-event-study-ml/
├── code/
│   ├── data pipline/             # Universe construction & price downloads
│   ├── y_label/                  # CAR & event-window returns, V/R labelling
│   │   ├── clustering_version/
│   │   └── quantile_version/
│   ├── finance/                  # Financial feature engineering → X_fin_pca
│   ├── nlp/
│   │   ├── 01_data_cleaning/     # Parse and filter transcripts
│   │   ├── 02_finbert/           # FinBERT sentiment features
│   │   ├── 03_lda/               # LDA topics + AI narrative features
│   │   └── 04_merge/             # Merge into X_text.csv
│   ├── modeling/
│   │   ├── finance_only/         # Baseline on PCA finance features
│   │   ├── text_only/            # Baseline on text features
│   │   ├── combined_model/       # Finance + text combined
│   │   ├── Final_model_comparison/   # Head-to-head & SHAP feature importance
│   │   ├── Hypothesis_Tests/     # Continuous & group-difference tests
│   │   └── Robustness_Tests/     # Industry-only & finance-robustness checks
│   └── visualization/            # Profiling and robustness visualizations
├── data/
│   ├── raw/                      # Source files (not pushed)
│   └── processed/                # Cleaned & feature outputs
└── docs/                         # Proposal drafts, reporting notes
```

Each module has its own README with pipeline steps, inputs/outputs, and feature dictionaries.

## Key Findings

- **Text features beat finance-only.** The text-only L1 Logistic Regression has the highest accuracy and ROC-AUC across the feature sets and models we tested.
- **Combined finance + text adds little over text alone.** Random Forest on the combined set is competitive but does not clearly beat the text-only model. Most of the predictive signal comes from the text.
- **AI-narrative features rank highest in importance.** Top features include `topic_02_ai_semiconductor_design`, `topic_11_ai_saas_cybersecurity`, `ai_topic_loading_sum`, `ai_keyword_ratio`, and the FinBERT measures `std_sentiment`, `ratio_strong_negative`, and `ratio_strong_positive`.
- **Hypothesis tests support the direction.** AI-narrative intensity and strong-positive sentiment ratios are significantly higher in the Vulnerable group, suggesting that pre-shock AI optimism was a main channel of exposure to the DeepSeek repricing.

## Reproducing the Pipeline

Run the modules in the order below. Most upstream steps require WRDS access.

1. `code/data pipline/00_build_universe_and_prices.ipynb`: firm universe + daily prices
2. `code/y_label/quantile_version/y_label_quantile.ipynb`: CAR computation + V/R labels
3. `code/finance/`: financial feature engineering → `X_fin_pca.csv`
4. `code/nlp/01_data_cleaning/` → `02_finbert/` → `03_lda/` → `04_merge/`: text feature pipeline → `X_text.csv`
5. `code/modeling/{finance_only, text_only, combined_model}/`: per-feature-set baselines
6. `code/modeling/Final_model_comparison/`: head-to-head and SHAP-based feature importance
7. `code/modeling/Hypothesis_Tests/` and `Robustness_Tests/`: statistical validation

Required Python packages (see individual notebooks for the full list): `pandas`, `numpy`, `scikit-learn`, `xgboost`, `transformers` (FinBERT), `gensim` / `sklearn` LDA, `statsmodels`, `wrds`.

## Team & Contributions

| Member | GitHub | Module |
| :---- | :---- | :---- |
| Harry Yan | [@hiharryyim](https://github.com/hiharryyim) | NLP: text data cleaning, FinBERT, LDA, text-feature merge (`code/nlp/`) |
| Yirui Chen | [@yc4704](https://github.com/yc4704) | Y / label construction, data pipeline (`code/y_label/`, `code/data pipline/`) |
| Xinzhe Cheng | [@chengvicky02-lgtm](https://github.com/chengvicky02-lgtm) | Modeling: baselines, final comparison, hypothesis & robustness tests (`code/modeling/`) |
| Josie Nie | [@JosieNN](https://github.com/JosieNN) | Financial feature engineering (`code/finance/`) |
| Erica Zhu | [@Ericazhu2026](https://github.com/Ericazhu2026) | Visualization & profiling (`code/visualization/`) |

## License

For academic use only. Columbia University, 2026.
