---
layout: post
title: Data Processing on AWS
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Data Processing, AWS]
mathjax: true
summary: Data Processing on AWS
---

# Data Processing on AWS

## Batch and Streaming processing model

Batch processing model
- Data is collected over a period of time, then run through analyitcs tools
- Time consuming, designed for large quantities of information that are not time-sensitive

Streaming processing model
- Data is processed in a stream, a record at a time or in micro-batches of tens, hundreds, or thousands of records
- Fast, designed for information that's needed immediately

## Services for Batch Processing ETL

AWS services used for batch processing

**Glue Batch ETL**
- Schedule ETL jobs to run at a minimum of 5-minute intervals
- Process micro-batches
- Serverless

**EMR Batch ETL**
- Use Impala or Hive to process batches of data
- Cluster of servers
- Very flexible in tool/application selection

## Services for Streaming Processing ETL

AWS services used for streaming processing

**Lambda**
- Reads records from your data stream, runs functions synchronously
- Frequently used with Kinesis
- Serverless

**Kinesis**
- Use KCL, Kinesis Analytics, Kinesis Firehose to process your data
- Serverless

**EMR Streaming ETL**
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


## Workflows in Glue

- Use workflows to create and visualize complex ETL tasks involving multiple crawlers, jobs, and triggers
- Manages the execution and monitoring of all components
- Glue console provides a visual representation of a workflow as a graph
- Chain interdependent jobs and crawlers
  - Event triggers fired by both jobs or crawlers, and can start both jobs and crawlers
- Views:
  - Static: shows the design of the workflow
  - Dynamic: run time view, shows the latest run information for each of the jobs and crawlers

# Orchestration of Glue and EMR Workflows
Several ways to operationalize Glue and EMR workflows
- Glue Workflows
- Automate workflow using Lambda
- Step Functions with Glue
- Step Functions with EMR and Apache Livy
- Step Functions directly with EMR

## Glue Workflows

A workflow is a grouping of a set of jobs, crawlers, and triggers in Glue
- Can design a complex multi-job ETL sequence that Glue can execute and track as single entity
- Create workflows using the AWS Console or the Glue API
- Console lets you see the components and flow of a workflow with a graph

## Automate Workflow using Lambda

Use Lambda functions and CloudWatch Events to orchestrate your workflow
- Start your workflow with Lambda Trigger
- Use CloudWatch Events to trigger other steps in your workflow

## Step Functions with Glue

Use Step Functions to automate your Glue workflow
- Serverless orchestration of your Glue steps
- Easily integrate with EMR steps

## Step Functions with EMR and Apache Livy

- Step Functions state machine starts executing, and the input file path of the data stored in S3 is passed to the state machine
- First step in the state machine triggers a Lambda function
- Lambda function interacts with Apache Spark running on EMR using Apache Livy, and submits a Spark job
- State Machine checks the Spark job status
- Based on the job status, the state machine moves to the success or failure state
- Output data from the Spark job is stored in S3

## Step Functions Directly with EMR

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
Use Kinesis Analytics to gain real-time insights into your streaming data
- Query data in your stream or build streaming applications using SQL or Flink
- Use for filtering, aggregation, and anomaly detection
- Preprocess your data with lambda
