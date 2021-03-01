## 172 - About the Serverless Section

- note saying these lectures are extracted from the developer course

#

## 173. Serverless Introduction

- Serverless is a new paradigm in which the developers dont have to manage servers anymore
- you just deploy code
- just deploy functions
- initially serverless = Functions as a Service
- Serverless was pioneered by AWS Lambda but now also includes anything that's managed: databases, messaging, storage, etc
- serverless does not mean there are no servers
  - it means you just don't manage/provision/see them
- Serverless in AWS
  - AWS Lambda
  - DynamoDB
  - AWS Cognito
  - AWS API Gateway
  - Amazon S3
  - AWS SNS & SQS
  - AWS Kinesis Data Firehose
  - Aurora Serverless
  - Step Functions
  - Fargate

#

## 174. Lambda Overview

- why AWS Lambda?
  - differences vs EC2
  - EC2
    - virtual server in the cloud
    - limited by RAM and CPU
    - Continuously running
    - scaling means intervention to add/remove servers
  - Lambda
    - virtual functions - no servers to manage
    - limited by time - short executions
    - run on-demand
    - scaling is automated
- benefits of Lambda
  - easy pricing
    - pay per request and compute time
    - free tier
  - integrated with AWS suite of services
  - integrated with many programming languages
  - easy monitoring through AWS CloudWatch
  - easy to get more resources per function(up to 3GB of RAM)
  - increasing RAM improves CPU and network
- supported languages
  - node
  - python
  - java
  - C#
  - Golang
  - C#
  - Ruby
  - custom runtime
- does not support Docker - must use ECS/Fargate

#

## 175. Lambda Hands On

- Lambda console
- create function
- notice: demo of how Lambda autoscales
- create function
- can choose from 3 options: author from scratch, use blueprint, browse serverless app repo
- give function name
- execution role
- create lambda function
- create test event
- test the test event
- it creates an iam role with permissions to log to CloudWatch

#

## 176. Lambda Limits

- per region
  - execution limits
  - memory allocation: 128mb - 3008mb
    - max execution time: 15minutes 900 seconds
    - environment variables (4kb)
    - disk capacity in the function container: 512mb
    - concurrency executions: 1000(can be increased by request)
  - deployment
    - lambda function deployment size(compressed zip): 50mb
    - size of uncompressed deployment(code+dependencies): 250mb
    - can use the /tmp directory to load other files at startup
    - size of environment variables: 4kb

#

## 177. Lambda @Edge

- example
  - you have deployed a CDN using cloudfront
  - what if you want to run a global AWS Lambda alongside?
  - or how to implement request filtering before reaching your application?
  - deploy Lambda Edge
- deploying Lambda functions alongside your CloudFront CDN
  - builds more responsive applications
  - you don't manage servers, Lambda is deployed globally
  - customize the CDN content
  - pay only for what you use
- how to use Lambda Edge
  - change cloudfront requests and responses:
    - viewer request - user to cloudfront
    - origin request - cloudfront to origin
    - origin response - origin to cloudfront
    - viewer response - cloudfront to user
- use case
  - create global application
  - website privacy and security
  - dynamic web application at the edge
  - search engine optimization
  - intelligently route across origins and data centers
  - bot mitigation at the edge
  - real time image transformation
  - A/B testing
  - user auth and authorization
  - user prioritization
  - user tracking and analytics

#

## 178. DynamoDB Overview

-

#

## 179. DynamoDB Hands-On

-

#

## 180. DynamoDB Advanced Features

-

#

## 181. API Gateway Overview

-

#

## 182. API Gateway Basics Hands-On

-

#

## 183. API Gateway Security

-

#

## 184. AWS Cognito Overview

-

#

## 185. Serverless Application Model(SAM) Overview

-

#

## Quiz 14: Serverless Quiz

-

#
