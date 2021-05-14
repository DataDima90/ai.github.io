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

**Durability and availability**
- SPICE automatically replicates data for high availability
- Scale to hundreds of thousands of users
- Simulataneous analytics across AWS data sources

**Scalability and elasticity**
- Fully managed service, scale to terabytes of data

**Interfaces**
- RDS, Aurora, Redshift and Redshift Spectrum, Athena, S3 and S3 Analytics, Apache Spark, IoT Analytics, Presto
- SaaS, applications such as Salesforce

**Anti-patterns**
- Highly formatted canned Reports, better for ad hoc query, analysis and visualization of data
- ETL, Glue better choice

## AWS Visualization Services
Identify the appropriate visualization to gain insight through these data patterns. There is the following visualization types
- KPIs
- Relationships
- Comparisons
- Distributions
- Compositions

### Data Visualizations - KPI

Single value that conveys how well your doing in an area or function

**KPI types**
- Net Promoter Score (NPS): how likely is it for a customer to recommend your product or service to a friend
- Customer Profitability Score (CPS): how much profit does a customer bring to your business after deducting customer acquisition and customer retention costs
- Conversion Rate: how many leads get converted to customers
- Relative Market Share: how big is your slice of the pie compared to your competitors in the market
- Net Profit Margin: the percent of your renveue which is net profit

### Data Visualizations - Relationships

Trying to either establish or prove whether a relationship exists between 2 or more variables

**Relationship types**
- Two variables: Scatter Chart - comparing Stock Price to Price Change
- Three variables: Bubble Chart - demonstrating relationship between ROI, Investment Time, and Investment Size

### Data Visualizations - Comparisons

Trying to show or examine how different variables change over time or provide a static snapshot of how different variables compare

**Comparison types**
- One variable: Bar Chart - Comparing sales of various models of phones in a given month
- Three variable: Table - Comparing Revenue Per Salesperson, Revenue Growth, and Territory Size
- One or two variables changing over time: Column Chart - Showing month-over-month Sales and Phone Traffic
- Three or more variables changing over time: Line Chart - Showing month-over-month Sales, Web Traffic, Account Registrations, Perspectus Downloads

### Data Visualiatzions - Distributions

Trying to show how your data is distributed over certain intervals where interval implies clustering or grouping of data, and not time

**Distribution types**
- One variable: Column Histogram - Showing how many traders have made one trad, two trades, three trades, etc. Binning things such as amount, frequency, duration
- Two variables: Scatter Chart - comparing Sales to Revenue

### Data Visualizations - Compositions

Want to highlight the various elements that make up your data - its composition; static or if it is changing over time

**Composition types**
- Pie Chart: Showing composition of stock names in a mutual fund
- Tree Map: Showing the composition customer base by how much revenue they each contribute to the whole
- Stacked Area Chart: Showing the composition of how the number of wins of each team have changed on a weekly basis

## QuickSight Capabilities and operational characterics

Build visualizations, perform ad-hoc analysis, and quickly get business insights from your data

### Capabilities and operational characterics

**Data Source Types**

Use many sources for data analysis, including files, AWS services, and on-premises databases
- Athena, Aurora, Redshift and Redshift Spectrum, S3 and S3 Analytics, Apache Spark, IoT Analytics, RDS, Presto
- Use other data sources by linking or importing them through supported data sources
- Redshift clusters, Athena databases, and RDS instances must be in AWS
- Other data sources must be in one of the following
  - EC2
  - on-prem databases
  - file systems
- Supported file types: CSV and TSV, ELF and CLF, JSON, XLSX

**SPICE**

Super-fast, Parallel, In-memory Calculation Engine (SPICE): engineered to rapidly perform advanced calculations and serve data; in the Enterprise edition data in SPICE is encrypted at rest
- SPICE capacity allocated separately per AWS region
- Can release unused SPICE capacity

**Analysis**
- Use analysis to create and interact with visuals and stories, a container for a set of related visuals and stories
- Use multiple data sets in an analysis, but any given visual can only use one data set
- Share an analysis with other users by emailing them a link; can only share analysis with other users in your QuickSight account
- Use calculated fields to transform your data
  - Many functions, such as extract, formatDate, etc.
  - Aggregate functions

**Visuals**
- Graphical representation of a data set using a diagram, chart, graph, or table
- Supports up to 30 datasets in a single analysis, and up to 30 visuals in a single sheet, and a limit of 20 sheets per analysis

**ML Insights**
- ML insights use machine learning to uncover hidden insights and trends in your data, identify key drivers, and forecast business metrics
- Use the insights generated by ML insights in NLP narratives within dashboards

**Sheets**
- Set of visuals that are viewed together in a single page

**Stories**
- Use a story to play multiple iterations of an analysis sequentially to provide a narrative about the analysis data
- On September 30, 2020, AWS permanently removed all existing stories from QuickSight
- AWS suggests that you recreate stories as visuals that are side-by-side in an analysis as an alternative

**Dashboards**
- Read-only snapshot of an analysis that you can share with other users for reporting
- Preserves the configuration of the analysis at the time of publishing, including filtering, parameters, controls, and sort order
- When a user views the dashboard, it reflects the **current** data in the data sets used by the analysis
- With the enterprise edition a shared dashboard can also be embedded in a website or app
- Drill up/down into data points


## Analysis for Visualization

Understand the best service for your analysis and visualization needs

### Analysis and Visualization Services - Data Lake

- With a data lake you can use your analytics without moving your data to another system
- Access vast amounts of structured, unstructured, and semi-structured data in your data lake
  - Lake Formation provides multi-service, fine-grained access control to data
  - Macie helps detect sensitive data that may have been stored in the wrong place
  - Inspector identfies configuration errors that could lead to data breaches

