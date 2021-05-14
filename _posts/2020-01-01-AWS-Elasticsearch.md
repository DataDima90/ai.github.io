---
layout: post
title: AWS Elasticsearch
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Elasticsearch, AWS]
mathjax: true
summary: AWS Elasticsearch Overview
---

# AWS Elasticsearch

Fully managed service that delivers Elasticsearch's APIs and built-in integration with Kibana

**Usage pattern**
- Analyze activity logs
- Analyze social media sentiments
- Usage monitoring for mobile applications
- Analyze data stream updates from other AWS services

**Cost**
- Elasticsearch instance hours
- EBS (Elastic Block Storage) storage (if you choose this option), and standard data transfer fees

**Performance**
- Instance type, workload, index, number of shards used, read replicas, storage configurations
- Fast SSD instance storage for storing indexes or multiple EBS volumes

- Zone Awareness: distributes domain instances across two different AZs

**Durability and availability**
- Zone Awareness gives you cross-zone replication
- Automated and manual snapshots for durability

**Scalability and elasticity**
- Use API and CloudWatch metrics to automatically scale up/down

**Interfaces**
- S3, Kinesis, DynamoDB Streams, Kibana
- Lambda Function as an event handler

**Anti-patterns**
- OLTP, RDS better choice
- Ad-hoc data querying, Athena better choice
