---
layout: post
title: AWS Data Analysis and Visualization
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Data Analysis, Data Visualization, AWS]
mathjax: true
summary: AWS Data Analysis and Visualization
---

# Data Analysis and Visualization on AWS

## Analysis and Visualization Services - Data Lake

- With a data lake you can use your analytics without moving your data to another system
- Access vast amounts of structured, unstructured, and semi-structured data in your data lake
  - Lake Formation provides multi-service, fine-grained access control to data
  - Macie helps detect sensitive data that may have been stored in the wrong place
  - Inspector identfies configuration errors that could lead to data breaches

![]({{ site.url }}/assets/img/posts/datalake.png)

## Analysis and Visualization Services - Redshift

- With Redshift you can run queries on your Redshift cluster data warehouse directly
- Two options: query via AWS management console using the query editor, or via a SQL client tool
  - Use the query editor on the Redshift console, no need for a SQL client application
  - Supports SQL client tools connecting through JDBC and ODBC
  - Using the Redshift API you cann access Redshift data with web services-based applications, including AWS Lambda, AWS AppSync, SageMaker notebooks, and AWS cloud9

![]({{ site.url }}/assets/img/posts/datawarehouse.png)

## Analysis and Visualization Services - Athena and Glue

- With Athena and Glue you can directly query data on S3
- Glue crawls your data, categorizes and cleans it and moves to your data store
- Athena queries data sources that are registered with the AWS Glue Data Catalog
  - Running Data Manipulation Language (DML) queries in Athena via the Data Catalog uses the Data Catalog schema to derive insight from the underlying dataset
  - Run a Glue crawler on a data source from within Athena to create a schema in the Glue Data Catalog
  
![]({{ site.url }}/assets/img/posts/athenaglue.png)

## Analysis and Visualization Servies - Redshift Spectrum

- Use Redshift Spectrum to build queries that combine your Redshift warehouse data with data from S3
- Query data directly from files on S3
- Need a Redshift cluster and a SQL client connected to your cluster to execute SQL commands
- Visualize data via QuickSight
- Cluster and data in S3 must be in the same region

![]({{ site.url }}/assets/img/posts/redshiftspectrum.png)
