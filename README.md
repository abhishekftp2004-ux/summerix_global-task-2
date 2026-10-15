# Task 2 — Business Intelligence Sales Analytics Dashboard

## Overview
This project implements the Task 2 brief: SQL for analytics, PostgreSQL schema, Python database connectivity, advanced Pandas, data wrangling, feature engineering, Plotly visualizations, BI KPIs and an interactive Streamlit dashboard.

**Dataset disclosure:** `datasets/raw/sales.csv` is synthetic practice data created for this project. Do not present its numbers as actual company results.

## Folder structure
```text
sales_bi_dashboard_task2/
├── datasets/raw/sales.csv
├── datasets/processed/sales_clean.csv
├── sql/schema.sql
├── sql/analytics_queries.sql
├── database/.env.example
├── etl.py
├── dashboard/app.py
├── notebooks/task2_sales_analytics.ipynb
├── reports/eda_report.md
├── reports/figures/
├── documentation/TASK2_FULL_EXPLANATION.pdf
├── requirements.txt
└── README.md
```

## Install and run
Python 3.10+ recommended (the brief lists Python 3.13).
```bash
python -m venv .venv
# Windows PowerShell
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python etl.py
streamlit run dashboard/app.py
```
The dashboard can run locally using the bundled CSV, so PostgreSQL is optional for the initial demonstration.

## PostgreSQL setup (optional database integration)
1. Install PostgreSQL and pgAdmin 4.
2. Create a database named `salesdb`.
3. Configure `DATABASE_URL` using `database/.env.example` as a template. Never commit real credentials.
4. Run `sql/schema.sql` in the `salesdb` Query Tool to create normalized tables and indexes.
5. The ETL script can load a `sales_staging` table when `DATABASE_URL` is set. The provided schema and analytics SQL are examples; production loading into normalized foreign-key tables needs a dimension-first load process.

Example environment variable in PowerShell for the current terminal:
```powershell
$env:DATABASE_URL="postgresql+psycopg://postgres:YOUR_PASSWORD@localhost:5432/salesdb"
python etl.py
```

## Data dictionary
- `order_id`: sales order identifier
- `order_date`: order date
- `customer_id`: anonymized customer identifier
- `region`: sales region
- `product`: product name
- `category`: product category
- `quantity`: units sold
- `unit_price`: unit price
- `discount`: fraction between 0 and 1
- `gross_sales`: quantity × unit price
- `revenue`: gross sales × (1 − discount)
- `year`, `month`, `day_of_week`, `order_month_num`: derived date features
- `discount_band`: derived discount category

## Cleaning and assumptions
- Parse dates and numeric fields explicitly.
- Remove exact duplicates.
- Exclude rows with missing/non-positive quantity or missing/negative unit price.
- Fill missing region with `Unknown`.
- Fill missing discount with zero and clip it to [0,1].
- Calculate gross sales, net revenue and date features.
These are demonstration rules; real business policy should be confirmed with a data owner.

## Dashboard features
- Sidebar filters for region, product and year
- KPI cards for revenue, gross sales, unique orders, customers and average sales-line revenue
- Monthly trend, product performance, regional performance and category share
- Top customer table
- Filtered data table and CSV export

## SQL topics demonstrated
- SELECT, GROUP BY, ORDER BY
- JOIN
- CTE
- DENSE_RANK window function
- VIEW
- Indexes and constraints
