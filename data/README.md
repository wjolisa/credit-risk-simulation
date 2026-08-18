# Data Dictionary

The repository contains the anonymized numerical inputs required to reproduce the notebook. Neither
file contains issuer names or personal identifiers.

## `instrum_data.csv`

- Format: comma-separated, no header row
- Shape: 100 rows × 22 columns
- One row per counterparty

| Column(s) | Field | Description |
|---:|---|---|
| 1 | Counterparty ID | Numeric counterparty identifier |
| 2 | Credit driver ID | Assigned systematic driver; stored as 1–50 and converted to 0-based indexing in Python |
| 3 | Beta | Loading on the assigned systematic credit driver |
| 4 | Expected recovery rate | Fraction expected to be recovered in default |
| 5 | Market value | Current market value of the instrument |
| 6–13 | Migration probabilities | One-year probabilities ordered Default, CCC, B, BB, BBB, A, AA, AAA |
| 14–21 | State-dependent loss amounts | Gain/loss amount associated with each state in the same order |
| 22 | Market return | Market-return field supplied with the input data |

Observed validation ranges:

- Driver IDs: 1–50
- Beta: 0.4510–0.6563
- Expected recovery: 0.44–0.76
- Total portfolio market value: $845,973,751
- Migration probabilities sum to one for every row within floating-point tolerance

## `credit_driver_corr.csv`

- Format: tab-separated despite the `.csv` extension; no header row
- Shape: 50 rows × 50 columns
- Symmetric systematic-driver correlation matrix
- Diagonal entries equal one

The notebook reads this file with `sep="\t"` and applies a Cholesky decomposition to generate
correlated systematic-factor draws.
