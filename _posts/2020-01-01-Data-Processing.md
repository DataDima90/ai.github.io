---
layout: post
title: Data Processing on AWS
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Data Processing, AWS]
mathjax: true
summary: Data Processing on AWS
---


# Glue ETL on Apache Spark

- Use Glue when you do not need or want to pay for an EMR cluster
- Glue generates an Apache Spark (PySpark or Scala) script
- Glue runs in a fully managed Apache Spark environment

- Spark has 4 primary libraries:
  - Spark SQL
  - Spark Streaming
  - MLlib
  - GraphX

## Glue ETL Jobs - Structure
A Glue job defines the business logic that performs the ETL work in AWS Glue
- Glue runs your script to extract data from your sources, transform the data, and load it into your targets
- Glue triggers can start jobs based on a schedule or event, or on demand.
- Monitor your job runs to get runtime metrics: completion status, duration, etc.
- Based on your source schema and target location or schema, the Glue code generator automatically creates an Apache Spark API (PySpark) script.
  - Edit the script to customize to your requirements.

## Glue ETL Jobs - Types
Glue output file formats JSON, CSV, ORC (Optimized Row Columnar), Apache Parquet, and Apache Avro
Three types of Glue jobs:
- Spark ETL job: executed in managed Apache Spark environment, processes data in batches
- Streaming ETL job: uses the Apache Spark Structured Streaming framework
- Python shell job: schedule and run tasks that do not require an Apache Spark environment

## Glue ETL Jobs - Transforms
Glue has built-in transforms for processing data
- Call from within your ETL script
- In a DynamicFrame (an extension of an Apache Spark SQL DataFrame), your data passes from transfrom to transform
- Built-in transform types (subset)
  - ApplyMapping: maps source DynamicFrame columns and data types to target DynamicFrame columns and data types
  - Filter: selects records from a DynamicFrame and returns a filtered DynamicFrame
  - Map: applies a function to the records of a DynamicFrame and returns a transformed DynamicFrame
  - Relationalize: converts a DynamicFrame to a relational (rows and columns) form

## Glue ETL jobs - Triggers
A trigger can start specificed jobs and crawlers
- On demand, based on a schedule, or based on combination of events
- Add a trigger via the Glue console, the CLI or the Glue API
- Activate or deactivate a trigger via the Glue console, the CLI, or the Glue API

## Glue ETL jobs - Monitoring
Glue produces metrics for crawlers and jobs for monitoring
- Statistics about the health of your environment
- Statistics are written to the Glue Data Catalog

Use automated monitoring tools to watch Glue and report problems
- CloudWatch events
- CloudWatch logs
- CloudTrail logs

Profile your Glue jobs using metrics and visualize on the Glue and CloudWatch consoles to identify and fix issues


# EMR Cluster ETL Processing

- More flexible and powerful than Spark
- Can use Spark on EMR, but also have other options:
  - Flink: Framework and distributed processing engine for stateful computations over unbounded and bounded data streams
  - Hadoop: Framework that allows for the distributed processing of large data sets across clusters of computers
  - Spark: Distributed general-purpose cluster-computing framework
  - Sqoop: Command-line interface application for transferring data between relational databases and Hadoop
  - TensorFlow: Machine Learning library
  - Zepplin: Web-based notebook for data-driven analytics
  - Hive: A SQL-like interface to query data stored in various databases and file systems that integrates with Hadoop. Alternative to Glue Data Catalog
  - Hue: Web interface for analyzing data with Hadoop
  - Presto: High performance distributed SQL query engine for big data

## EMR Components

EMR is built upon clusters (or collections) of EC2 instances
- The EC2 instances are called nodes, all of which have roles (or node type) in the cluster
- EMR installs different software components on each node type, defining the node's role in the distributed architecture of EMR
- Three types of nodes in an EMR cluster
  - Master node: manages the cluster, running software components to coordinate the distribution of data and tasks across other nodes for processing
  - Core node: has software components that run tasks and store data in the HDFS on your cluster
  - Task node: node with software components that ONLY runs tasks and does NOT store data in HDFS

## EMR Cluster - Work
Options for submitting work to your EMR cluster
- Script the work to be done as functions that you specify in the steps you define when you create a cluster
  - This approach is used for clusters that process data and then terminate
