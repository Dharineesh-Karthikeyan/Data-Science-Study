# 📊 AWS DynamoDB

## 🚀 Overview
DynamoDB is a fully managed NoSQL key-value and document database offering single-digit millisecond latency at any scale.

## ⚙️ How It Works
- Tables consist of **primary keys** (partition key or partition + sort key)
- Scalable read/write throughput
- Supports Streams and TTL

## 🔧 Setup & Usage
- Create table with key schema
- Insert or query via console, CLI, or SDK

```python
import boto3
ddb = boto3.resource('dynamodb')
table = ddb.Table('Users')
table.put_item(Item={'id': '001', 'name': 'Alice'})
```

## 🎯 Real-World Use Cases
- Storing user sessions, configurations
- Event logs, clickstream data
- Real-time analytics backend

## 🧠 Interview Questions & Answers

### ❓ Q1: What's the difference between partition key and sort key?
**Answer**: Partition key determines data distribution; sort key allows multiple records under one partition.

### ❓ Q2: Is DynamoDB eventually or strongly consistent?
**Answer**: Both. By default, it's eventually consistent. Strong consistency is an option.

### ❓ Q3: How is DynamoDB billed?
**Answer**: Based on provisioned or on-demand RCU/WCU and storage.

## 🧩 Tips for Interviews
- Know the importance of partition key design
- Talk about use with Lambda, Kinesis, or Streams
