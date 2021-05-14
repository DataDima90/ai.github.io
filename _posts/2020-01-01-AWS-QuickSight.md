---
layout: post
title: AWS QuickSight
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [QuickSight, AWS]
mathjax: true
summary: AWS QuickSight Overview
---

# AWS QuickSight

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
