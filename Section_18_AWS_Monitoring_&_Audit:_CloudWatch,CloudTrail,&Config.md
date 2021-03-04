## 201. AWS Monitoring - Section Introduction

- introduction

#

## 202. AWS CloudWatch Metrics

- overview
  - CW provides metrics for every service in AWS
  - Metric is a variable to monitor(cpu utilization, networkIn...etc)
  - metrics belong to namespaces
  - dimensions are an attribute of a metric(instance ID, environment, etc)
  - up to 10 dimensions per metric
  - metrics have timestamps
  - can create a CW dashboard of metrics
- AWS Cloudwatch EC2 detailed monitoring
- EC2 instance metrics have metrics every 5 minutes
- with detailed monitoring(for cost) you can get data every minute
- use detailed monitoring if you want to promptly scale your ASG
- AWS free tier allows us to have 10 detailed monitoring metrics
- AWS CW custom metric
  - possible to define and send your own custom metrics to CW
  - abilityt to use dimensions(attributes) to segment metrics
  - ex: instance.id, environment.name
  - metric resolution
    - standard: 1 minute
    - high resolution
  - use API called PutMetricData
  - use exponential back off in case throttle errors

## 203. AWS CloudWatch Dashboards

- great way to setup dashboards for quick access to key metrics
- dashboards are global; accessed from any region
- dashboards can include graphs from diff regions
- can change time zone and time range
- can setup automatic refresh
- pricing: 3 dashboards free; up to 50 metrics for free
- $3 per dashboard/month afterwards

#

## 204. AWS CloudWatch Logs

-

#

## 205. CloudWatch Agent & CloudWatch Logs Agent

- applications can send logs to CW using the SDK
- CW can collect logs from:
  - Beanstalk
  - ECS
  - Lambda
  - VPC flow logs
  - API GW
  - CloudTrail
  - CW
  - Route 53
- logs can be stored in
  - log groups: arbitrary name, usually representing an application
  - log streams: instances within an app/log files/containers
- can define log ex policy
- using the AWS CLI we can tail CW logs
- to send logs to CW, make sure IAMS permissions are correct
- logs can be encrypted using KMS at the group level
- CW logs can use filter expressions
  - metric filters can be used to trigger alarms
- CW log insights
  - can be used to query logs and add queries to CW dashboards
  -

#

## 206. AWS CloudWatch Alarms

-

#

## 207. EC2 Instance Recovery with CloudWatch Alarms

-

#

## 208. AWS CloudWatch Events

-

#

## 209. AWS CloudTrail

-

#

## 210. AWS Config - Overview

-

#

## 211. AWS Config - Hands On

-

#

## 212. CloudTrail vs CloudWatch vs Config

-

#

## Quiz 17: Monitoring Quiz

-

#
