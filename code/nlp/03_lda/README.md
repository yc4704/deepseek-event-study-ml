## Pipeline for LDA topic modeling and AI keyword counting

1. **Document construction**: Each firm's transcripts concatenated into a
   single document (400 documents total, median 5,564 words each).
2. **Text cleaning**: Lowercase, URL removal, punctuation/number removal,
   lemmatization, stopword filtering (English + domain + person names).
   Min token length: 2 (preserves 'ai', 'ml').
3. **Vectorization**: CountVectorizer with `min_df=5`, `max_df=0.8`,
   `max_features=5000`.
4. **K selection**: Scanned K in {6, 8, 10, 12, 15, 18} by perplexity +
   qualitative inspection of top words. **K=12** selected for clearest
   AI-narrative separation.
5. **Topic identification**: Topics 2, 3, 8, 9, 11 identified as AI-related
   (combined software-AI and hardware-AI-supply-chain narratives).
6. **Dictionary counting**: 26-word AI dictionary applied to raw (uncleaned)
   text to produce `ai_keyword_ratio`.

## Output columns

| Column | Description |
|---|---|
| `companyid`, `companyname` | Firm identifiers |
| `topic_00_*` through `topic_11_*` | LDA topic loadings (sum to 1.0 per firm) |
| `ai_topic_loading_sum` | Sum of loadings on AI-related topics (2,3,8,9,11) |
| `ai_keyword_count` | Raw count of AI dictionary word occurrences |
| `ai_keyword_ratio` | `ai_keyword_count` / total word count |

## Notes for downstream modeling

1. **No missing values** — safe to merge directly on `companyid`.
2. **Topic loadings form a probability distribution** — they sum to 1 per firm,
   so there is built-in collinearity. Drop one topic column or use a
   non-identity-constrained model when using all 12 as features.
3. **AI measure redundancy**:
   - `ai_keyword_count` and `ai_keyword_ratio` correlate at 0.954 — use only
     `ai_keyword_ratio` in models (already normalized by document length).
   - `ai_topic_loading_sum` and `ai_keyword_ratio` correlate at 0.527 —
     provides genuine complementary signal; keep both.
4. **76% of firms agree between LDA and dictionary** on AI intensity
   (above/below median); the 24% of "disagreement" firms are analytically
   interesting.
5. **Extreme loadings near 1.0** on some topics (e.g., Topic 0 energy)
   reflect business-focused firms, not data artifacts.

## See also

- `topic_labels.md` — full list of topic labels and top words
- `../02_finbert/` — FinBERT sentiment features (complementary module)