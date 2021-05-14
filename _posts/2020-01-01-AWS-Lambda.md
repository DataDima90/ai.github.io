---
layout: post
title: AWS Lambda
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Lambda, AWS]
mathjax: true
summary: AWS Lambda Overview
---

# AWS Lambda

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

- Uses replication and redundancy for high availability of the service and the functions it operates

**Durability and availability**
- No maintenance windows or scheduled downtime
- On failure, Lambda synchronoously responds with an exception
- Asynchronous functions are retried at least 3 times

![]({{ site.url }}/assets/img/posts/lambda.png)

**Scalability and elasticity**
- Scales automatically with no limits with dynamic capacity allocation

**Interfaces**
- Trigger Lambda from AWS servies events
- Respond to CloudTrail audit log entries as events

**Anti-patterns**
- Long running applications: 900 sec runtime
- Dynamic websites
- Stateful applications: must be stateless
