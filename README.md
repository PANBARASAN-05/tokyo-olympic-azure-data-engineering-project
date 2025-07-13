# 🏅 Tokyo Olympic Data Analytics Pipeline

![Azure](https://img.shields.io/badge/Azure-Databricks-blue) ![Spark](https://img.shields.io/badge/Apache%20Spark-Data%20Processing-orange) ![Power BI](https://img.shields.io/badge/PowerBI-Dashboard-yellow)

> 🚀 **End-to-end Data Engineering & Analytics project on the Tokyo Olympic dataset using Azure Data Lake, Data Factory, Databricks, Synapse, and Visualization tools.**

---

## 📚 Table of Contents
- [About](#about)
- [Solution Architecture](#solution-architecture)
- [Tech Stack](#tech-stack)
- [Data Pipeline Workflow](#data-pipeline-workflow)
- [Sample Transformations](#sample-transformations)
- [Results](#results)
- [How to Run](#how-to-run)
- [Future Improvements](#future-improvements)
- [License](#license)

---

## ✨ About
This project implements a complete data pipeline for the **Tokyo Olympics dataset** on the Azure cloud stack.  
It automates:
- Data ingestion via Data Factory
- Transformation & analysis using Databricks (PySpark)
- Storing structured data in Azure Data Lake
- Loading into Synapse Analytics for downstream BI dashboards (Power BI, Looker Studio, Tableau).

---

## 🏗️ Solution Architecture

![Architecture](./assets/architecture.png)

- **Data Factory**: Extracts raw CSV files into Data Lake Gen2.  
- **Databricks**: Processes, cleans & transforms data using PySpark.  
- **Data Lake Gen2**: Stores raw & transformed datasets.  
- **Synapse Analytics**: Performs further analysis and exposes datasets for visualization.  
- **Power BI / Looker Studio / Tableau**: Connects to Synapse for dynamic dashboards.

---

## ⚙️ Tech Stack
| Tool                        | Usage                          |
|------------------------------|-------------------------------|
| **Azure Data Factory**        | Ingest raw Olympic CSV data   |
| **Azure Data Lake Gen2**      | Storage for raw & processed data |
| **Azure Databricks (PySpark)**| Data cleaning & transformations |
| **Azure Synapse Analytics**   | Query & prepare analytical data |
| **Power BI, Looker Studio, Tableau** | Interactive dashboards |

---

## 🔄 Data Pipeline Workflow

1. **Ingest:**  
   - Data Factory moves raw `.csv` files into `tokyo-olympic-data` container on ADLS Gen2.

2. **Mount & Transform:**  
   - Databricks mounts ADLS securely using OAuth (service principal).
   - Loads datasets (`athletes`, `coaches`, `teams`, `entriesgender`, `medals`).
   - Casts data types, computes averages, ranks top medal countries.

3. **Store:**  
   - Writes clean data back to Data Lake under `/transformed-data`.

4. **Analytics:**  
   - Synapse reads processed data for reporting.

5. **Visualize:**  
   - Dashboards created on Power BI, Looker Studio, Tableau.

---

## 🔥 Sample Transformations

### ✅ Cast columns & calculate gender ratio
```python
entriesgender = entriesgender.withColumn("Female", col("Female").cast(IntegerType())) \
    .withColumn("Male", col("Male").cast(IntegerType())) \
    .withColumn("Total", col("Total").cast(IntegerType()))

average_entries_by_gender = entriesgender.withColumn(
    'Avg_Female', entriesgender['Female'] / entriesgender['Total']
).withColumn(
    'Avg_Male', entriesgender['Male'] / entriesgender['Total']
)

