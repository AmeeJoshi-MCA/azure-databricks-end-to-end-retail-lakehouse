# Azure Databricks End-to-End Retail Lakehouse

## Overview

This repository demonstrates an **end-to-end data engineering project on Azure Databricks** using the **Medallion Architecture (Bronze → Silver → Gold)**. The pipeline ingests raw retail data, refines it incrementally, and delivers **analytics-ready dimension and fact tables** using **Delta Lake** and **Unity Catalog**.

---

## Business Use Case

Retail analytics platform enabling:

* Sales and revenue analysis
* Product price and category history tracking
* Customer-based reporting
* Time-based trend analysis (monthly / yearly)

---

## Architecture

![1748691271296](https://github.com/user-attachments/assets/27210726-dda7-4d18-b29d-6e5d1f6cdce0)


### Key Characteristics

* Incremental ingestion using **Databricks Auto Loader**
* Parallel Silver and Gold processing
* Append-only fact table design
* SCD Type-1 and SCD Type-2 dimensional modeling

---

## Tech Stack

* **Azure Databricks**
* **Delta Lake**
* **Unity Catalog**
* **PySpark & Spark SQL**
* **Auto Loader**
* **Azure Data Lake Storage (ADLS Gen2)**

---

## Data Layers

### Bronze Layer (Raw Ingestion)

* Incremental file ingestion using Auto Loader
* Schema inference and append-only storage
* No transformations

### Silver Layer (Refined Data)

* Data cleansing and standardization
* Deduplication by business keys
* Entity-specific transformations

Tables:

* `silver_customers`
* `silver_orders`
* `silver_products`

---

## Gold Layer (Business-Ready)

### DimCustomers – SCD Type 1

* Stores latest customer attributes
* Updates overwrite existing values
* No historical tracking

### DimProducts – SCD Type 2

* Tracks historical product changes
* Uses effective start/end dates and current flag
* Preserves full change history

### FactOrders

* Grain: **one row per product per order**
* Append-only transactional fact table
* Stores only business keys and measures
* Dimensions joined at query time

<img width="1170" height="478" alt="image" src="https://github.com/user-attachments/assets/50f6c605-e67b-4280-9f02-ea833909c3be" />

---

## Data Modeling

* **Star Schema**
* Fact table at the center
* Dimensions joined at query time
* SCD handling aligned with business requirements

| Table        | Type      | Strategy    |
| ------------ | --------- | ----------- |
| DimCustomers | Dimension | SCD Type 1  |
| DimProducts  | Dimension | SCD Type 2  |
| FactOrders   | Fact      | Append-only |

---

## Orchestration Strategy

* Parameterized notebooks
* Bronze must complete before Silver
* Silver must complete before corresponding Gold
* Gold dimensions and fact tables run **in parallel**
* FactOrders does not depend on dimensions at load time

---

## Key Skills Demonstrated

* Azure Databricks
* End-to-end data pipeline design
* Medallion Architecture
* Delta Lake MERGE
* SCD Type 1 & Type 2
* Fact table modeling
* Lakehouse best practices

---

## Conclusion

This project demonstrates a **production-style Azure Databricks Lakehouse implementation** using industry-standard data engineering practices. It showcases the complete lifecycle of data—from raw ingestion to analytics-ready datasets—implemented through a scalable **Medallion Architecture**.

By combining **Auto Loader–based incremental ingestion**, **Delta Lake transactional guarantees**, **Unity Catalog governance**, and **star schema modeling**, the solution delivers a robust and maintainable analytics platform. The use of **SCD Type 1 for Customers**, **SCD Type 2 for Products**, and an **append-only FactOrders table** reflects real-world business requirements and best practices.

The pipeline is intentionally designed to be **free-tier compatible**, avoiding paid features while still adhering to enterprise-grade design principles. This makes the project both **practically executable** , demonstrating strong understanding of modern data engineering concepts, trade-offs, and architectural decisions.

---
