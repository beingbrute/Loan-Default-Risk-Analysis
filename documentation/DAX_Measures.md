# DAX Measures and Calculations

## Portfolio KPIs

### Total Loans

```DAX
Total Loans =
DISTINCTCOUNT(Loan_default[LoanID])
```

### Total Loan Amount

```DAX
Total Loan Amount =
SUM(Loan_default[LoanAmount])
```

### Average Loan Amount

```DAX
Average Loan Amount =
AVERAGE(Loan_default[LoanAmount])
```

### Median Loan Amount

```DAX
Median Loan Amount =
MEDIAN(Loan_default[LoanAmount])
```

### Average Interest Rate

```DAX
Average Interest Rate % =
DIVIDE(
    AVERAGE(Loan_default[InterestRate]),
    100
)
```

### Average DTI Ratio

```DAX
Average DTI Ratio % =
AVERAGE(Loan_default[DTIRatio])
```

## Default-risk measures

### Defaulted Loans

```DAX
Defaulted Loans =
CALCULATE(
    [Total Loans],
    KEEPFILTERS(Loan_default[Default] = TRUE())
)
```

### Default Rate

```DAX
Default Rate % =
DIVIDE(
    [Defaulted Loans],
    [Total Loans],
    0
)
```

### Defaulted Loan Amount

```DAX
Defaulted Loan Amount =
CALCULATE(
    [Total Loan Amount],
    KEEPFILTERS(Loan_default[Default] = TRUE())
)
```

### Defaulted Amount Share

```DAX
Defaulted Loan Amount % =
DIVIDE(
    [Defaulted Loan Amount],
    [Total Loan Amount],
    0
)
```

This measure is displayed in report visuals using the business-facing label:

```text
Defaulted Amount Share
```

## Time-intelligence measures

### YTD Loan Amount

```DAX
YTD Loan Amount =
CALCULATE(
    [Total Loan Amount],
    DATESYTD('Date Table'[Date])
)
```

### YoY Loan Amount Change

```DAX
YoY Loan Amount Change % =
VAR PreviousYearAmount =
    CALCULATE(
        [Total Loan Amount],
        DATEADD(
            'Date Table'[Date],
            -1,
            YEAR
        )
    )
RETURN
    IF(
        ISBLANK(PreviousYearAmount),
        BLANK(),
        DIVIDE(
            [Total Loan Amount] - PreviousYearAmount,
            PreviousYearAmount
        )
    )
```

### YoY Defaulted Loans Change

```DAX
YoY Defaulted Loans Change % =
VAR PreviousYearDefaults =
    CALCULATE(
        [Defaulted Loans],
        DATEADD(
            'Date Table'[Date],
            -1,
            YEAR
        )
    )
RETURN
    IF(
        ISBLANK(PreviousYearDefaults),
        BLANK(),
        DIVIDE(
            [Defaulted Loans] - PreviousYearDefaults,
            PreviousYearDefaults
        )
    )
```

## Dynamic Borrower Risk title

```DAX
Borrower Risk Chart Title =
VAR SelectedOrder =
    SELECTEDVALUE(
        'Borrower Risk Factor'[Borrower Risk Factor Order],
        -1
    )
RETURN
    SWITCH(
        SelectedOrder,
        0, "Default Rate by Co-signer Status",
        1, "Default Rate by Mortgage Status",
        2, "Default Rate by Dependent Status",
        "Default Rate by Borrower Risk Factor"
    )
```

The numerical parameter order is used to avoid the field-parameter composite-key limitation.

## Date table

```DAX
Date Table =
ADDCOLUMNS(
    CALENDAR(
        MIN(Loan_default[Loan Date]),
        MAX(Loan_default[Loan Date])
    ),
    "Year", YEAR([Date]),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMM"),
    "Year Month", FORMAT([Date], "YYYY-MM"),
    "Year Month Sort",
        YEAR([Date]) * 100 + MONTH([Date])
)
```

