# 155. Introduction to Messaging

- when we deploy apps, they will need to communicate with each other
- 2 patterns of app communication
  - sync: app to app
  - async/event based: app to queue to app
- sync between apps can be a problem if there are spikes of traffic
  - ex: video encoding service with spike of service
- better to decouple your apps
  - using SQS: queue model
  - using SNS: pub/sub model
  - using Kinesis: real time streaming model
- these services can scale independently from our app

#

## 156. Amazon SQS - Standard Queues Overview

- client sends a messaage to the queue is called a producer
  - could one/more than one producer
- receivers are called consumers
  - they poll the service to check for messages
  - consumer process message, delete from queue
- fully managed service used to decouple applications
- attributes:
  - unlimited throughput
  - unlimited num of messages in queue
  - retention: default 4 days; max is 14 days
  - low latency <10ms
  - limitation of 256kb
  - can have duplicate messages - at least once delivery
  - can have out of order messages - best effort ordering
- SQS Producers
  - produced to SQS using the SDK(sendmessage API)
  - message is persisted in SQS until a consumer deletes it
- SQS Consumers
  - applications running on EC2 instances, servers, or AWS Lambda
  - consumers poll(check for msgs) the SQS queue. can receive up to 10 messages at a time
  - process messages and do whatever coding needs to be done
  - consumer deletes msg from SQS using the deletemessage API
- SQS multiple EC2 instance consumers
  - scale this up
  - consumers receive and process msgs in parallel
  - that why there is "at lesat once" delivery
  - best effort msging order
  - consumers delete msgs after processing
  - we can scale consumers horizontally to improve throughput of processing
- SQS w/ASG
  - setup a cloudwatch metric
    - ex: approximateNumberOfMessages reaches certain length
    - triggers an Cloudwatch Alarm
    - notifies the ASG to add more instances
- SQS to decouple application tiers
  - ex: app that renders videos
  - decouple into 2 apps
    - first app receives the videos sends a msg to SQS to process the video
    - back end app polls SQS, gets msg, processes the videos; stores in S3 bucket when done
    - this way, you can use diff instances based on need for the FE and BE
    - decouples app so there is no bottlenecks
- SQS security
  - encryption
    - inflight using HTTPS API
    - at rest encryption using KMS keys
    - client can also do client side encryption if it wants
  - access control
    - IAM policies to regulate access to the SQS API
  - SQS access policies
    - similar to S3 bucket policies
    - useful for cross account access to SQS queues
    - useful for allowing other services(SNS, S3...) write to SQS queue

#

## 157. SQS - Standard Queue Hands On

- SQS console
- create message
- choose standard vs FIFO
- configuration
- retention period
- access policy
- basic/advanced
- encryption
- create queue
- top right hand side, send and rec msgs
-

#

## 158. SQS - Message Visibility Timeout

- after a msg if polled by a consumer, it becomes invisible to other consumers
- default the "message visibility timeout" is 30 seconds
- that means the msg has 30 seconds to be processed
- after the msg visibility timeout is over, the message is visible in SQS
- if a msg is not processed within the visibility timeout, it will be processed twice
- a consumer could call the ChangeMessageVisibility API to get more time
- can change default setting in configuration of the queue

#

## 159. SQS - Dead Letter Queues

- if consumer fails to process msg within the visibility timeout, the msg goes back into queue
- we can set a threshold of how many times a msg can go back to queue
- after the MaximumReceives threshold is exceeded, the msg goes into a dead letter queue(DLQ)
- useful for debugging
- make sure to process the msgs in the DLQ before they expire
- good idea to check retention time
- config of original queue, enable DLQ and specify the arn of the DLQ, configure settings

#

## 160. SQS - Delay Queues

- delay a msg so consumers dont see immediately. up to 15 mins
- default is 0 seconds - msgs avail right away
- can set a default at queue level
- can override the default on send using the DelaySeconds parameter

#

## 161. SQS - FIFO queues

- first in first out
- ordering guarantee
- has limited throughput
- 300 msg/per second without batching
- exactly once send capabilities by removing dupes
- messages processed in order by the consumer
- select FIFO when creating queues
- must name queue with .fifo extension
  - example: myqueue.fifo

#

## 162. SQS + Auto Scaling Group

- scale num of instances based on num of msgs in queue
- create a CloudWatch custom view metric
  - que length divided by num of instances
  - create alarm when it reaches a certain number
  - step scaling policy when it's above/below the metric
- this is how we create decoupling

#

## 163. Amazon SNS - Overview

-

#
