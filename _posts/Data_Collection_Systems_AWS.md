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

Options for real-time data streaming solutions to the cloud using different technologies.
Amazon Web Servies (AWS) provides various services to assist in ingesting data, such as:
- Kinesis Data Streams,
- Kinesis Video Streams,
- Kinesis Firehose,
- Managed Kafka (MSK), and
- MQ


**What is Amahon Kinesis Data Streams?**
Amazon Kinesis Data Streams is a highly scalable and durable managed streaming service. Use cases include aggregating data from financial markets driving real-time trading decisions to analyzing sensor data at a warehouse to determine when a machine requires servicing or breaks.

**Issue**
Unlike some other AWS services, Kinesis does not provide a native auto-scaling solution like DynamoDB On-Demand or EC2 Auto Scaling. Therefore, there is a need for the right number of shards to be calculated for every stream based on the expected number of records and/or the size of the records. This can lead to over/under-provisioning of shards within a stream resulting in higher costs and/or data ingestion being throttled.


**Implications**
Over/under-provisioning
The obvious implication of over-provisioning is overpaying. Under provisioning results in throttled data ingestion which leads to the next point.

**Throttled data ingestion**
The capacity of a stream is determined by the number of shards in the stream. Each shard can handle up to 1000 records/second or 1MiB/second. Attempting to write more than the provisioned capacity will result in throttling.

**What’s available out of the box?**
Amazon makes it simple to scale the stream up or down using the UpdateShardCount, MergeShards, and SplitShard APIs. But, in the absence of an intelligent mechanism for scaling the stream, the responsibility of calling the APIs for increasing/decreasing the number of shards is left to the producer(s). 

### Notifications

What are Shards?
Amazon Kinesis Data Streams are made up of shards. A shard represents a sequence of records in a stream. It is also used to represent the base throughput unit:

One shard can:
- Ingest 1 MiB/second or 1,000 records/second.
- Support up to a maximum total data read rate of 2 MiB/second via the GetRecords API.

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

- CloudWatch Metrics can be captured at the stream level or the shard level. IncomingBytes, IncomingRecords, or WriteProvisionedThroughputExceeded are metrics of interest, which are all available for streams and shards.

Scaling frequency: By default, the UpdateShardCount API can only be called a certain number of times per rolling 24-hour period. 

Max and Min Shard Counts: During scale out and scale in operations, the number of shards can be increased to a maximum of 2x and a minimum of 0.5x, where x is the current number of open shards in the stream.

Shard Count Increments: When using the UpdateShardCount API, AWS recommends a target Shard count which is a multiple of 25% (e.g., 25%, 50%, 75%, 100%) of the current open shard count. Any target value within shard limits can be specified. However, if a target is specified which is not a multiple of 25%, the scaling action might take longer to complete. 



All articles I read about Data Collection Systems:

[https://comcastsamples.github.io/KinesisShardCalculator/](https://comcastsamples.github.io/KinesisShardCalculator/)
[https://medium.com/slalom-data-analytics/amazon-kinesis-data-streams-auto-scaling-the-number-of-shards-105dc967bed5](https://medium.com/slalom-data-analytics/amazon-kinesis-data-streams-auto-scaling-the-number-of-shards-105dc967bed5)
[https://towardsdatascience.com/delivering-real-time-streaming-data-to-amazon-s3-using-amazon-kinesis-data-firehose-2cda5c4d1efe](https://towardsdatascience.com/delivering-real-time-streaming-data-to-amazon-s3-using-amazon-kinesis-data-firehose-2cda5c4d1efe)
[https://medium.com/swlh/create-kinesis-firehose-data-stream-from-iot-core-to-s3-using-serverless-framework-2af1d0b35500](https://medium.com/swlh/create-kinesis-firehose-data-stream-from-iot-core-to-s3-using-serverless-framework-2af1d0b35500)
[https://medium.com/dataseries/how-to-build-a-simple-data-lake-using-amazon-kinesis-data-firehose-and-amazon-s3-1a6a6887f441](https://medium.com/dataseries/how-to-build-a-simple-data-lake-using-amazon-kinesis-data-firehose-and-amazon-s3-1a6a6887f441)
[https://faun.pub/apache-kafka-vs-apache-kinesis-57a3d585ef78](https://faun.pub/apache-kafka-vs-apache-kinesis-57a3d585ef78)
