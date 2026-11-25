# Corporate Financial Performance Dashboard (Power BI)

An interactive Power BI report that turns a raw general-ledger (GL) extract into a P&L, balance sheet, and cross-country performance view—allowing finance teams to answer *"how are we doing?"* without compiling spreadsheets by hand.

> **Note:** GitHub does not render `.pbix` files directly in the browser. Download `Corporate_Financial_Performance_Dashboard.pbix` and open it in **Power BI Desktop**, or explore the visual walkthrough below.

## Dashboard Preview

### P&L Overview (`P&L`)
<img width="1854" height="915" alt="P&L Overview" src="https://github.com/user-attachments/assets/c9a4ac28-7a55-49bd-95dd-45c61cfb14a1" />

### P&L Detail (`P&L2`)
<img width="1851" height="915" alt="P L2" src="https://github.com/user-attachments/assets/56ec173c-0e21-4b08-a3bd-43b52a04443b" />

### Cross-Country Comparison (`CrossCountry`)
<img width="1852" height="912" alt="CrossCountry" src="https://github.com/user-attachments/assets/f19a792b-c7d7-4da0-9141-f29356b49088" />

### Balance Sheet & Ratios (`BalanceSheet`)
<img width="1849" height="913" alt="BalanceSheet" src="https://github.com/user-attachments/assets/b38d8e44-6215-490a-a13a-fc6aebbecf06" />

### Sales Profile (`Sales Profile`)
<img width="1851" height="899" alt="SalesProfit" src="https://github.com/user-attachments/assets/8c334f26-8b8d-45e9-86e0-a880223d2338" />

---

## Business Questions Answered
* **Revenue & Profit Trajectory:** How do sales, gross profit, EBITDA, and net profit develop year over year?
* **Margin Dynamics:** Are gross and net margins expanding or being compressed as the business scales?
* **Regional Performance:** How do the seven countries compare across sales, gross profit, and net profit?
* **Balance Sheet & Liquidity:** What is the financial position, and how liquid and leveraged is the business (current ratio, quick ratio, gearing, asset turnover)?

---

## Report Pages

| Page | Content |
| :--- | :--- |
| **P&L** | Sales revenue, gross profit, EBITDA, operating profit, PBIT, and net profit with yearly trend charts |
| **P&L2** | Account-class matrix (all classes, annual movements), gross and net margin vs. sales, marketing cost vs. sales, KPI cards, and Country/Year slicers |
| **CrossCountry** | Sales, gross profit, and net profit trends segmented across 7 countries |
| **BalanceSheet** | Statement of financial position by account and sub-class, plus current ratio, quick ratio, asset turnover, and gearing |
| **Sales Profile** | Quarterly and annual summary of sales, marketing costs, gross margin, and net margin |

---

## Data & Architecture

* **Source:** General-ledger workbook containing four tables: `GL`, `Chart of Accounts`, `Territory`, and `Calendar`.
* **Scale:** 27,909 GL entries spanning January 2018 to December 2020 across 7 countries (4 regions) and a 54-account chart of accounts.
* **Currency:** Values are presented without a currency symbol as the source extract is currency-agnostic.

### Data Model
A **star schema** with one central fact table and three dimension tables connected via many-to-one, single-direction relationships:

```text
Calendar ──┐
           ├──< GL (fact) >── Chart of Accounts
Territory ─┘
