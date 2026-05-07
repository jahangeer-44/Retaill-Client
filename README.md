# Retail Sales Data Warehouse ETL & Data Quality Validation Project

## 📌 Project Overview

This project implements an end-to-end **Retail Sales Data Warehouse** using **AWS S3 + Databricks + SQL** with complete ETL processing, data quality validation, incremental loading, archival strategy, and SCD Type 2 handling.

The solution simulates a real-world enterprise retail pipeline where source CSV files arrive periodically and are processed through multiple layers:

```text
S3 Landing Zone → Bronze (Raw) → Silver (Clean) → Gold (Warehouse) → Archive
```

---

# 🎯 Business Problem

A retail organization needs to:

* Process customer, product, store, and sales transaction data
* Maintain historical customer changes using SCD Type 2
* Perform full and incremental loads
* Archive previous files whenever new files arrive
* Validate ETL pipeline quality end-to-end
* Monitor failures and data quality issues

---

# 🛠️ Tech Stack

## Cloud & Storage

* AWS S3
* Databricks (Unity Catalog / SQL / PySpark)

## Data Processing

* SQL
* PySpark
* Databricks Workflows

## Version Control

* GitHub

---

# 📂 Source Files

| File                       | Description             |
| -------------------------- | ----------------------- |
| customers_src.csv          | Customer master data    |
| products_src.csv           | Product master data     |
| stores_src.csv             | Store master data       |
| sales_transactions_src.csv | Sales fact transactions |

### Naming Convention:

```text
<filename>_DDMMYYYY_HHMMSS.csv
```

### Example:

```text
customers_src_20042026100105.csv
```

---

# 🏗️ Architecture Layers

## 1️⃣ Landing Layer (S3)

* Raw source file arrival zone
* Latest file remains active
* Previous file archived after successful processing

---

## 2️⃣ Bronze Layer (Raw)

* Initial ingestion from landing
* No transformations
* Source preservation for audit/debugging

### Example:

* bronze.customers_raw
* bronze.products_raw
* bronze.stores_raw
* bronze.sales_raw

---

## 3️⃣ Silver Layer (Cleaned)

Data transformations include:

* Duplicate removal
* Email lowercase conversion
* Trim spaces
* Standardized dates
* Latest incremental merge
* Data quality validation

### Example:

* silver.customers_clean
* silver.products_clean
* silver.stores_clean
* silver.sales_clean

---

## 4️⃣ Gold Layer (Warehouse)

### Dimension Tables:

* dim_customer (SCD Type 2)
* dim_product
* dim_store

### Fact Table:

* fact_sales

---

# 🧠 SCD Type 2 Implementation (DimCustomer)

## Handles:

* City changes
* Address changes
* Email changes

## Logic:

### If customer changes:

* Old row → IsActive = 0
* EndDate updated
* New row inserted
* StartDate = current load date

### Fields:

* CustomerSK
* CustomerID
* CustomerName
* Email
* City
* Address
* StartDate
* EndDate
* IsActive

---


# 📦 Archival Strategy

Whenever a new file arrives:

```text
Old File → Archive Folder
New File → Landing Folder
```

## Validation Rules:

* Only latest file in active zone
* Previous files archived
* Naming convention validated
* Failure detection supported

---

# 🧪 ETL Testing Scope

## Source-to-Target Testing

* Row count validation
* Schema validation
* Data type validation
* Column mapping validation

## Data Transformation Testing

* Trim checks
* Lowercase checks
* Proper case validation
* Derived Amount validation

## Data Quality Testing

* Duplicate detection
* Null checks
* Invalid records rejection
* Referential integrity checks

## SCD Type 2 Testing

* Active/inactive rows
* Historical tracking
* StartDate/EndDate validation

## Incremental + Archival Testing

* New file detection
* Old file archival
* Latest file presence

---

# 📋 Deliverables Covered

* ETL Pipeline Design
* Bronze / Silver / Gold Implementation
* Incremental Load Framework
* Archival Design
* SQL Validation Queries
* Data Quality Checks

---

## Databricks Workflow:

```text
Bronze → Silver → Gold → Fact → Archive → Validation
```

## Features:

* Scheduled pipeline execution
* Failure logging
* Alert generation
* Automated archival



# 📈 Key Learnings

* Enterprise ETL architecture
* Real-world data validation
* Incremental processing
* SCD Type 2 implementation
* Cloud data engineering
* Databricks + AWS integration
* QA-focused ETL testing


# 🚀 Future Enhancements

* Full CI/CD deployment
* Real-time ingestion
* Dashboard reporting (Power BI / Tableau)
* Automated email alerts
* Data observability tools
* Advanced orchestration


---

# 🏁 Final Status

```text
✔ Full Load
✔ Incremental Load
✔ Bronze Layer
✔ Silver Layer
✔ Gold Layer
✔ Fact Table
✔ SCD Type 2
✔ Archival
✔ Validation
```
