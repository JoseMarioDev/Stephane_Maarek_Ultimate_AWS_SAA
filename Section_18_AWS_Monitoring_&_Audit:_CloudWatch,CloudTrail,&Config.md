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

#

## 205. CloudWatch Agent & CloudWatch Logs Agent

- by default no logs go from EC2->CW
- you need to run a CW agent on EC2 to push log files you want
- make sure IAM permissions are correct
- CW log agent can be installed on EC2 instances and onprem as well
- there are 2 types of log agents:
  - CW logs agent
    - old version of agent
    - can only send to CW Logs
  - unified agent
    - collects additional system level metrics such as RAM, processes, etc...
    - also collects logs to send to CW Logs
    - better because it can do both metrics and logs
    - can also configure agent easily using the SSM parameter store
- unified agent metrics
  - collected directly on your linux server/EC2 instance
  - types of metrics you can create
    - CPU(active,guest,idle,system,user,steal)
    - disk metrics
    - RAM
    - netstat
    - processes
    - swap space
- reminder: out of the box metrics for EC2 - disk, CPU, network(high level)

#

## 206. AWS CloudWatch Alarms

- alarms are used to trigger notifications for any metrics
- can be attached to any ASG, EC2 actions, SNS notifications
- various options(sampling, %, max,min,etc)
- alarm states:
  - ok
  - insufficient_data
  - alarm
- period: length of time in seconds to evaluate metric
- high resolution custom metrics: only choose 10 sec or 30 sec

#

## 207. EC2 Instance Recovery with CloudWatch Alarms

- you can have a status check on your EC2 instances
  - types:
  - instance status - checks the EC2 vm
  - system status - checks the underlying hardware
- you can have a metric called StatusCheckFailed_System
- if this fails, triggers an alarm that starts an action called Instance Recovery
- same private, public, elastic iP, metadata, placement group
- you lose any data that is on the instance store(locally)
- EBS data can be saved

#

## 208. AWS CloudWatch Events

- can use CW events to:
- schedule Cron jobs
- event pattern: event rules to react to a service doing something
- trigger to Lambda functions, SQS, SNS, Kinesis msgs
- CW Events create a small jSON doc to gie information about the change

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
