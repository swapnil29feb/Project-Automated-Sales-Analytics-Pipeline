# Project: Automated Sales Analytics Pipeline

## 🔧 Tech Stack
- Python
- Apache Airflow
- AWS S3
- AWS Glue
- MySQL
- Snowflake
- AWS EC2 (Linux server for deployment)

## 📈 Problem Statement
A company receives daily sales data from multiple regions in JSON format to an S3 bucket. The data must be validated, cleaned, enriched using metadata from MySQL, transformed using AWS Glue, and finally loaded into Snowflake for analytics. The process must be automated with Apache Airflow.

## 📁 Project Structure
```
sales-analytics-pipeline/
├── dags/
│   └── etl_sales_pipeline.py          # Airflow DAG for orchestration
├── glue_jobs/
│   └── transform_sales_data.py        # AWS Glue job for transformation
├── mysql_metadata/
│   └── init_product_store.sql         # SQL script to setup MySQL metadata
├── s3_data/
│   └── raw_sales_data/                # Folder for JSON data
├── scripts/
│   ├── data_validation.py             # S3 data validation script
│   └── snowflake_loader.py            # Snowflake load script
└── README.md                          # This file
```

## 📝 Data Format
### Raw Sales Data (S3 `raw_sales_data/*.json`)
```json
[
  {
    "order_id": "ORD12345",
    "product_id": "PROD1001",
    "store_id": "STORE001",
    "sale_date": "2025-08-01",
    "amount": 1500
  },
  {
    "order_id": "ORD12346",
    "product_id": "PROD1002",
    "store_id": "STORE001",
    "sale_date": "2025-08-01",
    "amount": 950
  }
]
```

### MySQL Metadata
- **products**
  ```sql
  CREATE TABLE products (
    product_id VARCHAR(20) PRIMARY KEY,
    product_name VARCHAR(255),
    category VARCHAR(100)
  );
  ```
- **stores**
  ```sql
  CREATE TABLE stores (
    store_id VARCHAR(20) PRIMARY KEY,
    store_name VARCHAR(255),
    region VARCHAR(100)
  );
  ```

## ⚙️ Setup Instructions

### Step 1: Setup EC2 Linux Instance
- Launch EC2 with Ubuntu 22.04 or Amazon Linux 2
- Install Docker, Python 3.10+, Git
- Clone this repo
- Install Apache Airflow (use Docker or system install)

### Step 2: S3 Bucket
- Create a bucket: `sales-pipeline-data`
- Create folder: `raw_sales_data/`
- Upload sample JSON files (structure above)

### Step 3: MySQL Setup
- Use RDS or local MySQL
- Run `init_product_store.sql` to create and populate metadata tables

### Step 4: Snowflake
- Create database: `SALES_DB`
- Create schema: `PUBLIC`
- Create table: `SALES`
- Create external stage or use `COPY INTO` for load

### Step 5: AWS Glue Job
- Create Glue job named `transform_sales_data`
- Use script from `glue_jobs/transform_sales_data.py`

### Step 6: Environment Variables
Set these in Airflow:
```bash
export SF_USER=your_user
export SF_PASSWORD=your_password
export SF_ACCOUNT=your_account
```

### Step 7: Run Airflow
```bash
cd sales-analytics-pipeline
airflow db init
airflow users create ...
airflow webserver &
airflow scheduler &
```

## 🔁 Workflow Steps
1. Detect new files in S3
2. Validate structure & required fields
3. Trigger Glue job to transform and enrich
4. Load output to Snowflake
5. Monitor & retry on failure

## ✅ Use Cases Covered
- Schema validation
- Deduplication
- Data enrichment from MySQL
- Automated scheduling with Airflow
- Snowflake ingestion

## 📌 Test Scenarios
| Scenario | Description |
|----------|-------------|
| ✅ Valid File | Valid JSON file is processed end-to-end |
| ❌ Missing Field | Job fails on missing `order_id` |
| 🔁 Duplicate Order | Duplicates are dropped in Glue step |
| 🧬 Schema Drift | Alerts if unknown field is present |

## 📊 Sample Resume Bullet
> Built and deployed an end-to-end data pipeline using Python, AWS Glue, MySQL, S3, Airflow, and Snowflake on EC2. Automated ingestion, transformation, and loading of daily sales data with data validation, deduplication, and quality checks.

---
