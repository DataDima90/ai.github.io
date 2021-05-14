---
layout: post
title: AWS Kinesis
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Kinesis, AWS]
mathjax: true
summary: AWS Kinesis Overview
---

# Kinesis

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

**Analytics Solutions Patterns with Kinesis**

Streams data to analytics processing solutions
- Video analytics applications
- Real-time analytics applications
- Analyze IoT device data
- Blog posts and article analytics
- System and application log analytics

![]({{ site.url }}/assets/img/posts/kinesispattern.png)
