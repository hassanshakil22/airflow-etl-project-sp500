# 📈 S&P 500 ETL Pipeline — Airflow | Snowflake | S3 | YFinance

> **Enterprise-grade ETL pipeline** orchestrated with **Apache Airflow**, designed to extract, transform, and load daily S&P 500 stock data from Yahoo Finance into **Snowflake**, with intermediate data archival to **AWS S3**.

---

## 🚀 Overview

This project automates the full **data ingestion workflow** for S&P 500 market data — from raw extraction to Snowflake warehousing — showcasing real-world data engineering practices in a production-grade DAG environment.

### 🔁 Pipeline Stages
1. **Extract (Wikipedia + YFinance)**  
   - Scrapes the latest S&P 500 tickers from Wikipedia.  
   - Pulls daily OHLCV (Open, High, Low, Close, Volume) data from Yahoo Finance.

2. **Transform (Pandas)**  
   - Cleans and formats the data.  
   - Computes daily close change (`close_change`) and close percentage change (`close_pct_change`).  
   - Outputs a timestamped CSV file.

3. **Load (AWS S3 + Snowflake)**  
   - Uploads the transformed CSV file to an S3 bucket.  
   - Loads the same dataset into a Snowflake table using a temporary stage and COPY INTO command.

4. **Orchestrate (Airflow)**  
   - Entire process managed via a daily-scheduled Airflow DAG with robust XCom communication and task dependencies.

---

## ⚙️ Tech Stack

| Layer | Tool / Library | Purpose |
|:------|:----------------|:--------|
| Orchestration | **Apache Airflow** | Task scheduling, dependency management |
| Storage | **AWS S3** | Data backup and archival |
| Data Warehouse | **Snowflake** | Final data storage for analytics |
| Extraction | **Requests, Pandas, YFinance** | Data scraping and collection |
| Transformation | **Pandas** | Data cleaning and enrichment |
| Infrastructure | **Docker Compose** | Containerized Airflow environment |

---

## 🧠 Architecture Diagram

Wikipedia (Tickers)
│
▼
Yahoo Finance (Price Data)
│
▼
Transform
(Compute Δ Close)
│
├──► S3 (Backup)
│
└──► Snowflake (Final Table)



---

## 📦 Snowflake Table Schema

```sql
CREATE OR REPLACE TABLE AIRFLOW_ETL_DATABASE.PUBLIC.SP500_TABLE (
    DATE DATE,
    SYMBOL VARCHAR(16777216),
    OPEN FLOAT,
    HIGH FLOAT,
    LOW FLOAT,
    CLOSE FLOAT,
    VOLUME FLOAT,
    CLOSE_CHANGE FLOAT,
    CLOSE_PCT_CHANGE FLOAT
);


🧱 Local Development Setup
# 1️⃣ Clone the repo
git clone https://github.com/<your-username>/sp500-airflow-etl.git
cd sp500-airflow-etl

# 2️⃣ Start Airflow stack
docker compose up -d

# 3️⃣ Access Airflow UI
http://localhost:8080

# 4️⃣ (Optional) Run ETL locally without Airflow
python etl_local.py
