# Loan Default Risk Analysis

An end-to-end Power BI project analyzing a portfolio of **255,347 loans** to identify default-risk patterns, quantify defaulted exposure, and demonstrate time-intelligence reporting.

The report moves from an executive portfolio summary to borrower risk drivers, financial exposure, and monthly and yearly trend analysis.

## Walkthrough

A one-minute screen recording of the report in Power BI Desktop, showing cross-filtering, synced slicers, the Borrower Risk Factor field parameter, the report-page tooltip, decomposition-tree drill-down and the Reset Filters button.

https://github.com/user-attachments/assets/65466586-595a-4cb7-9301-5b38e11bdbb1

## Business objectives

- Measure the size and quality of the loan portfolio.
- Identify borrower segments associated with higher default rates.
- Compare default frequency with the share of loan amount in default.
- Analyze financial exposure across different loan purposes.
- Demonstrate monthly, YTD and year-over-year trend analysis with a dedicated Date table.
- Provide an interactive dashboard for segment-level analysis.

## Data source

- Dataset downloaded from a Udemy data analyst course. It is based on a public Kaggle loan-default dataset (255,347 loans, 18 borrower and loan fields), with an added `Loan Date` column.
- The date column is labelled `Loan Date (DD/MM/YYYY)`, but its values are in MM/DD/YYYY format (for example, `10/15/2018`), so Power Query converts it using the English (United States) locale.
- The loan dates are spread evenly across 2013–2018 and are not linked to any other field. The Portfolio Trends page therefore demonstrates time intelligence (YTD, YoY and monthly trends) rather than real changes in lending over time.
- Amounts are shown without a currency because the source does not specify one.

## How I improved this report

A review of the first version of this report showed that the default rate by employment type divided each group's defaults by *all* loans instead of the loans in that group, and that several visuals showed loan volume rather than risk. I then rebuilt the report:

- Replaced the separate default-rate calculations with one reusable `Default Rate %` measure
- Added a marked Date table and moved all time calculations (YTD, YoY) onto it
- Combined three measure tables into one, organised with display folders
- Redefined the age, credit-score and income groups, with proper sort order
- Added the Default Risk Drivers page (with a field parameter), the Financial Exposure page, a report-page tooltip, synced slicers and reset buttons
- Checked every KPI against the source data

## Recommendations (from this dataset)

The findings below are associations observed in this dataset, not proof of cause and effect.

**1. Review the highest-risk segments more closely.**
Young Adults default at 19.71% against a portfolio average of 11.61%, Low Income borrowers at 17.16%, and Very High Interest Rate loans at 17.82%. A lender could test tighter approval criteria or risk-based pricing for these segments.

**2. Test co-signer requirements for borderline applicants.**
Borrowers without a co-signer default at 12.87% compared with 10.36% for those with one, a 2.51 percentage point gap. This makes co-signer requirements worth testing, although the data alone does not show that co-signers reduce default.

**3. Size exposure by loan value, not only by loan count.**
Defaulted Amount Share (13.15%) is higher than the count-based Default Rate (11.61%), showing that defaulted loans carry above-average balances. Estimates based on loan counts alone would understate the value at risk.

**Limitation:** Defaulted Loan Amount represents defaulted exposure, not realised loss. The dataset contains no recovery or loss-given-default information, so these figures size exposure rather than final write-offs.

## Headline results

| KPI | Result |
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

Amounts are in the dataset's own units; the source does not specify a currency.

## Dashboard pages

### 1. Executive Overview

Provides a high-level summary of portfolio size, default performance, borrower employment risk, age-group risk, and annual default rates.

![Executive Overview](screenshots/01_executive_overview.png)

### 2. Default Risk Drivers

Analyzes default rates across credit score, income, interest-rate and borrower-status segments.

The Borrower Risk Factor field parameter allows users to switch dynamically between:

- Co-signer Status
- Mortgage Status
- Dependent Status

![Default Risk Drivers](screenshots/02_default_risk_drivers.png)

### 3. Financial Exposure

Compares total loan exposure with defaulted amount share by loan purpose. The decomposition tree allows users to investigate defaulted loan exposure through different borrower characteristics.

![Financial Exposure](screenshots/03_financial_exposure.png)

### 4. Portfolio Trends

Demonstrates time-intelligence measures built on the Date table:

- Monthly Loan Amount
- Monthly Default Rate
- YoY Loan Amount Change
- YoY Defaulted Loans Change