Date table configuration:

- Marked as a Date table using `Date Table[Date]`
- `Month Name` sorted by `Month Number`
- `Year Month` sorted by `Year Month Sort`
- Active relationship with `Loan_default[Loan Date]`

## Borrower Risk Factor field parameter

```DAX
Borrower Risk Factor = {
    (
        "Co-signer Status",
        NAMEOF('Loan_default'[Co-signer Status]),
        0
    ),
    (
        "Mortgage Status",
        NAMEOF('Loan_default'[Mortgage Status]),
        1
    ),
    (
        "Dependent Status",
        NAMEOF('Loan_default'[Dependent Status]),
        2
    )
}
```

## Age groups

```DAX
Age Groups =
SWITCH(
    TRUE(),
    Loan_default[Age] <= 29, "Young Adults",
    Loan_default[Age] <= 44, "Adults",
    Loan_default[Age] <= 59, "Middle-Aged",
    "Seniors"
)
```

```DAX
Age Group Sort =
SWITCH(
    TRUE(),
    Loan_default[Age] <= 29, 1,
    Loan_default[Age] <= 44, 2,
    Loan_default[Age] <= 59, 3,
    4
)
```

## Credit-score bands

```DAX
Credit Score Bins =
SWITCH(
    TRUE(),
    Loan_default[CreditScore] < 580, "Poor",
    Loan_default[CreditScore] < 670, "Fair",
    Loan_default[CreditScore] < 740, "Good",
    Loan_default[CreditScore] < 800, "Very Good",
    "Excellent"
)
```

```DAX
Credit Score Sort =
SWITCH(
    TRUE(),
    Loan_default[CreditScore] < 580, 1,
    Loan_default[CreditScore] < 670, 2,
    Loan_default[CreditScore] < 740, 3,
    Loan_default[CreditScore] < 800, 4,
    5
)
```

## Income brackets

```DAX
Income Bracket =
SWITCH(
    TRUE(),
    Loan_default[Income] < 50000, "Low Income",
    Loan_default[Income] < 100000, "Middle Income",
    "High Income"
)
```

```DAX
Income Bracket Sort =
SWITCH(
    TRUE(),
    Loan_default[Income] < 50000, 1,
    Loan_default[Income] < 100000, 2,
    3
)
```

## Interest-rate bands

```DAX
Interest Rate Band =
SWITCH(
    TRUE(),
    Loan_default[InterestRate] < 8, "Low",
    Loan_default[InterestRate] < 14, "Moderate",
    Loan_default[InterestRate] < 20, "High",
    "Very High"
)
```

```DAX
Interest Rate Band Sort =
SWITCH(
    TRUE(),
    Loan_default[InterestRate] < 8, 1,
    Loan_default[InterestRate] < 14, 2,
    Loan_default[InterestRate] < 20, 3,
    4
)
```

## Business-friendly status labels

### Co-signer Status

```DAX
Co-signer Status =
IF(
    Loan_default[HasCoSigner] = TRUE(),
    "With Co-signer",
    "Without Co-signer"
)
```

### Mortgage Status

```DAX
Mortgage Status =
IF(
    Loan_default[HasMortgage] = TRUE(),
    "Has Mortgage",
    "No Mortgage"
)
```

### Dependent Status

```DAX
Dependent Status =
IF(
    Loan_default[HasDependents] = TRUE(),
    "Has Dependents",
    "No Dependents"
)
```

## Measure display folders

### 01 Portfolio KPIs

- Total Loans
- Total Loan Amount
- Average Loan Amount
- Median Loan Amount
- Average Interest Rate %
- Average DTI Ratio %

### 02 Default Risk

- Defaulted Loans
- Default Rate %
- Defaulted Loan Amount
- Defaulted Loan Amount %
- Borrower Risk Chart Title

### 03 Time Intelligence

- YTD Loan Amount
- YoY Loan Amount Change %
- YoY Defaulted Loans Change %