- Build a long-running cluster and submit steps (containing one or more jobs( to it via the console, the EMR API, or the AWS CLI
  - This approach is used for clusters that process data continuously or need to remain available
- Create a cluster and connect to the master node and/or other nodes as required using SSH
  - This approch is used to perform tasks and submit queries, either scripted or interactively, via the interfaces of the installed applications

## EMR Cluster - Processing Data
At launch time, you choose the framework and applications to install to achieve your data prcoessing needs.
You submit jobs or queries to installed applications or run steps to process data in your EMR cluster
- Submitting jobs/steps to installed applications
  - Each step is a unit of work that has instructions to process data by software installed on the cluster
  - Example: Submit a request to run a set of steps

## EMR Cluster - Lifecycle

1. Provisions EC2 instances of the cluster
2. Runs bootstrap actions
3. Installs applications such as Hive, Hadoop, Sqoop, Spark
4. Connect to the cluster instances; cluster sequentially runs any steps that specified at creation; submit additional steps
5. After steps complete the cluster waits or shuts down, depending on config
6. When all instances are terminated, the cluster moves to COMPLETED state

## EMR Architecture - Storage
Architected in layers.
Storage: file systems used by the cluster
- HDFS: distributes the data it stores across instances in the cluster (ephemeral)
- EMR File System (EMRFS): directly access data stored in S3 as if it were a file system like HDFS
- Local file system: EC2 locally connected disk

## EMR Architecture - Cluster Management
YARN (Yet Another Resource Manager)
- Centrally manage cluster resources
- Agent on each node that keeps the cluster healthy, and communicates with EMR
- EMR defaults to scheduling YARN jobs so that jobs will not fail when task nodes running on spot instances are terminated

## EMR Architecture - Data Processing Frameworks
Framework layer that is used to process and anaylze data
Different frameworks are available:
- Hadoop MapReduce:
  - Parallel distributed applications that use Map and Reduce functions (e.g. Hive)
    - Map function maps data to sets of key-value pairs
    - Reduce function combines the key-value pairs and processes the data
  - Spark
    - Cluster framework and programming model for processing big data workloads

## EMR Architecture - Application Programs
- Supports many application programs
- Create processing workloads
- Use machine learning algorithms
- Create stream processing apps
- Build data warehouses and data lakes

## EMR Applications and Categories
EMR supports many hadoop applications
- Spark
- Hive
- Presto
- HBase

Data scientists and engineers use EMR to run analytics workflows using these tools along with Hue and EMR Notebooks

Several important categories of EMR applications
- Data Processing 
- SQL
- NoSQL
- Interactive Analytics

### Data Processing Applications

Apache Spark
- Hadoop ecosystem engine used process large data sets very quickly
- Runs fault-tolerant resilient distributed datasets (RDDs) in-memory, and defines data transformations
- Includes Spark SQL, Spark Streaming, MLlib, and GraphX

Apache Flink
- Streaming dataflow engine that allows you to run real-time stream processing on high-throughput data sources
- Supports APIs optimized for writing both streaming and batch applications

### SQL Applications

Apache Hive
- Data warehouse and analytics package that runs on top of Hadoop
- Allows you to structure, summarize, and query data
- Supports map/reduce functions and complex extensible user-defined data types like JSON and Thrift
- Can process complex and unstructured data sources such as text documents and log files

Presto
- SQL query engine optimized for low-latency, ad-hoc analysis of data
- Can process data from multiple data sources including the HDFS and Amazon S3

### NoSQL Applications
Apache HBase
- Non-relational, distributed database modeled after Google's BigTable
- Runs on top of HDFS to provide BigTable-like capabilities for Hadoop
- Store large amounts of sparse data using column-based compression and storage
- Use S3 as a data store for HBase, enabling you to lower costs and reduce operational complexity

### Interactive Analytics Applications
Hue
- User Interface for Hadoop that allows you to create and run Hive queries, manage files in HDFS, create and run Pig scripts, and manage tables
- Also integrates with S3, so you can query directly against S3 and easily transfer files between HDFS and Amazon S3

EMR Notebooks
- Notebooks pre-configured for Spark that are based on Jupyter notebooks
- Interactively run Spark jobs on EMR clusters written in PySpark, Spark SQL, Spark R or Scala

## Optimizing EMR
Based on your workload you have several options for optimizing your EMR cluster.
Design options:
- Instance type
- Instance configuration
- HDFS capacity
- Dynamic sizing
- Instance fleets or uniform instance groups
- Run multiple steps in parallel

### EMR Instance Types

Ways to add EC2 instances to your cluster.

Instance Groups:
- Manually add instances of the same type to existing core and task instance groups
- Manually add a task instance group, can use different instance type
- Automatic scaling for an instance group based on the value of a CloudWatch metric specified by you

Instance Fleets:
- Add a single task instance fleet
- Change the target capacity for on-demand and Spot Instances for existing core and task instance fleets.

Best Practices:
- Plan your instance capacity:
  - Run a test cluster using a representative sample data set and monitor the node utilization
  - Calculate instance capacity and compare that value against the size of your data
- Master node does not require high computational capacity
- Most EMR clusters can run on m5.xlarge or m4.xlarge
- Data processing load performance depends on the capacity of your core nodes and data size as input, during processing, and as output

### EMR Intance Configuration

Long-running clusters and data warehouses
- Persistent EMR cluster (e.g. Data Warehouse)
  - Master and core instance group as on-demand instances
  - Task instance group as spot instances

Cost-driven workloads: low cost, partial work loss OK
- Transient clusters
- Run all groups, master core, core, and task instance groups as spot instances

Data-critical workloads: low cost, partial work loss not OK
- Master and core instance groups as on-demand, task instance groups as spot

Test environment: all instances groups on spot

### EMR HDFS Capacity
To calculate the storage allocation for you cluster consider the following
- Number of EC2 instances used for core nodes
- Capacity of the EC2 instance store for the instance type used
- Number and size of EBS volumes attached to core nodes
- Replication factor: how each data block is stored in HDFS for RAID-like redundancy
  - 3 for a cluster of 10 or more core nodes, 2 for a cluster of 4-9 core nodes, and 1 for a cluster of three or fewer nodes
- HDFS capacity of you cluster
  - For each core node, add instance store volume capacity to EBS storage capacity
  - Multiply by the number of core nodes, then divide the total by the replication factor
  
### EMR Dynamic Sizing
Dynamically scales nodes in your cluster based on demand
- On scale down of task nodes on a running cluster expect a short delay for any running Hadoop to decommission
- On scale down of core nodes EMR waits for HDFS to decommission to protect your data
- Changing configuration improperly on a cluster with high load can seriously degrade cluster performance

### EMR Instance Fleets vs Uniform Instance Groups
- Choice applies to all nodes for the lifetime of the cluster
- Instance fleets and instance groups cannot coexist in a cluster

Instance fleets:
- Each node type has a single instance fleet; task instance fleet is optional
- Up to 5 instance types (if using allocation strategy, up to 15 instances types on task instance fleets), which can be provisioned as on-demand and Spot Instances
- Mix of specified instance types to reach target capcities: On-Demand and Spot Instances
- Core and task instance fleets: assign a target capcity for On-Demand Instances, and another for Spot Instances

Uniform Instance Groups
- Simplified, up to 50 instance groups: 1 master instance group containing 1 EC2 instance, core instance group containing one or more EC2 instances, up to 48 optional task instance groups
- Scale each instance group by adding and removing EC2 instances manually, or set up automatic scaling

### Run Multiple Steps in Parallel
- Allows for parallel processing and greater speed
- Considerations
  - Use EMR automatic scaling to scale up/down based on the YARN resouroces to prevent resource contention
  - Running multiple steps in parallel requires more memory and CPU utilzation from the master node than running one step at a time
  - Use YARN scheduling features such as FairScheduler or CapacityScheduler to run multiple steps in parallel
  - If you run out of resources because the cluster is running to many concurrent steps, manually cancel any running stepts to free up resources


## Which Tool or Service for Batch or Streaming ETL?

Batch processing model
- Data is collected over a period of time, then run through analyitcs tools
- Time consuming, designed for large quantities of information that are not time-sensitive

Streaming processing model
- Data is processed in a stream, a record at a time or in micro-batches of tens, hundreds, or thousands of records
- Fast, designed for information that's needed immediately

### Batch Processing ETL

AWS services used for batch processing

Glue Batch ETL
- Schedule ETL jobs to run at a minimum of 5-minute intervals
- Process micro-batches
- Serverless

EMR Batch ETL
- Use Impala or Hive to process batches of data
- Cluster of servers
- Very flexible in tool/application selection

### Streaming Processing ETL

AWS services used for streaming processing

Lambda
- Reads records from your data stream, runs functions synchronously
- Frequently used with Kinesis
- Serverless

Kinesis
- Use KCL, Kinesis Analytics, Kinesis Firehose to process your data
- Serverless

EMR Streaming ETL
- Use Spark Streaming to build your stream processing ETL application
- Cluster of servers

## Orchestration of Workflows

Coordinating your ETL jobs across Glue, EMR and Redshift.
- With orchestration you can automate each step in your workflow
  - Retry on errors
  - Find and recover jobs that fail
  - Track the steps in your workflow
  - Build repeatable workflows
- Respond to state changes on EMR cluster
- Use CloudWatch metrics to manage your EMR cluster
- View and monitor your EMR cluster
- Leverage EMR API calls in CloudTrail
- Orchestrate Spark, Redshift and EMR workloads using Step Functions

### Respond to State Changes on EMR Cluster
Trigger create, terminate, scale cluster, run Spark, Hive, or Pig workloads based on Cluster state changes
- EMR CloudWatch events support notify you of state changes in your cluster
- Respond to state changes programmatically
- EMR CloudWatch events
  - Cluster State Change
  - Instance Group and Instance Fleet State Change
  - Step State Change
  - Create filters and rules to match events and route them to SNS topics, Lambda functions, SQS queues, Kinesis Streams

### Use CloudWatch Metrics to Manage EMR Cluster

EMR metrics updated every 5 minutes, collected and pushed to CloudWatch
- Non-configurable metric timming
- Metrics archived for two weeks then discarded

EMR metrics uses
- Track progress of cluster: `RunningMapTasks`, `RemainingMapTasks`, `RunningReducedTasks`, and `RemainingReduceTasks`
- Detect idle clusters: `IsIdle` metric tasks if a cluster is live, but not currently running tasks. Set an alarm to fire when the cluster has been idle for a given period of time
- Detect when a node runs out of storage: `HDFSUtilization` metric gives the percentage of disk space currently used. If it rises above an acceptable level for your application, such as 80% of capacity used, you take an action to resize your cluster and add more core nodes

### View and Monitor EMR Cluster

EMR has several tools for retrieving information about your cluster
- Console, CLI, and API
- Hadoop web interfaces and logs on Master node, e.g. Hue
- Use monitoring services like CloudWatch and Ganglia to track the performance of your cluster
- Application history available through persistent application UIs for Spark History Server, persistent YARN timeline Server, and Tez user interfaces

### Leverage EMR API Calls in CloudTrail

CloudTrail holds a record of actions taken by users, roles, or an AWS servie in EMR
- Captures all API calls for EMR as events
- Enable continous delivery of CloudTrail events to an S3 bucket
- Determine the EMR request, the IP address from which the request was made, who made the request, when it was made, and additional details

### Orchestrate Spark and EMR workloads using Step Functions

- Use Apache Oozie or Apache Airflow scheduler tools for EMR Spark applications
- Use Step Functions and interact with Spark applications on EMR using Apache Livy
- Directly connect Step Functions to EMR
  - Create data processing and analysis workflows with minimal code and optimize cluster utilization

### Workflows in Glue

- Use workflows to create and visualize complex ETL tasks involving multiple crawlers, jobs, and triggers
- Manages the execution and monitoring of all components
- Glue console provides a visual representation of a workflow as a graph
- Chain interdependent jobs and crawlers
  - Event triggers fired by both jobs or crawlers, and can start both jobs and crawlers
- Views:
  - Static: shows the design of the workflow
  - Dynamic: run time view, shows the latest run information for each of the jobs and crawlers

## Orchestration of Glue and EMR Workflows
Several ways to operationalize Glue and EMR workflows
- Glue Workflows
- Automate workflow using Lambda
- Step Functions with Glue
- Step Functions with EMR and Apache Livy
- Step Functions directly with EMR

### Glue Workflows

A workflow is a grouping of a set of jobs, crawlers, and triggers in Glue
- Can design a complex multi-job ETL sequence that Glue can execute and track as single entity
- Create workflows using the AWS Console or the Glue API
- Console lets you see the components and flow of a workflow with a graph

### Automate Workflow using Lambda

Use Lambda functions and CloudWatch Events to orchestrate your workflow
- Start your workflow with Lambda Trigger
- Use CloudWatch Events to trigger other steps in your workflow

### Step Functions with Glue

Use Step Functions to automate your Glue workflow
- Serverless orchestration of your Glue steps
- Easily integrate with EMR steps

### Step Functions with EMR and Apache Livy

- Step Functions state machine starts executing, and the input file path of the data stored in S3 is passed to the state machine
- First step in the state machine triggers a Lambda function
- Lambda function interacts with Apache Spark running on EMR using Apache Livy, and submits a Spark job
- State Machine checks the Spark job status
- Based on the job status, the state machine moves to the success or failure state
- Output data from the Spark job is stored in S3

### Step Functions Directly with EMR

- Step Functions now interacts directly with EMR
- Use Step Functions to control all steps in your EMR cluster operation
- Also integrates other AWS servies into your workflow, e.g.
  - Lambda, AWS Batch, DynamoDB, ECSFargate
  - SNS, SQS, Glue, SageMaker, EMR, CodeBuild, StepFunctions

## Integrates with the following data stores
- Use S3 as an object store for Hadoop
- HDFS on the Core nodes instance storage
- Directly access and process data in DynamoDB
- Process data in RDS
- Use COPY command to load data in parallel into Redshift from EMR
- Integrates with S3 Glacier for low-cost storage


# Kinesis ETL (Streaming) Processing
- Use Kinesis Analytics to gain real-time insights into your streaming data
  - Query data in your stream or build streaming applications using SQL or Flink
  - Use for filtering, aggregation, and anomaly detection
  - Preprocess your data with lambda
