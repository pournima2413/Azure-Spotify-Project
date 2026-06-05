# 🎵 Spotify End-to-End Azure Data Engineering Project

> A production-grade cloud data platform built on Azure — ingesting, transforming, governing, and serving Spotify streaming data using Medallion Architecture.

---

## 📌 Project Overview

This project demonstrates an end-to-end **Azure Data Engineering pipeline** that processes Spotify streaming data through a Medallion Architecture (Bronze → Silver → Gold). It covers metadata-driven ingestion, incremental CDC loading, PySpark transformations, Delta Live Tables automation, Unity Catalog governance, and analytics serving via Azure Synapse Analytics.

---

## 🏗️ Architecture

![Architecture Diagram](architecture/Data_Lake.jpg)

> **Flow:** SQL DB + GitHub → Azure Data Factory → Bronze Data Lake → PySpark (Silver) → Delta Live Tables (Gold) → Azure Synapse Warehouse
> Security layer: Azure Key Vault + Microsoft Entra ID

---

## 🛠️ Technology Stack

| Category | Technology |
|---|---|
| Data Ingestion | Azure Data Factory |
| Source Database | Azure SQL Database |
| Storage | Azure Data Lake Storage Gen2 |
| Processing Engine | Azure Databricks |
| Transformation | PySpark, Spark Streaming |
| Pipeline Automation | Delta Live Tables (DLT) |
| Data Governance | Unity Catalog |
| Data Modeling | Star Schema, SCD |
| Incremental Loading | Change Data Capture (CDC) |
| Version Control | GitHub |
| CI/CD | Databricks Asset Bundles |
| Serving Layer | Azure Synapse Analytics |
| Security | Microsoft Entra ID, Azure Key Vault |

---

## 📂 Project Structure

```
spotify-azure-data-engineering/
│
├── adf/
│   ├── initial_load_pipeline/
│   ├── incremental_pipeline/
│   └── metadata_config/
│
├── sql/
│   ├── spotify_initial_load.sql
│   └── spotify_incremental_load.sql
│
├── databricks/
│   ├── bronze/
│   ├── silver/
│   ├── gold/
│   └── delta_live_tables/
│
├── metadata/
│   ├── loop_input.txt
│   └── cdc.json
│
├── architecture/
│   └── architecture_diagram.png
│
└── README.md
```

---

## 📥 Data Ingestion — Azure Data Factory

ADF orchestrates all data movement from source systems into the Bronze layer using a metadata-driven framework.

**Key features:**
- Metadata-driven pipelines with dynamic datasets
- Parameterized pipelines for reusability
- Initial full loads and incremental CDC loads
- JSON and Parquet file format support

---

## 🔄 Incremental Loading — CDC

Incremental data loading is implemented using Change Data Capture (CDC) driven by a metadata configuration file.

**CDC config (`cdc.json`):**
```json
[
  { "table": "DimUser",   "cdc_col": "updated_at" },
  { "table": "DimTrack",  "cdc_col": "updated_at" },
  { "table": "DimDate",   "cdc_col": "updated_at" },
  { "table": "DimArtist", "cdc_col": "updated_at" },
  { "table": "FactStream","cdc_col": "updated_at" }
]
```

**Incremental pipeline steps:**
1. Lookup previous CDC watermark timestamp
2. Read current metadata configuration
3. Execute incremental SQL query against source
4. Copy only changed records to the Bronze layer
5. Update CDC control file with new watermark
6. Delete temporary files when no changes are detected

---

## 🥉 Bronze Layer — Raw Ingestion

Stores data exactly as received from source systems with no transformations applied.

- Preserves source data for replayability and auditing
- Acts as the single source of truth for all downstream processing
- Stored in **Azure Data Lake Storage Gen2**

---

## 🥈 Silver Layer — Cleansed Data

Implemented using **Azure Databricks + PySpark**.

**Transformations applied:**
- Schema validation and enforcement
- Null handling and deduplication
- Data type conversion and standardization
- Data quality checks
- Incremental processing with Delta Lake

---

## ⭐ Gold Layer — Analytics-Ready

Business transformations applied via **Delta Live Tables (DLT)**.

**Outputs:**
- KPI aggregations
- Listening, user, track, and artist analytics
- Optimized datasets for BI consumption

---

## 📐 Data Modeling — Star Schema

```
         DimUser
            │
DimArtist ──┼── FactStream ──┬── DimTrack
            │
          DimDate
```

| Type | Tables |
|---|---|
| Fact | FactStream |
| Dimensions | DimUser, DimTrack, DimArtist, DimDate |

Slowly Changing Dimensions (SCD) are implemented to track historical changes in dimension data.

---

## 🚀 Delta Live Tables

DLT manages the entire transformation pipeline automatically:

- Automated dependency resolution between transformations
- Built-in data quality enforcement with expectations
- Automatic lineage tracking
- Reduced maintenance overhead vs. manual Spark jobs

---

## 🔐 Governance & Security

| Component | Purpose |
|---|---|
| Unity Catalog | Data governance, access control, lineage, metadata management |
| Microsoft Entra ID | Identity and authentication |
| Azure Key Vault | Secrets and credentials management |
| Managed Identities | Secure service-to-service communication |

---

## 🔄 CI/CD Pipeline

- **GitHub** — Source control, branch management, and collaboration
- **Databricks Asset Bundles** — Infrastructure as code, automated deployments, multi-environment management

---

## 📈 Serving Layer — Azure Synapse Analytics

Azure Synapse provides SQL-based access to Gold layer datasets for reporting and business analytics consumption.

---

## 💡 Key Learnings

- Designed and implemented a full **Medallion Architecture** on Azure
- Built **metadata-driven pipelines** that scale without code changes
- Applied **CDC patterns** for efficient incremental data processing
- Used **Delta Live Tables** to automate and govern transformation pipelines
- Implemented **Unity Catalog** for enterprise-grade data governance
- Practiced **CI/CD** with GitHub and Databricks Asset Bundles
- Debugged real-world issues: pipeline failures, permission errors, schema mismatches

---

## 🙏 Acknowledgements

Special thanks to **Ansh Lamba** for creating a comprehensive, practical Azure Data Engineering project walkthrough that bridges the gap between theory and real-world implementation.

---

## 👩‍💻 Author

**Pournima Kamble**
Master's Student, Computer Science — Cleveland State University

Interests: Data Engineering · Cloud Computing · Big Data · AI & Machine Learning

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](https://linkedin.com)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=flat&logo=github)](https://github.com)

---

⭐ If you found this project helpful, please star the repository!

---

## 🧰 Tech Stack Badges

![Azure Data Factory](https://img.shields.io/badge/Azure%20Data%20Factory-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Azure Databricks](https://img.shields.io/badge/Azure%20Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![Azure Synapse](https://img.shields.io/badge/Azure%20Synapse%20Analytics-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Azure SQL](https://img.shields.io/badge/Azure%20SQL%20Database-CC2927?style=for-the-badge&logo=microsoftazure&logoColor=white)
![ADLS Gen2](https://img.shields.io/badge/Azure%20Data%20Lake%20Gen2-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![Spark Streaming](https://img.shields.io/badge/Spark%20Streaming-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![Delta Live Tables](https://img.shields.io/badge/Delta%20Live%20Tables-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![Unity Catalog](https://img.shields.io/badge/Unity%20Catalog-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![CI/CD](https://img.shields.io/badge/CI%2FCD-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![Azure Key Vault](https://img.shields.io/badge/Azure%20Key%20Vault-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Microsoft Entra ID](https://img.shields.io/badge/Microsoft%20Entra%20ID-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
