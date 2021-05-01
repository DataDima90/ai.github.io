---
layout: post
title: Data Collection Systems on AWS
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [AWS, Kinesis]
mathjax: true
summary: Data Collection Systems
---

# Kinesis



### Notifications

**Metrics**

Producers
Record Aggregation
- We can decide whether or not the producer is using Kinesis record aggregation. 
This feature is available in the KPL (Kinesis Producer Library) and allows us to group user records into fewer larger aggregated records.
- Kinesis Record Aggregation increases throughput and reduces cost, at the expense of latency.

Consumers
Enhanced Fan-Out
- We can decide whether or not the consmer is using Kinesis Enhanced Fan-out feature. 
It essentially isolates this consumer from the other consumers, so that the fna-out consumers each have a dedicated, 2MB/s/shard bandwith.
Maximum Consumption Speed
- This is the maximum number of records that a single consumer instance (i.e. process) can handle. This assumes that a single shard is consumed
by exactly one consumer process. It impacts the shard count because it can limit the actual throughput on a shard.


All articles I read about Data Collection Systems:
[https://comcastsamples.github.io/KinesisShardCalculator/](https://comcastsamples.github.io/KinesisShardCalculator/)
