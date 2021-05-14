---
layout: post
title: AWS Operational and Analytical Data Storage Services
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [AWS, Data Storage]
mathjax: true
summary: Operational and Analytical Data Storage Services on AWS
---

# Operational and Analytical Data Storage Characteristics

**Operational Storage Characteristics**
- Data stored as rows
- Low latency
- Hight throughput
- Highly concurrent
- Frequent changes
- Benefits from caching
- Often used in enterprise critical applications

**Analytical Storage Characteristics**
- Two types: 
  - OLAP (ad-hoc queries)
  - DSS (long running aggregations)
- Data stored as columns
- Large datasets that take advantage of partitioning (e.g. parquet)
- Frequent complex aggregations
- Loaded in bulk or via streaming
- Less frequent change


## Operational Storage Services on AWS

### 1. RDS - distributed relational database service
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

### 2. DynamoDB - fully managed NoSQL database
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

### 3. Elasticache - fully managed Redis and Memcached
- Use cases
  - Caching, session stores, gaming real-time analytics
- Sub-milisecond response time from in-memory data store
  - Single-digit milisecond latency at scale
- Reliability and durability
  - Redis Elasticache offers multi-AZ with automatic failover

### 4. Timestream - fully managed time series database
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

## Analytical Storage Services on AWS

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


## Data Freshness

We should consider our data's freshness when selecting our storage system components
- Place hot data in cache (Elasticache or DAX) or NoSQL (DynamoDB)
- Place warm data in SQL data stores (RDS)
- Can use S3 for all types (hot, warm, cold)
- Use S3 Glacier for colda data

## Columnar storage
- Drastically reduces the overall disk I/O requirements and reduces the amount of data we need to load from disk
  - In relational dabases, data blocks store values sequentially for each consecutive column making up the entire row
  - In columnar databases, each data block stores values of a single column for multiple rows

## Data Access and Retrieval Patterns
- Characteristics of our data
  - What type of date are we storing? 
    - Structured data
      - Examples: accounting data, demograhpic info, logs, mobile device, geolocation data
      - Storage options: RDS, Redshift, S3 Data Lake
    - Unstructured data
      - Examples: email text, photos, video, audio, PDFs
      - Storage options: S3 Data Lake, DynamoDB
    - Semi-structured data
      - Examples: email metadata, digital photo metadata, video metadata, JSON data
      - Storage options: S3 Data Lake, DynamoDB
- Data storage life cycle
  - How long do we need to retain our data?
    - S3
    - S3 Glacier
- Data access retrieval and latency requirements
  - How fast does our retrieval need to be?
    - Elasticache
    - DynamoDB Acceleration (DAX)

### Data Lake vs Data Warehouse
- Data Warehouse
  - Optimized for relational data produced by transactional systems
  - Data structure/schema defined which optimizes fast SQL queries
  - Used for operational reporting and analysis
  - Schema on write, i.e. data is transformed before loading
- Data Lake
  - Relational and non-relational data
  - Data structure/schema not defined when stored in the data lake
  - Big data analytics, text analysis, ML
  - Schema on read

### Object vs Block store
- Object storage
  - S3 is used for object storage: highly scalable and available
  - Store structured, unstructured, and semi-structured data
  - Web sites, mobile apps, archive, analytics applications
  - Storage via a web service
- File storage
  - Elastic File System (EFS) is used for file storage: shared file systems
  - Content repositories, development environments, media stores, user home directories
- Block storage
  - Elastic Block Storage (EBS) attached to EC2 instances, EFS: volume type choices
  - Redshift, Operating Systems, DBMS installs, file systems
  - HDD: throughput intensive, large I/O, sequential I/O, big data
  - SSD: high I/O per second, transaction, random access, boot volumes
  - What is the difference between HDD and SSD?

### Data Storage Lifecycle
- Persistent data
  - OLTP and OLAP
  - DynamoDB, RDS, Redshift
- Transient data
  - Cached data, streaming data consumed in near-time time
  - Elasticache (Redis, Memcached), DynamoDB Accelerator (DAX)
  - Website session infor, streaming gaming data
- Archive data
  - Retained for years, typically regulatory
  - S3 Glacier

### Data Access Retrieval and Latency
- Retrieval speed
  - Near-real time
    - Streaming data with near-real time dashboad display
  - Cached data
    - Elasticache (Memcached, Redis) **What is the difference?**
    - DAX
      - Right through cache

## Data Management

### Different Approaches to Data Management
- Data Lake
  - Store any data centrally at massive scale
  - Store raw data
- Data Warehouse
  - Centralized data repository for BI and analysis
  - Access the centralized data using BI tools, SQL clients, and other analytics apps



## Sources

### DynamoDB
- [https://www.trek10.com/blog/best-practices-for-secondary-indexes-with-dynamodb](https://www.trek10.com/blog/best-practices-for-secondary-indexes-with-dynamodb)
- [https://www.trek10.com/blog/easier-dynamodb-unit-testing-with-python](https://www.trek10.com/blog/easier-dynamodb-unit-testing-with-python)
- [https://codeburst.io/inverted-index-gsi-overloading-and-sparse-index-in-dynamodb-cbfb520b2696](https://codeburst.io/inverted-index-gsi-overloading-and-sparse-index-in-dynamodb-cbfb520b2696)
- [https://codeburst.io/dynamodb-data-modeling-7f11950b25bf](https://codeburst.io/dynamodb-data-modeling-7f11950b25bf)
- [https://blog.thepolyglotprogrammer.com/10-things-you-should-know-about-dynamodb-e5b9593e6340](https://blog.thepolyglotprogrammer.com/10-things-you-should-know-about-dynamodb-e5b9593e6340)
