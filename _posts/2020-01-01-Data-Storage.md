---
layout: post
title: Data Storage on AWS
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [AWS, Database]
mathjax: true
summary: Data Storage
---

# Data Storage

**Operational storage characteristics**
- Data stored as rows
- Low latency
- Hight throughput
- Highly concurrent
- Frequent changes
- Benefits from caching
- Often used in enterprise critical applications

**Analytic storage characteristics**
- Two types: OLAP (ad-hoc queries) and DSS (long running aggregations)
- Data stored as columns
- Large datasets that take advantage of partitioning (e.g. parquet)
- Frequent complex aggregations
- Loaded in bulk or via streaming
- Less frequent change


### Operational Characteristics of Storage Services on AWS

**RDS - distributed relational database service**
- Use cases
  - E-commerce, web, mobile
- Fast OLTP database options
  - SSD-backed storage options
- Scale
  - Vertical scaling or in other words scaling up
  - Instance and storage size determine scale
- Reliability and durability
  - Multi-AZ
  - Automated backups and snapshots
  - Automated failover

**DynamoDB - fully managed NoSQL database**
- Use cases
  - Ad Tech, gaming, retail, banking and finance
- Fast NoSQL database options
  - Single-digit milisecond latency at scale
- Scale
  - Horizontal scaling
  - Can store data without bounds




