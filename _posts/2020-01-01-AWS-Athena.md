---
layout: post
title: AWS Athena
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Athena, AWS]
mathjax: true
summary: AWS Athena Overview
---

# AWS Athena

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
