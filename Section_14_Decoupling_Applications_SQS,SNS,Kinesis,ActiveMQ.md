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

-

#

## 157. SQS - Standard Queue Hands On

-

#
