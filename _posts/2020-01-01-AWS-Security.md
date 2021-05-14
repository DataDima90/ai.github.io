---
layout: post
title: Security on AWS
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Security, AWS]
mathjax: true
summary: Security on AWS
---

# IAM Authentication

To set permissions for an identity in IAM, choose an AWS managed policy, a customer managed policy, or an inline policy.

**AWS managed policy**
- Standalone policy that is created and administered by AWS
- Provide permissions for many common use cases: full, power-user, and partial access

**Customer managed policy**
- Standalone policies that you administer in your AWS account

**Inline policy**
- strict one-to-one relationship of policy to identity
- Policy embedded in an IAM identity (a user, group or role)

## Security Use Cases

IAM best practices of how to apply IAM and other access controls

**IAM User**
- Use AWS CLI to create resources in AWS account

**IAM Role**
- Create Glue crawlers and ETL jobs that access S3 buckets
- Single sign on into corporate network and applications

**Cognito**
- Web application that needs to access DynamoDB through a REST endpoint

# IAM Authorization

- Use IAM identity- and resource-based permissions to authorize access to analytics resources
- Policy is an object in AWS you associate with an identity or resource to define the identity or resource permissions
- To use a permissions policy to restrict access to a resource you choose a policy type
  - Identity-based policy
    - Attached to an IAM user, group or role
    - Specifiy what an identity can do (its permissions)
    - Example: attach a policy to a user that allows that user to perform the EC2 RunInstances action
  - Resource-based policy
    - Attached to a resource
    - Specify which users have access to the resource and what actions they can perform on it
    - Example: attach resource-based policies to S3 buckets, SQS queues, and Key Management Service encryption keys




