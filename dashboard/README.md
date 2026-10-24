# Dashboard

`Loan_Default_Risk_Analysis.pbix` is the Power BI Desktop report.

To open it:

1. Open the file in Power BI Desktop.
2. Set the `DataFolder` parameter to the full path of this repository's `data` folder, ending with a backslash (Home → Transform data → Manage Parameters), for example `C:\Users\you\Loan-Default-Risk-Analysis\data\`.
3. Select **Refresh**.

The report has five pages (Executive Overview, Default Risk Drivers, Financial Exposure, Portfolio Trends and Loan Purpose Comparison) plus a hidden report-page tooltip. The Executive Overview page also has a phone layout.

It contains two row-level security roles, Home Loans Team (static) and Purpose Managers (dynamic, using `USERPRINCIPALNAME()`). Test them in Power BI Desktop with **Modeling → View as**.
