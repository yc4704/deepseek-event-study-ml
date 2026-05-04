#### Feature dictionary

| Column | Type | Description |
|---|---|---|
| `companyid`, `companyname` | ID | Firm identifiers |
| `n_sentences` | int | Sentences used in aggregation (min 55, median 280) |
| `mean_p_positive/negative/neutral` | float [0,1] | Mean FinBERT class probabilities across sentences |
| `mean_sentiment` | float [-1,1] | Mean of (p_positive - p_negative); primary tone signal |
| `std_sentiment` | float | Std of signed sentiment; captures tone volatility |
| `pct_positive/negative/neutral` | float [0,1] | Proportion of sentences assigned each label via argmax |
| `ratio_strong_positive` | float [0,1] | Proportion with p_positive > 0.5 (high-confidence optimistic) |
| `ratio_strong_negative` | float [0,1] | Proportion with p_negative > 0.5 (high-confidence pessimistic) |

#### Notes for modeling

1. **No missing values** — safe to merge directly on `companyid`.
2. **Multicollinearity**:
   - `pct_positive` ~ `ratio_strong_positive` (r = 0.999)
   - `pct_negative` ~ `ratio_strong_negative` (r = 0.997)
   - Suggest keeping only the `ratio_strong_*` versions for tree models;
     logistic regression should drop one of each pair.
3. **Known confound — geographic/linguistic bias**:
   - Top-10 most optimistic firms are dominated by Chinese ADRs (Bilibili,
     Baidu, Kingsoft Cloud, Full Truck Alliance, etc.).
   - Consider adding an ADR/non-US dummy as control, or using rank-based
     sentiment rather than absolute values.
4. **Theoretically relevant features for H2** (AI optimism → vulnerability):
   - `mean_sentiment` and `ratio_strong_positive` are the primary signals.
   - Check against LDA's AI-narrative intensity for interaction effects.
5. **Pipeline details**: sentences with <3 words excluded (6% of raw sentences);
   all 400 firms survived the filter.