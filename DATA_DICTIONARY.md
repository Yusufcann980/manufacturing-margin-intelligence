# Data Dictionary & Modelling Notes

All data in this project is **synthetic**. Amounts are in Turkish Lira (₺ / TRY).

## `PBI_Fact_PL` — fact table (grain: one line item per month)

| Column | Type | Description |
|---|---|---|
| `Date` | Date | First day of the month (YYYY-MM-DD) |
| `Year` | Whole number | Fiscal year (2025) |
| `Month_Number` | Whole number | 1–12 |
| `Month_Name` | Text | January … December |
| `Quarter` | Text | Q1–Q4 |
| `Line_Item_TR` | Text | P&L line item, Turkish |
| `Line_Item_EN` | Text | P&L line item, English |
| `Account_Code` | Text | Chart-of-accounts code (e.g. 600 = sales) |
| `Cost_Type` | Text | Revenue · Değişken Gider (variable) · Sabit Gider (fixed) · Semi-Variable |
| `Department` | Text | Sales · Production · Marketing · Intercompany |
| `Category` | Text | Roll-up bucket (Revenue, Variable Cost, Fixed Cost, Expenses Total, …) |
| `Is_Total_Row` | Boolean | TRUE = subtotal/total row — exclude from detail visuals |
| `Amount_TRY` | Decimal | Monthly amount in TRY |
| `Monthly_Avg_TRY` | Decimal | Trailing monthly average |

## `PBI_Monthly_Summary` — pre-aggregated P&L (QA reference)

Rows: Total Sales · Total Variable Cost · Total Fixed Cost · Total Expenses ·
Monthly Net Profit · Cumulative Net Profit. Columns are the 12 months.
Use this tab to cross-check dashboard measures against a known-good aggregate.

## Power BI rebuild — quick steps

1. **Connect:** Get Data → Excel → this workbook → load `PBI_Fact_PL`.
2. **Types:** set `Date` = Date, `Year`/`Month_Number` = Whole Number,
   `Amount_TRY`/`Monthly_Avg_TRY` = Decimal, `Is_Total_Row` = Boolean.
3. **Measures:** add the four DAX measures from the README.
4. **Visuals:** line (monthly trend), waterfall (P&L bridge), clustered bar
   (department), KPI cards.
5. **Slicers:** Year, Quarter, Month_Name, Category, Department, Is_Total_Row.
6. **Guard-rail:** filter `Is_Total_Row = FALSE` on detail visuals.
