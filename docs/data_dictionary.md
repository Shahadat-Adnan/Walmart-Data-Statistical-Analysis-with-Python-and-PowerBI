# Data Dictionary — Walmart Weekly Sales

## Raw Source File
`data/raw/Walmart_Sales.csv` — 6,435 rows × 8 columns, 45 stores, weekly observations from
2010-02-05 to 2012-10-26 (143 weeks per store, no gaps).

| Column | Type | Description |
|---|---|---|
| Store | Integer | Store identifier, 1–45 |
| Date | Date (raw: text `DD-MM-YYYY`) | Week-ending date |
| Weekly_Sales | Float | Total sales for the store that week, USD |
| Holiday_Flag | Binary (0/1) | 1 if the week contains Super Bowl, Labor Day, Thanksgiving, or Christmas |
| Temperature | Float | Average regional temperature that week, °F |
| Fuel_Price | Float | Regional fuel price that week, USD/gallon |
| CPI | Float | Regional Consumer Price Index that week |
| Unemployment | Float | Regional unemployment rate that week, % |

## Processed / Feature-Engineered File
`data/processed/walmart_sales_cleaned.csv` — output of `notebooks/01_walmart_sales_eda.ipynb`,
used as the Power BI data source.

| Column | Type | Description | Source |
|---|---|---|---|
| Store | Integer | Same as raw | Raw |
| Date | Date | Properly typed datetime | Cleaned from raw |
| Year | Integer | Calendar year | Derived |
| Month | Integer | Calendar month (1–12) | Derived |
| Month_Name | Text | Month name, for readable axis labels | Derived |
| Quarter | Integer | Calendar quarter (1–4) | Derived |
| Week_Of_Year | Integer | ISO week number | Derived |
| Weekly_Sales | Float | Same as raw | Raw |
| Holiday_Flag | Binary | Same as raw (official flag) | Raw |
| Is_Holiday_Week | Text | "Holiday" / "Non-Holiday" readable version of Holiday_Flag | Derived |
| Store_Tier | Text | Quartile rank of the store's average weekly sales: A (top 25%) → D (bottom 25%) | Derived |
| WoW_Sales_Change_Pct | Float | Week-over-week % change in sales, per store | Derived |
| Temperature | Float | Same as raw | Raw |
| Fuel_Price | Float | Same as raw | Raw |
| CPI | Float | Same as raw | Raw |
| Unemployment | Float | Same as raw | Raw |

## Known Data Quirk (see notebook Section 13)
`Holiday_Flag` marks the calendar week *containing* a major holiday (e.g. the week ending Dec 31
for Christmas), but the actual pre-Christmas sales spike occurs the week before that
(week ending ~Dec 23–24). This means the official flag slightly under-credits the Christmas effect.
Documented rather than "fixed," since the dashboard should show the data as it actually is, with
this caveat called out in a tooltip/note.
