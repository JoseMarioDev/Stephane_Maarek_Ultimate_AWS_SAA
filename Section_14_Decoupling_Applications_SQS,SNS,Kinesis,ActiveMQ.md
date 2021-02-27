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
  - consumers poll(check for msgs) the SQS queue.  can receive up to 10 messages at a time
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
    - trigggers an Cloudwatch Alarm
    - notifies the ASG to add more instances
- SQS to decouple application tiers
  - ex: app that renders videos
  - decouple into 2 apps 
    - first app receives the videos sends a msg to SQS to process the video
    - back end app polls SQS, gets msg, processes the videos; stores in S3 bucket when done
    - this way, you can use diff instances based on need for the FE and BE
    - decouples app so there is no bottlenecks
- SQS security
  - encrpytion
    - inflight using HTTPS API
    - at rest encrpytion usin KMS keys
    - client can also do client side encryption if it wants
  - access control
    - IAM policies to regulate access to the SQS API
  - SQS access policies
    - similar to S3 bucket policies
    - useful for cross account access to SQS queues
    - useful for allowing other services(SNS, S3...) write to SQS queue
#

## 157. SQS - Standard Queue Hands On

- 

#