Because the loan dates are evenly distributed (see [Data source](#data-source)), this page shows how the measures work rather than real lending trends.

![Portfolio Trends](screenshots/04_portfolio_trends.png)

## Interactive tooltip

A dedicated report-page tooltip displays contextual KPIs when users hover over a chart category:

- Total Loans
- Defaulted Loans
- Default Rate
- Total Loan Amount

![Interactive Tooltip](screenshots/05_interactive_tooltip.png)

## Key findings

- **Young Adults** have the highest age-based default rate at **19.71%**.
- **Low Income** borrowers have a default rate of **17.16%**.
- Loans in the **Very High Interest Rate** band have a default rate of **17.82%**.
- **Unemployed** borrowers have the highest employment-based default rate at **13.55%**.
- **Poor Credit** borrowers default at **12.47%**, compared with **9.81%** for borrowers with excellent credit.
- Borrowers without a co-signer have a default rate of **12.87%**, compared with **10.36%** for borrowers with a co-signer.
- Borrowers without dependents have a default rate of **12.72%**, compared with **10.50%** for borrowers with dependents.
- Defaulted Amount Share is **13.15%**, higher than the loan-count Default Rate of **11.61%**. This indicates that defaulted loans tend to carry larger amounts than the portfolio average.
- Home loans have the lowest Defaulted Amount Share among loan purposes at **11.64%**.

## Power BI features implemented

- Power Query data cleaning, including locale-aware date conversion
- Dedicated Date table
- Active one-to-many relationship
- Centralized measure table
- Measure display folders
- DAX measures and calculated columns
- YTD and YoY time intelligence
- Monthly portfolio analysis
- Field parameters
- Dynamic visual titles
- Report-page tooltips
- Synced slicers
- Bookmark-based Reset Filters buttons
- Conditional formatting
- Positive and negative YoY colors
- Heatmap matrix
- Secondary Y-axis combination chart
- Decomposition tree

## Data model

The semantic model contains:

- `Loan_default` — loan-level fact table
- `Date Table` — dedicated calendar table
- `_Measures` — centralized measure table
- `Borrower Risk Factor` — disconnected field-parameter table

The active relationship is:

```text
Date Table[Date]  1 ──────── *  Loan_default[Loan Date]
```

Cross-filter direction is single from the Date table to the loan fact table.

The Date table covers:

```text
01 January 2013 to 31 December 2018
```

## Data preparation

- Verified 255,347 loan records.
- Checked for missing values and duplicate rows.
- Converted `Loan Date` from MM/DD/YYYY text to a Date type using the English (United States) locale.
- Assigned appropriate numeric, text, and logical data types.
- Converted mortgage, dependent, co-signer, and default indicators to Boolean values.
- Created age, credit-score, income and interest-rate bands.
- Added numerical sort columns for ordered categories.
- Created and marked a dedicated Date table.
- Removed the redundant fact-table Year column.
- Hid technical sorting fields and raw Boolean columns.
- Organized measures into business-friendly display folders.

Detailed documentation is available in:

- [DAX Measures](documentation/DAX_Measures.md)
- [Data Dictionary](documentation/Data_Dictionary.md)
- [Data Preparation](documentation/Data_Preparation.md)

## Tools used

- Power BI Desktop
- Power Query
- DAX

## Repository structure

```text
Loan-Default-Risk-Analysis/
├── README.md
├── dashboard/
│   ├── README.md
│   └── Loan_Default_Risk_Analysis.pbix
├── data/
│   ├── README.md
│   └── Loan_Default_Dataset.xlsx
├── documentation/
│   ├── DAX_Measures.md
│   ├── Data_Dictionary.md
│   └── Data_Preparation.md
└── screenshots/
    ├── README.md
    ├── 01_executive_overview.png
    ├── 02_default_risk_drivers.png
    ├── 03_financial_exposure.png
    ├── 04_portfolio_trends.png
    └── 05_interactive_tooltip.png
```

## How to use the project

1. Download or clone this repository.
2. Open `dashboard/Loan_Default_Risk_Analysis.pbix` in Power BI Desktop.
3. Set the `DataFolder` parameter to the full path of this repository's `data` folder, ending with a backslash (Home → Transform data → Manage Parameters), for example `C:\Users\you\Loan-Default-Risk-Analysis\data\`.
4. Refresh the Power BI model.
5. Use the slicers, borrower-risk selector, tooltips, and decomposition tree to explore the portfolio.

## Interpretation note

`Defaulted Loan Amount` represents the loan amount associated with defaulted records.

It should be interpreted as **defaulted exposure**, not realized financial loss, because the dataset does not contain recovery or loss-given-default information.

## Author

**Aditya Ranjan**

Data Analyst | Python · SQL · Power BI · Tableau

[LinkedIn](https://www.linkedin.com/in/aditya-ranjan-data) · [GitHub](https://github.com/beingbrute) · adityaranjan17302215@gmail.com
