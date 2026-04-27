# Olist E-commerce DE Pipeline

End-to-end data engineering pipeline on 100k+ Brazilian e-commerce orders.

## Stack
PySpark · Databricks Community Edition · Delta Lake · Unity Catalog · GitHub

## Architecture
Raw CSVs → Bronze (Delta) → Silver (cleaned + joined) → Gold (KPIs)

## Dataset
Kaggle Olist Brazilian E-commerce — 9 CSVs, 112,650 order items

## What I built

### Bronze
- Ingested all 9 CSVs into Unity Catalog Delta tables
- Added audit columns (_ingest_at, _source) for lineage tracking
- Zero transformations — raw data preserved exactly as source

### Silver  
- Cast timestamp columns, stripped whitespace from city/state
- Deduplicated geolocation: 1,000,163 rows → 20,549 unique zip codes
- Aggregated payments and reviews before joining to prevent row fanout
- Built master_sales_data: 112,650 rows × 40 cols joining 7 tables
- Used left joins throughout to preserve all order_items grain

### Gold KPIs
| Table | Business Question |
|-------|------------------|
| revenue_by_state | Which states drive most revenue by month? |
| delivery_sla | Which states have worst late delivery rates? |
| seller_performance | Which sellers have best revenue + review scores? |

## Key Engineering Decisions
- **left joins everywhere** — inner joins silently drop unmatched rows,
  corrupting revenue totals in Gold
- **aggregate before join** — payments has multiple rows per order (installments),
  joining directly causes fanout and inflates row counts
- **geolocation dedup** — groupBy zip + avg(lat/lng) before joining,
  direct join would multiply every customer row by ~50x
- **writeTo().createOrReplace()** — used instead of .write.mode("overwrite")
  because Databricks Serverless does not support path-based writes

## How to Run
1. Upload CSVs to `/Volumes/workspace/olist_bronze/csvs/`
2. Run `01_Bronze_Ingestion.ipynb`
3. Run `02_Silver_Transformation.ipynb`
4. Run `03_Gold_Analysis.ipynb`
