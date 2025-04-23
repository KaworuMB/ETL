# ETL

This project implements an ETL (Extract, Transform, Load) process using AWS services for automatic processing and analysis of transaction data.

## 🧩 Project Overview

The pipeline loads raw data (CSV), cleans and transforms it using AWS Glue, stores it in Parquet format in Amazon S3, and enables querying through Amazon Athena.

---

## 🚀 Architecture

- **Amazon S3** — Storage for raw and processed data  
- **AWS Glue Crawler** — Automatically infers schema and creates a table in the Glue Data Catalog  
- **AWS Glue Job** — Cleans and transforms the data  
- **Amazon Athena** — Executes SQL queries on the processed data  
- **IAM Role** — Grants Glue and Athena access to required S3 buckets  

---

## 📁 Data Structure

**Raw Input (CSV):**
- `UserId` — Unique user identifier  
- `TransactionId` — Unique transaction identifier  
- `TransactionTime` — Date and time of the transaction  
- `ItemCode` — Item code  
- `ItemDescription` — Description of the item  
- `NumberOfItemPurchased` — Number of items purchased  
- `CostPerItem` — Cost per item  
- `Country` — Country where the item was purchased  

**Processed Output (Parquet):**
- All columns are cleaned and standardized  
- Data types are casted correctly  
- A new column `TotalCost = NumberOfItemPurchased * CostPerItem` is added  

---

## 🔄 ETL Process

1. **Upload CSV** to the `raw/` directory in S3  
2. **Glue Crawler** detects schema and creates a Glue catalog table  
3. **Glue Job** performs transformations:
   - Standardizes column names  
   - Casts correct data types  
   - Fills or removes missing values  
   - Adds calculated fields  
4. **Result** is saved to `processed/` in Parquet format  
5. **Athena** queries the processed data with SQL  

---

## 🧊 Archiving Unused Data

To reduce storage costs, rarely accessed or old processed data is moved to **Amazon S3 Glacier**.

### How it works:
- Processed files in `processed/` that haven’t been accessed in over 30 days are automatically transitioned to Glacier using an [S3 Lifecycle Policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)  
- Retrieval is possible when needed, with delays ranging from minutes to hours

> Example lifecycle policy JSON is available in `/s3-lifecycle/lifecycle-policy.json`.

---

## 🔐 Security and Cost Optimization

- IAM role for Glue is restricted to necessary actions only  
- Using **Parquet** format reduces data scanned and lowers Athena query costs  
- **S3 Lifecycle Policies** help automatically transition old or unused data to **S3 Glacier**, reducing long-term storage costs  
- **AWS Budgets** and alerts help monitor spending in real time  
- In the future, additional data classification and archiving strategies can be implemented to send rarely accessed processed data to **S3 Glacier**, especially useful for historical or compliance-driven data retention


---

## 📊 Sample Analytics

- Top purchased items by country  
- Average spend per user  
- Purchase trends over time  
- Anomaly detection in transactions  

---

## ✅ Benefits

- Fully automated and scalable ETL workflow  
- Fast access to cleaned and queried data  
- Minimal operational overhead  
- Cost-effective and extendable  

---

## 📝 Requirements

- AWS account  
- IAM roles for Glue and Athena with appropriate S3 access  
- Raw CSV data in S3  

---




