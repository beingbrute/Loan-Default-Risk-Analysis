# Data Preparation and Modeling

## 1. Source ingestion

The loan-level Excel dataset was imported into Power BI and loaded as `Loan_default`.

The dataset contains:

- 255,347 loan records
- 19 source fields
- Data from 01 January 2013 to 31 December 2018

## 2. Data-quality checks

The following checks were performed:

- Reviewed column quality and distribution
- Checked for missing values
- Checked for duplicate rows
- Verified the minimum and maximum loan dates
- Verified final portfolio totals after transformation

## 3. Data types

The following data types were applied:

- `Loan Date` → Date
- `HasMortgage` → True/False
- `HasDependents` → True/False
- `HasCoSigner` → True/False
- `Default` → True/False
- Loan amount, income, credit score, loan term and ratio fields → Numeric
- Employment, education, marital status and loan-purpose fields → Text

## 4. Reporting categories

Calculated columns were created for:

- Age Groups
- Credit Score Bins
- Income Bracket
- Interest Rate Band
- Co-signer Status
- Mortgage Status
- Dependent Status

Numerical sort columns were created to preserve the intended category order.

## 5. Date dimension

A dedicated Date table was created using the minimum and maximum loan dates.

The Date table contains 2,191 daily dates and includes:

- Year
- Quarter
- Month Number
- Month Name
- Year Month
- Year Month Sort

Configuration:

- Marked as the model Date table
- Month Name sorted by Month Number
- Year Month sorted by Year Month Sort
- Active relationship with Loan_default[Loan Date]
- Single-direction filtering from the Date table

## 6. Measure organization

Measures are stored inside `_Measures` and organized into:

- `01 Portfolio KPIs`
- `02 Default Risk`
- `03 Time Intelligence`

## 7. Model cleanup

- Removed duplicate and obsolete measures
- Removed obsolete measure tables
- Deleted the redundant Loan_default Year column
- Hid technical sorting columns
- Hid raw Boolean columns
- Hid the raw fact-table date where appropriate
- Kept the field-parameter table disconnected by design

## 8. Validated results

| Metric | Result |
| --- | ---: |
| Total Loans | 255,347 |
| Total Loan Amount | 32.58bn |
| Defaulted Loans | 29,653 |
| Default Rate | 11.61% |
| Average Loan Amount | 127.58K |
| Defaulted Loan Amount | 4.29bn |
| Defaulted Amount Share | 13.15% |
| Average Interest Rate | 13.49% |
| Average DTI Ratio | 50.02% |

## 9. Report functionality

- Common slicers are synchronized across report pages
- Reset Filters buttons use page-specific bookmarks
- Bookmarks capture only slicer data state
- Borrower Risk Factor uses a single-select field parameter
- Dynamic titles respond to the field-parameter selection
- A hidden report-page tooltip displays contextual KPIs
- YoY charts use balanced axes
- Negative YoY values are red
- Positive YoY values are green
- Amounts and percentages use consistent display formats
