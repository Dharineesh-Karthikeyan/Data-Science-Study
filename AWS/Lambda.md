# ⚡ AWS Lambda

## 🚀 Overview
AWS Lambda is a serverless compute service that runs code in response to events and automatically manages the underlying infrastructure.

## ⚙️ How It Works
- Upload code as a Lambda function
- Triggered by events (e.g., S3 upload, DynamoDB change, API Gateway)
- Charges based on runtime and memory

## 🔧 Setup & Usage
- Define a function in Python/Node.js, etc.
- Attach trigger (e.g., S3 event)
- Deploy via Console or CLI

```python
def lambda_handler(event, context):
    return {"statusCode": 200, "body": "Hello World"}
```

## 🎯 Real-World Use Cases
- ETL triggers from S3
- On-demand data processing
- Automated alerts and notifications

## 🧠 Interview Questions & Answers

### ❓ Q1: What's the max runtime of a Lambda?
**Answer**: 15 minutes per invocation.

### ❓ Q2: How is Lambda billed?
**Answer**: Based on memory allocated and execution time, rounded to the nearest millisecond.

### ❓ Q3: Can Lambda access a VPC?
**Answer**: Yes, it can be configured to access private resources within a VPC.

## 🧩 Tips for Interviews
- Know event-driven architecture patterns
- Understand integration with API Gateway, S3, DynamoDB
