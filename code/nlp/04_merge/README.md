# Text Feature Merge

Combines outputs from the FinBERT and LDA modules into a single
firm-level text feature matrix for downstream modeling.

## Input

- `../../../data/processed/firm_finbert_features.csv` (from 02_finbert/)
- `../../../data/processed/firm_lda_features.csv` (from 03_lda/)

## Output

- **`X_text.csv`**: 400 firms × 28 columns (2 IDs + 11 FinBERT + 15 LDA)

## Column groups

| Group | Columns | Description |
|---|---|---|
| ID | `companyid`, `companyname` | Firm identifiers (merge key: `companyid`) |
| FinBERT | 11 columns | Sentence-level sentiment aggregated per firm |
| LDA | 15 columns | Topic loadings + AI narrative intensity features |

See `../02_finbert/README.md` and `../03_lda/README.md` for detailed
feature dictionaries.

## Known multicollinearity

The following 8 feature pairs have |correlation| > 0.9. **Drop one from
each pair when fitting linear models (Logistic Regression, LASSO)**.
Tree-based models (Random Forest, XGBoost) can retain all.

======================================================================
Highly correlated feature pairs (|r| > 0.9)
======================================================================
Found 8 pairs with |correlation| > 0.9:

  r = +0.999  |  pct_positive  <-->  ratio_strong_positive
  r = +0.997  |  pct_negative  <-->  ratio_strong_negative
  r = +0.993  |  mean_p_neutral  <-->  pct_neutral
  r = +0.992  |  mean_p_positive  <-->  pct_positive
  r = +0.991  |  mean_p_positive  <-->  ratio_strong_positive
  r = +0.989  |  mean_p_negative  <-->  pct_negative
  r = +0.989  |  mean_p_negative  <-->  ratio_strong_negative
  r = +0.954  |  ai_keyword_count  <-->  ai_keyword_ratio

All redundancies are **within** FinBERT or **within** LDA — there are no
high correlations between FinBERT features and LDA features, meaning the
two methods capture independent aspects of firm communication (tone vs. content).




The notebook reads both source CSVs, performs an inner merge on
`(companyid, companyname)`, and writes the output. No randomness involved.