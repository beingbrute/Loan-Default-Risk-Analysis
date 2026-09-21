# Data

`Loan_Default_Dataset.xlsx` was downloaded from a Udemy data analyst course. It contains 255,347 loan records with 19 columns: 18 borrower and loan fields from a public Kaggle loan-default dataset, plus an added `Loan Date (DD/MM/YYYY)` column.

Notes:

- `Loan Date (DD/MM/YYYY)` is labelled DD/MM/YYYY, but its values are in MM/DD/YYYY format. The Power BI report converts it using the English (United States) locale.
- The loan dates are spread evenly across 2013–2018 and are not linked to any other field, so time-based results demonstrate time intelligence rather than real lending history.
- `Default` is 1 for a defaulted loan and 0 otherwise.
- Amounts have no stated currency.

Column definitions are in [../documentation/Data_Dictionary.md](../documentation/Data_Dictionary.md).
