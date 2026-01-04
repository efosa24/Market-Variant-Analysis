# Market-Variant-Analysis
This project is an R analytics workflow that connects to **Google BigQuery**, calculates **variance metrics** comparing **current-month projections vs full-month updated forecasts**, and generates **lollipop charts** for executive reporting.

It is designed to support performance monitoring across **markets and market categories** (Priority, Growth, Other) and produce **ready-to-use PNG visuals** for weekly/monthly leadership reports.

---

## What This Script Does

### 1) Connects to BigQuery and loads metric data
- Uses `bigrquery` + `DBI` to query a table from BigQuery
- Standardizes and formats:
  - Market names (`str_to_title`)
  - Calendar date (`parse_date_time`)

### 2) Builds market categories (Priority / Growth / Other)
Maps markets into reporting categories using `case_when()`.

Example categories:
- **Growth**: Florida And Gulf Coast, Indiana, Tennessee, Texas  
- **Priority**: Illinois, Michigan, Wisconsin  
- **Other**: Alabama, Kansas, Oklahoma, Baltimore, Binghamton

### 3) Filters to a reporting month
Filters to the **current reporting month** using:
```r
filter(floor_date(calendar_date, "month") == floor_date(ymd("2023-4-1")))
4) Calculates variance (Projection vs Target)
Computes variance using:

Volume metrics: uses numerator fields directly

Aggregate metrics: computes ratios (num/den)

Creates:

Target

Adsi_Projection

ascen_var = (Target - Adsi_Projection) / Target * 100

5) Generates executive-ready lollipop charts
Creates and saves the following visuals:

(A) Market Category × Metric Variance Lollipop

Shows average variance for each Metric within each Market Category

Output file:

Market Category.png

(B) Market × Metric Variance Lollipops by Category

Creates one chart per category (Priority, Growth, Other)

Output files:

Priority.png

Growth.png

Other.png

(C) Market Variance by Category

Variance by Market inside each Market Category

Output file:

Market category(market).png
