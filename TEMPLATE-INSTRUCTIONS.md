# Personal Finance Tracker — Spreadsheet Template

File: **`Personal-Finance-Tracker-Template.csv`**

> Note on format: this environment blocks the code runtime needed to emit a native binary `.xlsx`, so the template ships as a CSV with the formulas pre-wired. It opens straight into Excel and computes live — you just re-save it as `.xlsx` (step 2) to get your working file.

## How to turn it into your .xlsx
1. **Double-click** the CSV to open it in Excel (double-click matters — it makes Excel evaluate the formulas rather than show them as text).
2. **File → Save As → Excel Workbook (.xlsx)**. That's now your tracker.

## Layout (single sheet)
| Area | Columns | What it does |
|------|---------|--------------|
| **Transaction log** | A–E | Where you enter data: `Date`, `Type` (Income/Expense), `Category`, `Description`, `Amount` |
| **Dashboard** | G–H | Auto-totals for the chosen period: Income, Expenses, Net Savings, Savings Rate, plus expenses broken down by category |
| **Budget** | J–N | Per-category `Monthly Budget` vs. `Spent` (auto), `Remaining`, and `% Used`, with a TOTAL row |

## How to use it
1. **Set the period** — `H2` (Period Start) and `H3` (Period End). Every dashboard and budget number recalculates for that date range. Default is June 2026.
2. **Log transactions** — add a row under the samples. Set `Type` to exactly `Income` or `Expense`, and use a category name that matches the dashboard/budget lists (Groceries, Dining, Transport, Housing, Utilities, Health, Shopping, Entertainment, Education, Travel, Other).
3. **Set budgets** — edit the `Monthly Budget` numbers in column K. `Spent`, `Remaining`, and `% Used` fill in automatically.
4. The 12 sample rows are there to show the math working — delete them and enter your own.

## Recommended polish after saving as .xlsx
- Format `Amount` (E), and dashboard/budget money cells as **Currency**.
- Format `Savings Rate` (H7) and `% Used` (N3:N14) as **Percentage**.
- Select the transaction table and **Insert → Table** so new rows inherit formatting.
- Optional: add **Data → Data Validation → List** dropdowns on `Type` and `Category` for clean entry.

All formulas use whole-column `SUMIFS` over a date range, so they keep working no matter how many rows you add below the samples.
