# 🖥️ AWS EC2 (Elastic Compute Cloud)

## 🚀 Overview
EC2 provides scalable virtual servers to run applications in the cloud. It's ideal for compute-heavy workloads.

## ⚙️ How It Works
- Launch an instance (VM) using an AMI
- Choose instance type, OS, storage, and networking
- Pay for uptime (on-demand, reserved, spot)

## 🔧 Setup & Usage
- Create via Console or CLI
```bash
aws ec2 run-instances --image-id ami-xxxx --count 1 --instance-type t2.micro
```
- SSH into instance and install packages

## 🎯 Real-World Use Cases
- Hosting ML model APIs
- Running batch jobs
- Custom analytics processing

## 🧠 Interview Questions & Answers

### ❓ Q1: What is the difference between EC2 and Lambda?
**Answer**: EC2 is VM-based and stateful. Lambda is event-driven and stateless.

### ❓ Q2: How can you scale EC2?
**Answer**: Using Auto Scaling Groups and Load Balancers.

### ❓ Q3: What is an AMI?
**Answer**: Amazon Machine Image — a preconfigured template for launching EC2 instances.

## 🧩 Tips for Interviews
- Know instance types (general, compute-optimized)
- Understand EBS vs. instance store
