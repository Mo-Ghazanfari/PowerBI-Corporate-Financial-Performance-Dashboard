# DAX Measures

All measures live in the `GL` table unless noted. `FTP` = "for the period" (the value in the current filter context); `TTD` = "to date" (cumulative from the first date to the latest date in context).

## Base measures

| Measure | Expression | Purpose |
|---|---|---|
| `Total_FTP` | `SUM(GL[Amount])` | Sum of ledger amounts in the current filter context |
| `Total_TTD` | `CALCULATE([Total_FTP], DATESBETWEEN(Calendar[Date], [MinDateAcross], [MaxDate]))` | Cumulative total from the first date to the latest date in context |
| `MinDate` *(Calendar)* | `MIN('Calendar'[Date])` | Earliest date in context |
| `MaxDate` *(Calendar)* | `MAX('Calendar'[Date])` | Latest date in context |
| `MinDateAcross` *(Calendar)* | `CALCULATE([MinDate], FILTER(ALL('Calendar'[Date]), 'Calendar'[Date]))` | Earliest date in the whole calendar, ignoring filters |

## Profit and loss

| Measure | Expression | Purpose |
|---|---|---|
| `SalesFTP` | `CALCULATE([Total_FTP], 'Chart of Accounts'[SubClass] = "Sales")` | Sales revenue for the period |
| `Cost of Sales` | `CALCULATE([Total_FTP], 'Chart of Accounts'[SubClass] = "Cost of Sales")` | Cost of sales for the period |
| `GrossProfit` | `CALCULATE([Total_FTP], 'Chart of Accounts'[Class] = "Trading account")` | Sales less cost of sales |
| `EBITDA` | `[GrossProfit] + CALCULATE([Total_FTP], 'Chart of Accounts'[SubClass] = "Operating Expenses")` | Gross profit after operating expenses, before depreciation and amortisation |
| `Operating Profit 2` | `[GrossProfit] + CALCULATE([Total_FTP], 'Chart of Accounts'[Class] = "Operating Account")` | Gross profit after all operating-account items |
| `PBIT` | `[Operating Profit 2] + CALCULATE([Total_FTP], 'Chart of Accounts'[Class] = "Non-operating")` | Profit before interest and tax |
| `NetProfit` | `CALCULATE([Total_FTP], 'Chart of Accounts'[Report] = "Profit and Loss")` | Net profit for the period |
| `MarketingCostFTP` | `CALCULATE(-[Total_FTP], 'Chart of Accounts'[SubClass2] = "Marketing")` | Marketing cost (sign reversed to show as positive) |
| `GPMargin` | `[GrossProfit] / [SalesFTP]` | Gross margin % |
| `NPMargin` | `[NetProfit] / [SalesFTP]` | Net margin % |

## Balance sheet

| Measure | Expression | Purpose |
|---|---|---|
| `BalanceSheetValue` | `CALCULATE([Total_FTP], 'Chart of Accounts'[Report] = "Balance Sheet", DATESBETWEEN(Calendar[Date].[Date], [MinDateAcross], [MaxDate]))` | Cumulative balance-sheet value |
| `Total Assets` | `CALCULATE([Total_TTD], 'Chart of Accounts'[Class] = "Assets")` | Total assets to date |
| `Current Assets` | `CALCULATE([Total_TTD], 'Chart of Accounts'[SubClass2] = "current Assets")` | Current assets to date |
| `Inventory` | `CALCULATE([Total_TTD], 'Chart of Accounts'[Account] = "Inventory")` | Inventory to date |
| `Current Liabilities` | `CALCULATE([Total_TTD], 'Chart of Accounts'[SubClass2] = "Current Liabilities")` | Current liabilities to date |
| `Total Debt` | `CALCULATE([Total_TTD], 'Chart of Accounts'[SubClass] = "Liabilities")` | Total liabilities to date (not only interest-bearing debt) |
| `Total Equity` | `CALCULATE([Total_TTD], 'Chart of Accounts'[SubClass] = "Owners Equity")` | Owners' equity to date |

## Ratios

| Measure | Expression | Interpretation |
|---|---|---|
| `Current Ratio` | `[Current Assets] / [Current Liabilities]` | Short-term liquidity |
| `Quick Ratio` | `([Current Assets] - [Inventory]) / [Current Liabilities]` | Liquidity excluding inventory |
| `Gearing` | `[Total Debt] / [Total Equity]` | Leverage (total liabilities to equity) |
| `Asset turnover` | `[SalesFTP] / [Total Assets]` | Sales generated per unit of assets |

## Suggested additions (not yet in the report)

Year-over-year sales growth. Requires `Calendar[Date]` to be marked as a date table.

```dax
YoY Sales Growth % =
VAR PriorYearSales =
    CALCULATE ( [SalesFTP], SAMEPERIODLASTYEAR ( 'Calendar'[Date] ) )
RETURN
    DIVIDE ( [SalesFTP] - PriorYearSales, PriorYearSales )
```

## Known issues

- `Current Assets` filters on `"current Assets"` (lower-case c). DAX string comparison is case-insensitive, so it works, but the label should match the data.
- Ratio measures use `/` instead of `DIVIDE`, so a zero denominator returns an error instead of blank.
