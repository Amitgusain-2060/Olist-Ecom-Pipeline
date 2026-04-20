# Olist-Ecom-Pipeline
An automated ETL pipeline built on the Olist Brazilian E-commerce dataset. This project implements a Medallion Architecture (Bronze, Silver, Gold) to transform raw, messy CSV data into high-performance Delta tables for business analytics.

📌 Project Overview
This project builds a scalable Data Lakehouse using the Olist Brazilian E-Commerce dataset. The goal is to move data through a Medallion Architecture, transforming raw, unstructured data into clean, business-ready insights.

🛠 Tech Stack
Language: Python (PySpark)

Platform: Databricks

Storage: Delta Lake (ACID transactions, Time Travel)

Architecture: Medallion (Bronze/Silver/Gold)

Version Control: Git & GitHub

🏗 Key Features
Automated Ingestion: Efficiently loading raw CSV data into the Bronze Layer while preserving data lineage.

Data Quality & Cleaning: Handling null values, enforcing schemas, and deduplicating records in the Silver Layer.

Analytical Modeling: Creating aggregated tables in the Gold Layer for KPIs like "Monthly Revenue" and "Top Performing Categories."

Optimized Performance: Using Spark optimization techniques to ensure fast processing of large datasets.

📁 Repository Structure

notebooks/: Databricks notebooks for each pipeline stage.

data/: (Optional) Sample data or links to the Olist source.

docs/: Architecture diagrams and logic documentation.
