# 🧬 AWS Glue (ETL)

## 🚀 Overview
AWS Glue is a serverless ETL service that prepares and transforms data for analytics, ML, and reporting.

## ⚙️ How It Works
- Crawlers scan and catalog datasets into the Glue Data Catalog
- Jobs are written in PySpark or Scala and run serverlessly
- Can be triggered manually, by schedule, or event

## 🔧 Setup & Usage
- Create a crawler to scan S3
- Author a Glue job in Python
```python
import sys
from awsglue.context import GlueContext
glueContext = GlueContext(SparkContext.getOrCreate())
```

## 🎯 Real-World Use Cases
- Transforming S3 data into partitioned Parquet for Athena
- Scheduled batch ETL pipelines

## 🧠 Interview Questions & Answers

### ❓ Q1: What is a Glue Crawler?
**Answer**: A service that scans your data and infers schema to register in the Glue Data Catalog.

### ❓ Q2: What language is used for Glue jobs?
**Answer**: PySpark or Scala.

### ❓ Q3: How is Glue billed?
**Answer**: Billed by Data Processing Units (DPUs) per minute.

## 🧩 Tips for Interviews
- Be ready to describe how you’d build an S3 → Glue → Athena pipeline
- Talk about schema evolution and partitioning
