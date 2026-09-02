# Data Dictionary

## Source table: Loan_default

The source table contains one row per loan.

| Field | Type | Description |
| --- | --- | --- |
| LoanID | Identifier | Unique loan identifier |
| Age | Whole number | Borrower age in years |
| Income | Numeric | Borrower income |
| LoanAmount | Numeric | Original loan amount |
| CreditScore | Whole number | Borrower credit score |
| MonthsEmployed | Whole number | Number of months the borrower has been employed |
| NumCreditLines | Whole number | Number of borrower credit lines |
| InterestRate | Decimal number | Loan interest rate stored in percentage points |
| LoanTerm | Whole number | Loan term |
| DTIRatio | Decimal number | Borrower debt-to-income ratio |
| Education | Text | Borrower education category |
| EmploymentType | Text | Borrower employment category |
| MaritalStatus | Text | Borrower marital-status category |
| HasMortgage | True/False | Whether the borrower has a mortgage |
| HasDependents | True/False | Whether the borrower has dependents |
| HasCoSigner | True/False | Whether the loan has a co-signer |
| LoanPurpose | Text | Purpose of the loan |
| Loan Date | Date | Loan origination date |
| Default | True/False | Whether the loan defaulted |

## Derived reporting columns

| Field | Definition | Sort column |
| --- | --- | --- |
| Age Groups | Young Adults, Adults, Middle-Aged, Seniors | Age Group Sort |
| Credit Score Bins | Poor, Fair, Good, Very Good, Excellent | Credit Score Sort |
| Income Bracket | Low Income, Middle Income, High Income | Income Bracket Sort |
| Interest Rate Band | Low, Moderate, High, Very High | Interest Rate Band Sort |
| Co-signer Status | With Co-signer or Without Co-signer | Not applicable |
| Mortgage Status | Has Mortgage or No Mortgage | Not applicable |
| Dependent Status | Has Dependents or No Dependents | Not applicable |

## Category definitions

### Age Groups

| Category | Range |
| --- | --- |
| Young Adults | Age ≤ 29 |
| Adults | Age 30–44 |
| Middle-Aged | Age 45–59 |
| Seniors | Age 60+ |

### Credit Score Bins

| Category | Range |
| --- | --- |
| Poor | Below 580 |
| Fair | 580–669 |
| Good | 670–739 |
| Very Good | 740–799 |
| Excellent | 800+ |

### Income Bracket

| Category | Range |
| --- | --- |
| Low Income | Below 50,000 |
| Middle Income | 50,000–99,999 |
| High Income | 100,000+ |

### Interest Rate Band

| Category | Range |
| --- | --- |
| Low | Below 8% |
| Moderate | 8%–13.99% |
| High | 14%–19.99% |
| Very High | 20%+ |

## Date table

| Field | Type | Description |
| --- | --- | --- |
| Date | Date | Continuous daily date key |
| Year | Whole number | Calendar year |
| Quarter | Text | Calendar quarter displayed as Q1–Q4 |
| Month Number | Whole number | Number used to sort Month Name |
| Month Name | Text | Abbreviated calendar month |
| Year Month | Text | Monthly label formatted as YYYY-MM |
| Year Month Sort | Whole number | Numeric YYYYMM sorting value |

## Measures

| Measure | Description | Format |
| --- | --- | --- |
| Total Loans | Distinct count of LoanID | Whole number |
| Total Loan Amount | Sum of LoanAmount | Numeric amount |
| Average Loan Amount | Average LoanAmount | Numeric amount |
| Median Loan Amount | Median LoanAmount | Numeric amount |
| Average Interest Rate % | Average interest rate | Percentage |
| Average DTI Ratio % | Average debt-to-income ratio | Percentage |
| Defaulted Loans | Loans where Default is True | Whole number |
| Default Rate % | Defaulted Loans divided by Total Loans | Percentage |
| Defaulted Loan Amount | Amount associated with defaulted loans | Numeric amount |
| Defaulted Loan Amount % | Defaulted amount divided by total amount | Percentage |
| YTD Loan Amount | Year-to-date loan amount | Numeric amount |
| YoY Loan Amount Change % | Annual percentage change in loan amount | Percentage |
| YoY Defaulted Loans Change % | Annual percentage change in defaulted loans | Percentage |
| Borrower Risk Chart Title | Dynamic title generated from the risk-factor selection | Text |

## Interpretation

Defaulted Loan Amount represents exposure associated with defaulted loans. It does not represent realized loss because the source does not contain recovery or loss-given-default information.
