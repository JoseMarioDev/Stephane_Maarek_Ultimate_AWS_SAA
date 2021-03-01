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

- fully managed, highly available with replication across 3 AZs by default
- NoSQL database - not a relational database
- scale to massive workloads, distributed database
- millions of requests per seconds, scales largely
- fast and consistent performance
- integrated with IAM
- enables event driven programming w/DynamoDB Streams
- low cost and autoscaling capabilities
- basics
  - made of tables
  - each table has a primary key(must be decided at creation time)
  - each table has an infinite number of items(=rows)
  - each item has attributes(think columns) can be added over time- can be null
  - max size of an item is 400kb
  - data types supported are: scalar types, document types, set types
- provisioned throughput
  - tables must have provisioned read and write capacity units
  - Read Capacity Units(RCU)
  - Write Capacity Units(WCU)
  - option to setup autoscaling of throughput to meet demand
  - throughput can be exceeded temporarily using "burst credit"
  - if burst credits are empty, you'll get a "provisionedthroughputexception"
  - its then advised to do an exponential back-off retry

#

## 179. DynamoDB Hands-On

- dynamoDB console
- create table
- no need to create a db, it's a fully managed service
- table name
- primary key
- read/write capacity mode
  - provisioned or ondemand
  - specify RCUs and WCUs
  - there is a calculator to help you figure out how much you need
- can setup autoscaling
- enable encryption at rest
- create table

#

## 180. DynamoDB Advanced Features

- DAX
  - DDB Accelerator
  - seamless cache for DDb, no application rewrite
  - writes go through DAX to DDB
  - microsecond latency for cached reads and queries
  - solves the hot key problem(too many reads)
  - 5 minute TTL for cache by default
  - up to 10 nodes in a cluster
  - multiAZ - min 3 modes recommended for production
  - secure(encryption at rest with KMS, VPC, IAM, Cloudtrail...)
- DynamoDB Streams
  - integrate DDB w/Lambda functions
  - changes in DDB(Create,Update,delete) can endup in a stream
  - this stream can be read by AWS Lambda to do:
    - react to changes in real time(welcome email to new users)
    - analytics
    - create derivative tables
    - insert into ElasticSearch
  - could implement cross region replication using streams
  - stream as 24 hours of data retention
- new features
  - transactions - all or nothing , coordinate insert,update,deletes across multiple tables
  - on demand - scales automatically, no capacity planning. useful for spikes
- security
  - VPC endpoints to access w/o internet
  - access controlled by IAM
  - encryption at rest using KMS
  - intransit using SSL/TLS
- backup/restore features
  - point in time like RDS
  - no performance impact
- Global tables
  - multi region, fully replicated, high perform
- Amazon DMS can be used to migrate to DDB
- you can launch a local DDB instance on your computer for development purposes
- global tables - CRR. must enable streams, active active replication, many regions
- capacity planning - can provision RCU/WCU and autoscaling. can use ondemand capacity. more expensive

#

## 181. API Gateway Overview

- serverless offering that allows us to create REST APIs that are going to be public and accessible for clients
- client talks to API GW and it proxies request to Lambda function
- features
  - can use Lambda and APIGW to create serverless option.  No infrastructure to manage
  - supports websockets protocol
  - Handles API versioning
  - handles dif environments - dev,testing,prod
  - handles security - authentication and authorization
  - create API keys, handle request throttling
  - compatible with Swagger and OpenAPI to quickly define APIs
  - transform and validate req/res 
  - generate SDK and API specs
  - cache API responses
- integrates with - high level
  - Lambda functions
    - invoke lambda functions
    - easy way to expose REST APIs backed by AWS Lambda
  - HTTP
    - expose HTTP endpoints in the backend
    - why? add rate limiting, caching, user auth, api keys, etc
  - AWS Services
    - expose any AWS api through GW
    - ex: start an AWS step function workflow, post msg to SQS
- APIGW endpoint types
  - Edge optimized - default reqs are routed through cloudfront edge locations
  - regional - for clients within the same region
  - private - vpc access only, manage permissions through IAM

#

## 182. API Gateway Basics Hands-On

- APIGW console
- choose type of API you wanna build
- ex REST API
- build
- create API
- settings 
- endpoint type
- creat the API
- taken to the actions page
- create method
- GET request
- deploy API when ready

#

## 183. API Gateway Security

- IAM Permissions
 - creat an IAM auth policy and attach user/role
 - APIGW verifies IAM permissions passed when calling the applications
 - uses "SIG v4" capability where IAM creds are in the headers
- Lambda Authorizer - most common
  - uses AWS Lambda to verify token being passed in the header
  - option to cache result of authentication
  - helpful when using OAuth, SAML, 3rd party type of auth
  - Lambda must return an IAM policy for the user
- Cognito user pools
  - Cognito fully manages user lifecycle
  - APIGW automatically verifies identity from AWS Cognito
  - no custom implmentation required
  - Cognito only helps with Authentication, not authorization
- Summary
  - IAM
    - great for users/roles with AWS accounts
    - handles authentication and authorization
    - leverages SIG v4
  - lambda authorizer aka Custom Authorizer
    - great for 3rd party tokens
    - very flexible in terms of what IAM policy is returned
    - handles authentication and authorization
    - pay per Lambda invocation
  - Cognito user pools
    - you manage your own user pool(can be backed by FB, Google)
    - no need to write custom code
    - only authentication pattern



#

## 184. AWS Cognito Overview

- when we want to give our users an identity so that they can still interact with our app
- 3 ways to do:
  - Cognito User Pools
    - signin functionality for app users
    - integrate with APIGW
  - Cognito Identity Pools-Federated Identity
    - provide AWS creds to users so they can access AWS resources
    - integrate with Cognito User Pools as an identity provider
  - Cognito Sync
    - sync data from device to Cognito
    - replaced by AppSync
  - Cognito User Pools(CUP)
    - serverless db of user for your mobile app
    - simple login/email/password combination
    - possible to verify emails/phone numbers and add MFA
    - can enable federated identities(FB,Google,SAML)
    - sends back JWT
    - can be integrated with APIGW for authentication
  - Federated Identity Pools(FIP)
    - goal - provide direct access to AWS resources from client side
    - how - log in to federated identity provider, get temp creds from AWS with predefined IAM policy permissions
    - ex: provide temp access to S3 bucket using facebook login
  - Cognito Sync
    - deprecated - use AppSync now
    - store preferences, configuration, state of application
    - cross device sync-ios, android
    - offline capable
    - requires FIP
    - stores data in datasets(up to 1mb) up to 20 data sets

#

## 185. Serverless Application Model(SAM) Overview

- framework for devloping and deploying serverless applications
- all the config is in YAML
- lambda functions
- dynamodb tables
- apigw
- cognito user pools
- SAM can help you run your Lambda, APIGW, DDB locally
- SAM can use codeDeploy to deploy Lambda functions


#

## Quiz 14: Serverless Quiz

-

#
