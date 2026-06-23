# Walmart Weekly Sales — End-to-End Data Analysis & Power BI Dashboard

End-to-end analytics project on 3 years of weekly sales across 45 Walmart stores: data cleaning,
feature engineering, statistical analysis in Python, and an interactive Power BI dashboard.

## Business Problem

Walmart wants to understand what drives variation in weekly sales across its store network so that
merchandising, staffing, and promotional planning can be improved. This project answers:

1. How concentrated is revenue across the 45-store network?
2. Do holiday weeks produce a statistically significant sales lift?
3. Is there meaningful month-to-month seasonality?
4. Do macroeconomic conditions (CPI, unemployment, fuel price, temperature) actually drive sales?
5. Are there data quality issues that would mislead a dashboard built on this data?

## Dataset

- **Source:** `Walmart_Sales.csv` — 6,435 rows, 8 columns
- **Granularity:** weekly sales per store
- **Coverage:** 45 stores, 2010-02-05 to 2012-10-26 (143 weeks per store, no missing weeks)
- **Quality:** no nulls, no duplicates — clean at the row level (see `docs/data_dictionary.md` for
  full field definitions and a known labeling quirk in `Holiday_Flag`)

## Tech Stack

| Layer | Tool |
|---|---|
| Data cleaning & EDA | Python (pandas, numpy) |
| Statistical testing | scipy |
| Visualization (analysis) | matplotlib, seaborn |
| Notebook environment | Jupyter |
| Dashboard | Power BI Desktop + DAX |

## Project Structure

```
walmart_sales_analysis/
├── data/
│   ├── raw/                     # original, untouched source file
│   │   └── Walmart_Sales.csv
│   └── processed/                # cleaned + feature-engineered output -> feeds Power BI
│       └── walmart_sales_cleaned.csv
├── notebooks/
│   └── 01_walmart_sales_eda.ipynb   # full cleaning, EDA, stats, feature engineering
├── reports/
│   └── figures/                  # exported chart PNGs (also embedded in the notebook)
├── dashboard/
│   └── powerbi_theme.json        # custom Power BI theme matching the notebook's chart colors
├── docs/
│   ├── data_dictionary.md        # field-level documentation, raw + processed
│   └── powerbi_guide.md          # step-by-step Power BI Desktop build guide
├── requirements.txt
├── .gitignore
└── README.md
```

## Environment Setup (Windows)

A virtual environment is recommended — not strictly mandatory for a one-off script, but standard
practice for any project you're putting on GitHub or reusing later, since it pins exact dependency
versions and keeps this project isolated from your other Python/TensorFlow setups.

```powershell
# 1. From inside the walmart_sales_analysis folder
python -m venv venv

# 2. Activate it
venv\Scripts\Activate.ps1
# If PowerShell blocks this with an execution-policy error, run once:
# Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
# then retry the activate command.

# 3. Install dependencies
pip install -r requirements.txt

# 4. Register the Jupyter kernel for this venv
python -m ipykernel install --user --name walmart-venv --display-name "Python (walmart-venv)"

# 5. Launch Jupyter and select the "Python (walmart-venv)" kernel
jupyter notebook
```

To leave the environment when you're done: `deactivate`.

## How to Run

1. Set up the environment above.
2. Open `notebooks/01_walmart_sales_eda.ipynb` and run all cells top to bottom.
3. This regenerates `data/processed/walmart_sales_cleaned.csv` and all charts in `reports/figures/`.
4. Follow `docs/powerbi_guide.md` to build the Power BI dashboard on top of the cleaned CSV.

## Key Insights

| # | Insight |
|---|---|
| 1 | Revenue is concentrated: the **top 5 of 45 stores generate ~21.5%** of total network sales |
| 2 | Store performance varies **~8x** between the strongest and weakest store |
| 3 | Holiday weeks lift average sales **~7.8%** (Welch's t-test, p = 0.0076) — statistically significant, but a **small effect** (Cohen's d = 0.145); both can be true at once |
| 4 | An **A/A sanity-check test** (random 50/50 split, no real grouping) correctly found no significant difference (p = 0.925) — confirms the testing methodology isn't generating false positives |
| 5 | **December** is the strongest sales month, **January** the weakest — a ~38% gap |
| 6 | Macroeconomic indicators (CPI, unemployment, fuel price, temperature) are **weak predictors** of sales (\|r\| < 0.11) |
| 7 | The true Christmas sales spike falls the week *before* the week `Holiday_Flag` actually marks as the Christmas holiday — a labeling quirk worth knowing before trusting the flag at face value |

Full reasoning, hypotheses, effect sizes, confidence intervals, and power analysis for each test are
in `notebooks/01_walmart_sales_eda.ipynb` (Section 11 covers A/B + A/A testing in full).

### Sample Charts

`reports/figures/05_top_bottom_stores.png` — store performance spread
`reports/figures/06_holiday_impact.png` — holiday vs non-holiday sales distribution
`reports/figures/07_correlation_heatmap.png` — sales vs economic indicators

## Power BI Dashboard

Built on `data/processed/walmart_sales_cleaned.csv`, four pages:

1. **Executive Overview** — KPI cards, total sales trend, monthly seasonality
2. **Store Performance** — top/bottom store leaderboard, store-tier breakdown
3. **Holiday & Seasonality** — holiday lift, year-over-year seasonal curve
4. **Economic Drivers** — sales vs CPI/unemployment/fuel price/temperature scatterplots

Full build steps, exact DAX formulas, and a polish checklist: `docs/powerbi_guide.md`.

*(Add your exported PDF or page screenshots here once built, e.g.
`![Executive Overview](reports/dashboard_overview.png)`)*

## Possible Extensions

- Split `Store`/`Store_Tier` into a proper `Dim_Store` dimension table (star schema) instead of one
  flat table
- Add a forecasting notebook (e.g. Prophet or SARIMA) predicting next-quarter sales per store
- Geocode store regions (not in the source data) to enable a map visual in Power BI

## Author

Adnan — B.Tech AI & ML, Asansol Engineering College
