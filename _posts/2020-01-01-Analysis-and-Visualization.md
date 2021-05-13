---
layout: post
title: Data Analysis and Visualization on AWS
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Data Analysis, Data Visualization, AWS]
mathjax: true
summary: Data Analysis and Visualization on AWS
---

# Introduction
In the analytics and visualization you use your collected, processed, and transformed data to create actionable insights. 
We need to understand which analysis and visualization methods and tools to use based on your audience's expectations for access and the insight they expect to gain from your visualizations.

## Table of Contents

This Page is divided into following parts:

1. [Operational characteristics of an analysis and visualization solution](#anaviz)
2. [Select the appropriate data analysis solution for a given scenario](#ana)
3. [Select the appropriate data visualization solution for a given scenario](#viz)

<a name="anaviz"></a>
# 1. Operational Characteristics Of An Analysis And Visualization Solution

Here we will understand the analysis and visualization components of a data analyitcs solution and which AWS servies implement these concepts


## Purpose Built Analytics Services
Choose the correct approach and tool for your analytics problem. Know the AWS purpose bilt analytics services:
- Athena
- Elasticsearch
- EMR
- DynamoDB
- Kinesis Family (Data Streams, Firehose, Analytics, Video Streams)
- Redshift (Spectrum)
- MSK (Managed Streaming for Apache Kafka)
- QuickSight
- Also know where to use
  - S3
  - EC2
  - Glue
  - Lambda

### Glue

- Fully managed ETL service used to catalog, clean, enrich, and move data between data stores

**Usage patterns**
- Crawl your data and generate code to execute, including data transformation and loading
- Integrate with services like Athena, EMR, and Redshift
- Generates customizable, reusable, and portable ETL code using Python and Spark

![]({{ site.url }}/assets/img/posts/glueusagepattern.png)

**Cost**
- Hourly rate, billed by the minute, for crawler and ETL jobs
- Glue Data Catalog: pay a monthly fee for storing and accessing the metadata

**Performance**
- Scale-out Apache Spark environment to load data to target data store
- Specify the number of Data Processing Units (DPUs) to allocate to your ETL job

- Connect to many data sources, S3, RDS, or many other types of data sources

**Durability and availability**
- Glue leverages the durability of the data stores to which you connect
- Provides job status and pushes notifications to CloudWatch events
- Use SNS notifications from CloudWatch events to notify of job failures or success

**Scalability and elasticity**
- Runs on top of the Apache Spark for transformation job scale-out execution

**Interfaces**
- Crawlers scan many data store types
- Bulk import Hive metastore into Glue Data Catalog

**Anti-patterns**
- Streaming data, unless Spark Streaming
- Heterogeneous ETL job types; use EMR
- NoSQL databases: not supported as data source


### Lambda

Lambda is serverless, e.g. run code without provising or managing servers.

**Usage patterns**
- Execute code in response to triggers such as changes in data, changes in system state, or actions by users
- Real-time File Processing and stream
- Process AWS Events
- Replace Cron
- ETL

![]({{ site.url }}/assets/img/posts/lambdausagepattern.png)

**Cost**
- Charged by the number of requests to functions and code execution time
- $0.20 per 1 million requests
- Code execution $0.00001667 for every GB-second used

**Performance**
- Process events within milliseconds
- Latency higher for cold-start
- Retain a function instance and reuse it to serve subsequent requests, versus creating new copy

- Uses replication and redundancy for high availability of the service and the functions it operates

**Durability and availability**
- No maintenance windows or scheduled downtime
- On failure, Lambda synchronoously responds with an exception
- Asynchronous functions are retried at least 3 times

![]({{ site.url }}/assets/img/posts/lambda.png)

**Scalability and elasticity**
- Scales automatically with no limits with dynamic capacity allocation

**Interfaces**
- Trigger Lambda from AWS servies events
- Respond to CloudTrail audit log entries as events

**Anti-patterns**
- Long running applications: 900 sec runtime
- Dynamic websites
- Stateful applications: must be stateless

### EMR

Uses Hadoop to distribute data and processing across a resizeable cluster of EC2 instances

**Usage patterns**
- Reduces large processing problems and data sets into smaller jobs and distributes them across many compute nodes in a Hadoop cluster
- Log processing and analytics
- Large ETL data movement
- Ad targeting, click stream analytics
- Predictive analytics

**Cost**
- Pay for the hours the cluster is up
- EC2 pricing options (On-Demand, Reserved, and Spot)
- EMR price is in addition to EC2 price

**Performance**
- Driven by type/number of EC2 instances
- Ephemeral or long-running

![]({{ site.url }}/assets/img/posts/emr.png)

- Fault tolerant for core node failures and continues job execution if a slave node goes down

**Durability and availability**
- Starts p a new node if core node fails
- Will not replace nodes if all nodes in the cluster fail
- Monitor for node failures through CloudWatch

**Scalability and elasticity**
- Resize your running cluster: add core nodes, add/remove task nodes

**Interfaces**
- Tools on top of Hadoop: Hive, Pig, Spark, HBase, Presto
- Kinesis Connector: directly read and query data from Kinesis Data Streams

**Anti-patterns**
- Small data sets; Made for big data sets
- ACID transactions; RDS is a better choice


### Kinesis

Load and analyze streaming data; Ingest real-time data into data lakes and data warehouses

**Usage patterns**
- Move data from producers and continuously process it to transform before moving to another data store; drive real-time metrics and analytics
- Real-time data analytics
- Log intake and processing
- Real-time metrics and reporting
- Video/Audio processing

![]({{ site.url }}/assets/img/posts/kinesis.png)

**Cost**
- Pay for the resources consumed
- Data Streams hourly price per/shard
- Charge for each 1 million PUT transactions

**Performance**
- Data Streams: throughput capacity by number of shards
- Provision as many shards as needed

- Synchronously replicates data across three AZs

**Durability and availability**
- Highly available and durable due to config of multiple AZs in one region
- Use cursor in DynamoDB to restart failed apps
- Resume at exact position in the stream where failure occured

**Scalability and elasticity**
- Use API calls to automate scaling, increase or decrease stream capacity at any time

**Interfaces**
- Two interfaces:
  - Input (KPL, agend, PUT API)
  - Output (KCL)
  - Kinesis Storm Spout: read from an Kinesis stream into Apache Storm

**Anti-patterns**
- Small scale consistent throughput
- Long-term data storage and analytics; Redshift, S3, or DynamoDB are better choices

### DynamoDB

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

### Redshift

Fully-managed, petabyte-scale data warehouse service for analyzing data using BI tools

**Usage patterns**
- Designed for online analytical processing (OLAP) using business intelligence tools
- Analyze sales data for multiple products
- Analyze ad impressions and clicks
- Aggregate gaming data
- Analyze social trends

**Cost**
- Charges based on the size and number of cluster nodes
- Backup sotrage > provisioned storage size and backups stored after cluster termiantion billed at standard S3 rate

**Performance**
- Columnar storage, data compression, and zone maps to reduce query I/O
- Paralllizes and distributes SQL operations to take advantage of all available resources

- Automatically detects and replaces a failed node in your data warehouse cluster

**Durability and availability**
- Failed node cluster is read-only until replacement node is provisioned and added to the DB
- Cluster remains available on drive failure; Redshift mirrors your data across the cluster

**Scalability and elasticity**
- With API change the number, or type, of nodes while cluster remains live

**Interfaces**
- JDBC and ODBC drivers for use with SQL clients
- S3, DynamoDB, Kinesis, BI tools such as QuickSight

**Anti-patterns**
- Small data sets, better big data sets
- OLTP, better OLAP
- Unstructured data
- Blob data, S3 better choice

### Athena

Interactive query service used to analyze data in S3 using Presto and standard SQL

**Usage pattern**
- Interactive ad-hoc querying for web logs
- Query staging data before loading into Redshift
- Send AWS service logs to S3 for Analysis with Athena
- Integrate with Jupyter, Zeppelin

![]({{ site.url }}/assets/img/posts/athena.png)

**Cost**
- $5 per TB of query data scanned
- Save on per-query costs and get better performance by compressing, partitioning, and converting data into columnar formats

**Performance**
- Compressing, partitioning, and converting your data into columnar formats
- Convert data to columnar formats, allowing Athena to read only the columns it needs to process queries

- Executes queries using compute resources across multiple facilites

**Durability and availablity**
- Automatically routes queries if a particular facility is unreachable
- S3 is the underlying data store, gaining S3's 11 9s durability

**Scalability and elasticity**
- Serverless, scales automatically as needed

**Interfaces**
- Athena CLI, API, via SDK, and JDBC
- QuickSight visualizations

**Anti-patterns**
- Enterprise Reporting and Business Intelligence Workloads; Redshift better choice
- ETL Workloads; EMR and Glue better choice
- Not a replacement for RDBMS


### Elasticsearch Service

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


### QuickSight

BI service to build visualizations, perform ad-hoc analysis, and get business insights from data

**Usage pattern**
- Ad-hoc exploration/visualization
- Dashboard and KPIs
- Analyze and visualize data coming from logs and stored in S3
- Analyze and visualize data in SaaS applications like Salesforce

**Cost**
- Standard $9/user/month; Enterprise $18/user/month
- SPICE capacity: $.25/GB/month Standard; $.38/GB/month Enterprise

**Performance**
- SPICE uses a combination of columnar storage and in-memory technologies
- Machine code generation to run interactive queries on large datasets at low latency

- SPICE automatically replicates data for high availability

**Durability and availability**
- Scale to hundreds of thousands of users
- Simulataneous analytics across AWS data sources

**Scalability and elasticity**
- Fully managed service, scale to terabytes of data

**Interfaces
- RDS, Aurora, Redshift, Athena, S3
- SaaS, applications such as Salesforce

**Anti-patterns**
- Highly formatted canned Reports, better for ad hoc query, analysis and visualization of data
- ETL, Glue better choice


## Appropriate Analytics Service

**Analytics**
- Interactive analytics
  - Service: Athena
- Big data processing
  - Service: EMR
- Data warehousing
  - Service: Redshift
- Real-time analytics
  - Service: Kinesis
- Operational analyitcs
  - Service: Elasticsearch
- Dashboards and visualizations
  - Service: QuickSight

**Data movement**
- Real-time data movement
  - Service: MSK, Kinesis Family, Glue

**Data Lake**
- Object Storage
  - Service: S3, Lake Formation
- Backup and archive
  - Service: S3 Glacier, AWS Backup
- Data catalog
  - Service: Glue, Lake Formation
- Third-part data
  - Service: AWS Data Exchange

**Predictive analytics and machine learning**
- Frameworks and interfaces
  - Service: AWS Deep Learning AMIs
- Platform services
  - SageMaker

### Use Case - Data Warehousing

![]({{ site.url }}/assets/img/posts/datawarehousingusecase.png)

### Use Case - Big Data Processing

![]({{ site.url }}/assets/img/posts/bigdatausecase.png)

### Use Case - Real Time Analytics with MSK and Kinesis

![]({{ site.url }}/assets/img/posts/realtimeanalyticskinesismskusecase.png)

### Use Case - Operational Analytics

![]({{ site.url }}/assets/img/posts/operationalanalyticsusecase.png)



<a name="ana"></a>
# 2. Select The Appropriate Data Analysis Solution For A Given Scenario

Here we will understand which analysis solution for given scenario is the best.
- We select the appropriate type of analyis
- We select best solution for a scenario
  - Analysis method
  - Analysis tools
  - Analysis technology


<a name="viz"></a>
# 3. Select The Appropriate Data Visualization Solution For A Given Scenario

Here we will understand visualization solution characteristcs and AWS services and options for visualization.
- Presentation type
- Refresh schedule
- Delivery method
- Access method

Moreover we will understand aggregating data from different types of data sources into your visualization solution.





