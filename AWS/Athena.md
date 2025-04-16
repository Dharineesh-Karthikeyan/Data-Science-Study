# 🔍 AWS Athena

## 🚀 Overview
Athena is a serverless query service that lets you analyze data in S3 using SQL without needing infrastructure.

## ⚙️ How It Works
- Based on Presto engine
- Uses Glue Data Catalog for table definitions
- Charges per TB scanned

## 🔧 Setup & Usage
- Define tables using SQL or Glue crawler
- Run queries on top of S3 data

```sql
SELECT * FROM s3_data WHERE year = 2023 LIMIT 10;
```

## 🎯 Real-World Use Cases
- Interactive data exploration
- Log analysis, reporting dashboards

## 🧠 Interview Questions & Answers

### ❓ Q1: How does Athena know the schema?
**Answer**: It uses the Glue Data Catalog or a manual DDL statement.

### ❓ Q2: How to optimize query costs in Athena?
**Answer**: Use columnar formats like Parquet, partition your data, and project only needed columns.

## 🧩 Tips for Interviews
- Talk about S3 + Glue + Athena architecture
- Mention integration with QuickSight
