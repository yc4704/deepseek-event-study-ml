# 01 - Data Cleaning

Raw JSON transcripts + company list → aligned executive-only dataset.

## Pipeline

This notebook runs in the following order (not strictly top-to-bottom because of the manual-fix step in the middle):

1. **Cell 1**: Match tickers to S&P Capital IQ `companyid` via WRDS
2. **Cell 2**: Check for duplicates and nulls in the matched table
3. **[Manual step]**: For 13 tickers that failed auto-matching, look up IDs in WRDS *Code Lookup* and save as `deepseek_company_info_with_ciq_id_edited.csv`
4. **Cell 4**: Export the final CIQ ID string (used to download transcripts from WRDS)
5. **[External step]**: Download JSON transcripts from WRDS using the ID string, save to a local folder
6. **Cell 6**: Parse JSON files → extract executive speech/answers → save `Executives_Transcripts_Cleaned.csv`
7. **Cell 7**: Align the company universe using NLP-matched IDs → save `Final_Matched_Universe_Full.csv`

## Inputs
- `deepseek_company_info_with_cluster.csv` — initial company list (in `data/processed/`)
- Raw JSON transcripts downloaded from WRDS (local only, not in git; update the path in Cell 6)

## Outputs
- `Executives_Transcripts_Cleaned.csv` — all executive sentences (to `data/processed/`)
- `Final_Matched_Universe_Full.csv` — final company universe (to `data/processed/`)

## Primary key
`companyid` (S&P Capital IQ company ID)

## Requirements
- WRDS account (for `wrds` Python package)
- Local copy of JSON transcripts (contact C for access)