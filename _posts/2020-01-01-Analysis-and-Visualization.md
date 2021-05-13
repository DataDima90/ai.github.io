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

Fully managed ETL service used to catalog, clean, enrich, and move data between data stores

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





