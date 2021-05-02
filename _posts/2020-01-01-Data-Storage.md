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

#### 1. RDS - distributed relational database service
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

#### 2. DynamoDB - fully managed NoSQL database
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

#### 3. Elasticache - fully managed Redis and Memcached
- Use cases
  - Caching, session stores, gaming real-time analytics
- Sub-milisecond response time from in-memory data store
  - Single-digit milisecond latency at scale
- Reliability and durability
  - Redis Elasticache offers multi-AZ with automatic failover

#### 4. Timestream - fully managed time series database
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
  - Must have a primary key, two types:
    - Partition key: primary key with one attribute called the hash attribute
      - DynamoDB has function determines the partition where an item is located
    - Composite primary key: partition key plus sort key (range attribute) where all items with the same sort key are located together ordered by sort key value
  - No limit to number of items (rows) in a table
  - Maximum item size is 400KB
- DynamoDB has two models of consistency: eventual and strong consistency
  - Eventually consistent reads are the default
    - Achieves maximum read throughput (Performance +)
    - May return stale data (Accuracy -)
  - Strongly consistent reads
    - Returns result representing all prior write operations
    - Always accurate data returned (no stale data) (Accuracy +)
    - Increased latency with this option (Performance -)
- Atomicity, consistency, islolation, and durability (ACID)
  - Support for ACID transactions using transactional read and write APIs
  - ACI across one or more tables in one account and region
  - Use for transactions that require coordinated inserts, updates, or deletes to multiple items as part of one logical operation
- Cost versus performance - two capacity modes
  - On-demand capacity
    - DynamoDB automatically provisions capacity based on your workload, up or down
  - Provisioned capacity
    - Specify read capacity units (RCUs) and write capacity units (WCUs) per second for your application
    - One RCU equals one strongly consistent read or two eventually consistent reads per second for items to up to 4 KB
    - One WC equals one write per second for items to up to 1 KB
    - Can use auto scaling to automatically calibrate our table's capacity
      - Helps manage cost
- DynamoDB Global tables
  - Specifiy multple in which our table is available
  - DynamoDB propagates all changes across all regions
  - Any change to an item in any replica is propagated to all other replicas
  - New items propagated within seconds
  - Uses last-writer-wins reconciliation with concurrent updates
  - When a table in a region has issues, application directs to a different region


## Sources

### DynamoDB
- [https://www.trek10.com/blog/best-practices-for-secondary-indexes-with-dynamodb](https://www.trek10.com/blog/best-practices-for-secondary-indexes-with-dynamodb)
- [https://www.trek10.com/blog/easier-dynamodb-unit-testing-with-python](https://www.trek10.com/blog/easier-dynamodb-unit-testing-with-python)
- [https://codeburst.io/inverted-index-gsi-overloading-and-sparse-index-in-dynamodb-cbfb520b2696](https://codeburst.io/inverted-index-gsi-overloading-and-sparse-index-in-dynamodb-cbfb520b2696)
- [https://codeburst.io/dynamodb-data-modeling-7f11950b25bf](https://codeburst.io/dynamodb-data-modeling-7f11950b25bf)
