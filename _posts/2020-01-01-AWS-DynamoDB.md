---
layout: post
title: AWS DynamoDB
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [DynamoDB, AWS]
mathjax: true
summary: AWS DynamoDB Overview
---

# DynamoDB

Fully managed NoSQL database that provides low-latency access at scale

**Usage patterns**
- Apps needing low-latency NoSQL database able to scale storage and throughput up or down without code changes or downtime
- Mobile apps and gaming
- Sensor networks
- Digital and serving
- E-commerce shopping carts

![]({{ site.url }}/assets/img/posts/dynamodb.png)

**Cost**
- Three components:
  - Provisioned throughput capacity (per hour)
  - Indexed data storage (per GB per month)
  - Data transfer in or out (per GB per month)

**Performance**
- SSDs and limiting indexing on attributes provides high throughput and low latency
- Define provisioned throughput capacity required for your tables

- Fault tolerance: automatically synchronously replicates data across 3 data centers in a region

**Durability and avaiability**
- Protection against individual machine or facility failures
- DynamoDB Streams allows replication across regions
- DynamoDB Streams enables tables data activity replicated across geographic regions

**Scalability and elasticity**
- No limit to data storage, automatic storage allocation, automatic ddata partition

**Interfaces**
- REST API allows management and data interface
- DynamoDB select operation creates SQL-like queries

**Anti-patterns**
- Port application from relational database
- Joins or complex transactions
- Large data with low I/O rate
- Blob (Binary Large Object) data > 400KB, S3 better choice

![]({{ site.url }}/assets/img/posts/dynamodbdax.png)