![]({{ site.url }}/assets/img/posts/datalake.png)

### Analysis and Visualization Services - Redshift

- With Redshift you can run queries on your Redshift cluster data warehouse directly
- Two options: query via AWS management console using the query editor, or via a SQL client tool
  - Use the query editor on the Redshift console, no need for a SQL client application
  - Supports SQL client tools connecting through JDBC and ODBC
  - Using the Redshift API you cann access Redshift data with web services-based applications, including AWS Lambda, AWS AppSync, SageMaker notebooks, and AWS cloud9

![]({{ site.url }}/assets/img/posts/datawarehouse.png)

### Analysis and Visualization Services - Athena and Glue

- With Athena and Glue you can directly query data on S3
- Glue crawls your data, categorizes and cleans it and moves to your data store
- Athena queries data sources that are registered with the AWS Glue Data Catalog
  - Running Data Manipulation Language (DML) queries in Athena via the Data Catalog uses the Data Catalog schema to derive insight from the underlying dataset
  - Run a Glue crawler on a data source from within Athena to create a schema in the Glue Data Catalog
  
![]({{ site.url }}/assets/img/posts/athenaglue.png)

### Analysis and Visualization Servies - Redshift Spectrum

- Use Redshift Spectrum to build queries that combine your Redshift warehouse data with data from S3
- Query data directly from files on S3
- Need a Redshift cluster and a SQL client connected to your cluster to execute SQL commands
- Visualize data via QuickSight
- Cluster and data in S3 must be in the same region

![]({{ site.url }}/assets/img/posts/redshiftspectrum.png)

## Analytics Scenarios and their Solutions


### Type of Analysis

**Descriptive analysis (or data mining)**
- Determine what generated the data
- Highest effort

**Diagnostic analysis**
- Determine why data was generated
- Understand root causes of events

**Predictive analysis**
- Determine future outcomes
- Uses descriptive and diagnostic to predict future trends

**Prescriptive analysis**
- Determine action to take
- Uses other three to predict and can be automated

### Analysis Solutions

Identify analytics processing method based on data type collected and analysis type used

**Batch analytics**
- Large volumes of raw data
- Analytics process on a schedule, reports
- Map-reduce type services: EMR

**Interactive analytics**
- Complex queries on complex data at high speed
- See query results immediately
- Athena, Elasticsearch, Redshift

**Streaming analytics**
- Analysis of data that has short shelf-life
- Incrementally ingest data and update metrics
- Kinesis

## Analytics Solutions Patterns

Select the best option for a scenario based on the type of analytics and processing required

![]({{ site.url }}/assets/img/posts/analyticspatterns.png)

### Analytics Solutions Patterns - EMR

Uses the map-reduce technique to reduce large processing problems into small jobs distributed across many nodes in a Hadoop cluster
- On-Demand big data analyitcs
- Event-driven ETL
- Machine Learning predictive analytics
- Clickstream analysis
- Load data warehousees

Do not use for transactional processing or with small data sets

![]({{ site.url }}/assets/img/posts/emrpattern.png)

### Analytics Solutions Patterns - Kinesis

Streams data to analytics processing solutions
- Video analytics applications
- Real-time analytics applications
- Analyze IoT device data
- Blog posts and article analytics
- System and application log analytics

Do not use for small-scale throughput or with data with longer shelf-life

![]({{ site.url }}/assets/img/posts/kinesispattern.png)

#### Analytics Solutions Patterns - Redshift

OLAP using BI tools
- Near real-time analysis of millions of rows of manufacturing data generated by continous manufacturing equipment
- Analyze events from mobile app to gain insight into how users use the application
- Gain value and insights from large, complex, and dispersed datasets
- Make live data generated by range of next-gen security solutions available to large numbers of organizations for analysis

Do not use for OLTP or with small data sets

![]({{ site.url }}/assets/img/posts/redshiftpattern.png)


### Visualization Methods

Understand refresh schedule, delivery method, and access method based on a scenario. We use data from heterogeneous data sources in visualization solution.

### Refresh Schedule - Real-time Scenarios

Scenarios that question the appropriate refresh schedule based on data freshness requirement
Real-time scenarios, typically using Elasticsearch and Kibana
- Refresh_Interval in Elasticsearch domain updates the domain indices; determines query freshness
- Default is every second - expensive, make sure the shards are evenly distributed.
  - Formula: Number of shards for index = number of shards per node * number of data nodes 
- Balance refresh rate cost with decision making needs

### Refresh Schedule - Interactive Scenarios

Real-time scenarios, typically ad-hoc exploration using QuickSight
Refresh SPICE data
- Refresh a data set on a schedule, or you can refresh your data by refreshing the page in an analysis or dashboard
- Use the CreateIngestion API operation to refresh data

Data set based on a direct query and not stored in SPICE, refresh data by opening the data set

### Refresh Schedule - EMR Notebooks

Using EMR Notebooks to query and visualize data. 
Data refreshed every time the notebook is run
- Expensive proces, balance refresh rate cost with decision making needs

## Kibana Visualizations

### Visualization Tool
- Open-source data visualization and exploration tool used for log and time-series analytics, application monitoring, and operational intelligence use cases. Especially where searching is the main need.
- Tight integration with Elasticsearch
- Charts and reports to interactively navigate through large amounts of log data
- Pre-built aggregations and filters
- Dashboards and reports to share

### Kibana Configuration

- To visualize and explore data in Kibana, you must first create index patterns
- Index patterns point Kibana to the Elasticsearch indexes containing the data to explore
- Explore data with Kibana's data discovery functions
- Kibana visualizations are based on Amazon Elasticsearch queries

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





