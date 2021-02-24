## 92. Solutions Architecture Discussions Overview

- how the diff technologies all fit together
- section focuses on solution architecture, focusing on case studies

#

## 93. WhatsTheTime.com

- stateless web application

- website that eventually scales to multiAZ with ELB and ASG using an alias record on rt 53 to connect to the ELB. Also using security groups on the EC2 instances to make sure they only communicate with the ELB

- focuses on the 5 pillars of a well architected framework

1. cost
2. performance
3. reliability
4. security
5. operational excellence

#

## 94. MyClothes.com

- adds on the arch from previous lecture
- can use ELB stickiness
- introduces Elaticache for storing user sessions
- can also store them in DynamoDB
- introduces RDS for storing user info(address, etc)
- read replicas to improve performance
- MultiAZ standby for DR
-

#

## 95. MyWordPress.com

- adds Aurora DB to have easy MultiAZ and read-replica
- storing data in EBS is okay for single instance but:
- storing data in EFS is better for distributed applications when we have multiple instances/AZ
-

#

## 96. Instantiating Applications Quickly

- EC2 instances:
  - use a golden AMI: install apps, OS dependencies, before hand and launch your instances from that AMI
  - Bootstrapping using user data: for dynamic configuration details, user data scripts
  - hybrid: mix golden AMI and user data scripts
    - Elastic Beanstalk does this for us
- RDS DBs:
  - restore from snapshot: the database will have schemas and data ready
- EBS volumes:
  - restore from snapshot: the disk will be formatted and have data

#

## 97. Beanstalk Overview

- developer problems on AWS:
  - managing infrastructure
  - deploying code
  - configuring all the DBs, load balancers, etc
  - scaling concerns
  - most web apps have the same arch (ELB, ASG)
  - all devs want is to run their code
  - possible consistent across diff applications and environments
- Elastic Beanstalk is:
  - developer centric view of deploying apps on AWS
  - it uses all the components we've seen before:
    - EC2, ASG, ELB, RDS, etc..
  - but it's all in one view that easy to use
  - we still have full control over the config
  - Beanstalk is free but you pay for the underlying instances
  - managed service
    - instance config and OS handled by Beanstalk
    - deployment strategy is configurable but performed by Elastic Beanstalk
    - just the app code is the responsibility of the developer
    - three architecture models:
      1. single instance deployment - good for devs
      2. LB+ASG: great for preprod/prod web apps
      3. ASG only: great for non web apps in production(workers, etc)
  - Elastic Beanstalk has three components
    1. application
    2. application version: each deployment gets assigned a name
    3. environment name: dev/prod/test - free naming
  - you deploy application versions to environments and can promote application versions to the next environment
  - rollback feature to previous application version
  - full control over lifecycle of environments

#

## 98. Beanstalk Hands On

- go the Elastic Beanstalk console
- create web application
- name
- tags
- platform
- application code
- configure options
- create app
- click on the link and you see your app is running on the cloud

#

## Quiz 7: Solutions Architecture Discussions - Classic

-

#
