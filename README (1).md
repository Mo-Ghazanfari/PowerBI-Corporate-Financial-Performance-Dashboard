# Corporate Financial Performance Dashboard (Power BI)

An interactive Power BI report that turns a raw general-ledger (GL) extract into a P&L, balance sheet and cross-country performance view, so finance teams can answer "how are we doing?" without compiling spreadsheets by hand.

> GitHub does not render `.pbix` files. Download `Corporate_Financial_Performance_Dashboard.pbix` and open it in Power BI Desktop, or use the screenshots below.

## Dashboard preview

| P&L overview | P&L detail |
|---|---|
| ![P&L](images/pl-overview.png) | ![P&L detail](images/pl-detail.png) |

| Cross-country comparison | Balance sheet & ratios |
|---|---|
| ![Cross-country](images/cross-country.png) | ![Balance sheet](images/balance-sheet.png) |

| Sales profile |
|---|
| ![Sales profile](images/sales-profile.png) |

## Business questions it answers

- How do sales, gross profit, EBITDA and net profit develop year over year?
- Are margins (gross and net) improving or being squeezed?
- How do the seven countries compare on sales, gross profit and net profit?
- What is the balance-sheet position, and how liquid and leveraged is the business (current ratio, quick ratio, gearing, asset turnover)?

## Report pages

| Page | Content |
|---|---|
| **P&L** | Sales revenue, gross profit, EBITDA, operating profit, PBIT and net profit, with yearly trend charts |
| **P&L2** | Account-class table (all classes, annual movements), gross and net margin vs. sales, marketing cost vs. sales, KPI card, Country and Year slicers |
| **CrossCountry** | Sales, gross profit and net profit trends by country |
| **BalanceSheet** | Balance-sheet breakdown by account and sub-class, plus current ratio, quick ratio, asset turnover and gearing |
| **Sales Profile** | Sales, marketing cost, gross margin and net margin summary |

## Data

- **Source:** a sample general-ledger workbook (`Data for Assessment.xlsx`) with four sheets: `GL`, `Chart of Accounts`, `Territory` and `Calendar`. It is used here for learning and portfolio purposes only.
- **Scale:** 27,909 GL lines, January 2018 to December 2020, 7 countries (4 regions), a 54-account chart of accounts.
- **Currency:** not specified in the dataset, so values are shown without a currency symbol.

### Data model

A star schema with one fact table and three dimensions:

```
Calendar ──┐
           ├──< GL (fact) >── Chart of Accounts
Territory ─┘
```

| Table | Role | Key |
|---|---|---|
| `GL` | Fact: one row per ledger line (date, territory, account, details, amount) | `Date`, `Territory_key`, `Account_key` |
| `Calendar` | Date dimension | `Date` |
| `Chart of Accounts` | Account hierarchy: Report → Class → SubClass → SubClass2 → Account → SubAccount | `Account_key` |
| `Territory` | Country and region | `Territory_key` |

All relationships are many-to-one with single-direction filtering.

### Data preparation (Power Query)

Light preparation only: headers promoted, data types set, and two empty columns removed from `GL`. No rows were altered.

## DAX measures

26 measures cover the P&L, balance sheet and ratios. Highlights:

- **P&L chain:** `GrossProfit` → `EBITDA` → `Operating Profit 2` → `PBIT` → `NetProfit`
- **Margins:** `GPMargin`, `NPMargin`
- **Balance sheet:** cumulative values using a to-date pattern (`Total_TTD`, `BalanceSheetValue`)
- **Ratios:** `Current Ratio`, `Quick Ratio`, `Gearing`, `Asset turnover`

Full list with expressions and explanations: [DAX_Measures.md](DAX_Measures.md).

## What the data shows

Calculated from the GL in this model (please verify against the report before quoting):

- **Sales** grew from about 4.1M (2018) to 6.6M (2019) and 9.0M (2020).
- **Net profit** rose from about 0.72M to 1.50M in 2019, then stayed flat at about 1.48M in 2020.
- **Net margin** moved from roughly 17.5% to 22.9% and then fell to 16.5%.
- **Why:** in 2020, sales grew about 37%, but operating expenses grew about 58% and cost of sales about 44%, so the extra revenue did not reach net profit.
- **Seasonality:** net margin is lowest in Q1 (-3.4% in 2018, -3.8% in 2020) and highest in Q4 of every year (29.8%, 34.5%, 29.6%).
- **Liquidity vs. efficiency:** the current ratio rose from 5.17 (2018) to 8.20 (2019) and 8.08 (2020), while asset turnover fell from 0.92 to 0.64, because total assets grew about 3.2x against about 2.2x for sales.
- **Validation:** cumulative assets equal liabilities plus owners' equity in each year (4.46M, 10.62M and 14.17M).

## Limitations

- No budget or forecast data, so there is no budget-vs-actual analysis.
- No product or customer dimension; analysis is by account and country only.
- No year-over-year growth measure yet (planned).
- The data source is a local Excel file, so refresh requires pointing the source path to your own copy.
- `Gearing` uses total liabilities as "debt", not only interest-bearing debt.
- `EntryNo` is not unique in the GL (multi-line entries share a number).

## Roadmap

- Add `YoY Growth %` using time intelligence.
- Replace the hard-coded file path with a Power Query parameter.
- Add budget data and a budget-vs-actual page.
- Disable auto date/time and mark `Calendar` as the date table.

## Repository contents

| File | Description |
|---|---|
| `Corporate_Financial_Performance_Dashboard.pbix` | The Power BI report |
| `DAX_Measures.md` | All measures with expressions and explanations |
| `images/` | Report screenshots used above |

## Tools

Power BI Desktop, Power Query (M), DAX, Excel (source data).
