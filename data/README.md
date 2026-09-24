# Data

Raw competition data are intentionally **not stored in this GitHub repository**.

Local research expects the official McGill-FIAM files, including:

- `chars_final_with_names.parquet`
- `8k_20150101_20260831_identified.parquet`
- `factor_char_list.csv`

The Parquet files are excluded by `.gitignore` because they are large and should not be duplicated into source control.

Key alignment rule:

- `ret_exc_lead1m` is already the next-month excess-return outcome attached to the current characteristic row.
- It is a target, not a predictor.
- Do not shift it again.
- Train / validation / test splits are assigned by **target month**.
