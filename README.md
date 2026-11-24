# Corporate Financial Performance Dashboard

## Overview
Financial teams often spend hours manually compiling static spreadsheets just to answer basic performance questions. I built this interactive Power BI dashboard to give leadership a clear, real-time view of company revenue, profit margins, and expense trends—turning raw ledger data into instant visual insights.

> **Note:** GitHub doesn't render `.pbix` files directly in the browser. To explore the interactive dashboard, download `Corporate_Financial_Performance_Dashboard.pbix` and open it in Power BI Desktop, or view the screenshots below.

---

## Dashboard Preview

### Executive Summary
![Executive Summary](images/executive_summary.png)

### Margin & Expense Breakdown
![Margin Breakdown](images/margin_breakdown.png)

*(Tip: Once you upload screenshots to an `images/` folder, they will appear right here!)*

---

## Key Business Questions Answered
* **Revenue & Growth:** How are revenue and net profit trending Month-over-Month (MoM) and Year-over-Year (YoY)?
* **Profitability:** Which business units or product lines generate the highest margins versus the highest sales volume?
* **Cost Control:** Where are operational expenses exceeding budget, and what is driving those variances?

---

## How It Was Built
* **Data Cleaning (Power Query):** Standardized raw financial records, resolved missing values, and structured monthly budget figures for dynamic comparison against actuals.
* **Data Modeling:** Designed a clean **Star Schema** connecting the main financial transactions table (`Fact_Financials`) to lookup tables (`Dim_Date`, `Dim_Region`, `Dim_Product`) for fast filtering and accurate calculations.
* **DAX Calculations:** Built custom measures for time-intelligence (`YTD Revenue`, `YoY Growth %`), profitability ratios (`Gross Margin %`, `Net Profit Margin %`), and dynamic KPI variance indicators.
* **User Experience:** Added interactive slicers for fiscal period and region, custom hover tooltips, and drill-through views so managers can inspect underlying figures without cluttering the main screen.

---

## Key Takeaways from the Data
*(Replace these 3 bullets with real numbers or trends from your dashboard)*
* **Revenue vs. Margin:** Total revenue grew steadily across Q3, though rising operating expenses in `[Region/Department]` slightly compressed net margins.
* **Top Performers:** `[Top Category/Segment]` accounted for the largest share of overall profit, outperforming lower-margin high-volume items.
* **Budget Variance:** Actual spending stayed within `[X%]` of forecast across most departments, with the main variance concentrated in `[Specific Expense Category]`.

---

## How to View This Project
1. Download `Corporate_Financial_Performance_Dashboard.pbix` from this repository.
2. Open it in **Microsoft Power BI Desktop** (free to download).
3. Use the slicers at the top of the report to filter by year, region, or segment.
