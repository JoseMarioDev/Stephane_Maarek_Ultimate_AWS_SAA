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
-  AWS Cloudwatch EC2 detailed monitoring
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

-

#

## 204. AWS CloudWatch Logs

-

#

## 205. CloudWatch Agent & CloudWatch Logs Agent

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
