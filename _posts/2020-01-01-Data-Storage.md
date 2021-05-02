---
layout: post
title: Data Storage
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [AWS, Storage Solutions]
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


### Operational Storage Services on AWS

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
  - High performance and low cost even at extreme scale
- Reliability and durability
  - Data replicated across three AZs
  - Global tables for multi-region replication

**Elasticache - fully managed Redis and Memcached**
- Use cases
  - Caching, session stores, gaming real-time analytics
- Sub-milisecond response time from in-memory data store
  - Single-digit milisecond latency at scale
- Reliability and durability
  - Redis Elasticache offers multi-AZ with automatic failover

**Timestream - fully managed time series database**
- Use cases
  - IoT applications, Industrial telemetry, application monitoring
- Fast: analyze trillions of events per day
  - One tenth the cost of relational database
- Scale:
  - Vertical scaling
  - Timestream scales up or down depending on our load
- Reliability and durability
  - Managed service takes care of provisioning patching, etc.
  - Retention policies to manage reliability and durability

### Analytical Storage Services on AWS

**Redshift - cloud data warehouse**
- Use cases
  - Data science queries, marketing analysis
- Fast: columnar storage technology that parallelizes queries
  - Milisecond latency queries
- Reliability and durability
  - Data replicated within the Redshift cluster
  - Continous backup to S3

**S3 - object storage via a web service**
- Use cases
  - Data lake, analytics, data archiving, static website
- Fast: query structured and semi-structured data
  - Use Athena and Redshift Spectrum to query at low latency
- Reliability and durability
  - Data replicated across three AZs in a region
  - Same-region or cross-region replication


**Data Freshness**
We should consider our data's freshness when selecting our storage system components
- Place hot data in cache (Elasticache or DAX) or NoSQL (DynamoDB)
- Place warm data in SQL data stores (RDS)
- Can use S3 for all types (hot, warm, cold)
- Use S3 Glacier for colda data


### Operational Characteristics of DynamoDB in detail

- DynamoDB tables
  - Attributes are the columns
  - Items are the rows
  - A collection of items
  - Must have a primary key, two types
    - Partiion key: primary key with one attribute called the hash attribute
      - DynamoDB has function determines the partition where an item is located
    - Composite primary key: partition key plus sort key (range attribute) where all items with the same sort key are located together ordered by sort key value
  - No limit to number of items (rows) in a table
  - Maximum item size is 400KB
- DynamoDB has two models of consistency: eventual and strong consistency
- ACID
- Cost versus performance - two capacity modes
- DynamoDB Global tables



