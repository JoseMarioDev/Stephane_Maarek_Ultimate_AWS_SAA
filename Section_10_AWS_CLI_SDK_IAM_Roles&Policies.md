## 112. Developing on AWS introduction

- introducing the cli
- learn about sdk, iam polices, roles
- developing and performing AWS tasks against AWS can be done in several ways
  - using the AWS cli on our local computer
  - using the AWS cli on our EC2 machines
  - using the AWS SDK on our local computer
  - using the AWS SDK on our EC2 machines
  - using the AWS instance metadata service for EC2

#

## 113. AWS cli setup for windows

- how to install on windows

#

## 114. AWS cli setup on Mac OS X

- install on Mac

#

## 115. AWS cli Setup on Linux

- install on Linux

#

## 116. AWS cli Configuration

- connect to AWS network with cli over the www.
- do not share your access and secret keys!
- aws configure in the command prompt
- enter access key
- enter secret key
- choose region

#

## 117. AWS CLI on EC2

- AWS CLI on EC2 ....the bad way
  - run aws configure like we did locally
  - but it's insecure
  - never put your personal creds on EC2
  - if EC2 instance is compromised, so is your info
- AWS CLI on EC2...the right way
  - for EC2 there is a better way - IAM Roles
  - IAM roles are attached to EC2 instances
  - IAM roles come come with a policy authorizing exactly what the EC2 instance should be able to do
  - EC2 instances can then use these profiles automatically without any additional configuration
  - this is best practice on AWS
- in the IAM console, create a role
  - select trust entity -> AWS service
  - create policy
  - create role
- in the EC2 console
  - instance settings
  - attach/replace IAM role
  - attach the role
  - apply it
- policy may not kick in immediately, give it time to trickle down from IAM to EC2
- IAM roles can be attached to many instances, EC2 instances can only have one IAM Role

#

## 118. AWS CLI Practice with S3

- practice the commands
- can be found in AWS documentations
- popular commands:
  - aws S3 ls: lists contents of bucket
  - aws s3 cp: copy
  - aws s3 mb: make bucket
  - aws s3 rb: remove bucket

#

## 119. IAM Roles and Policies Hands On

- you can use a managed policy or create your own
- you can also add an inline policy - not recommended
- create your own policy using a visual editor or JSON editor

#

## 120. AWS Policy Simulator

- testing policy using AWS policy simulator
- it's a website you can use to test your policies

#

## 121. AWS EC2 Instance Metadata

- allows EC2 instances to "learn about themselves" without using an IAM role for that purpose
- url is http://169.254.169.254/latest/meta-data
- you can retrieve the IAM Role name from the metadata, but you cannot retrieve the IAM Policy
- metadata = info about the EC2 instance
- userdata = launch script of the EC2 instance

#

# 122. AWS SDK Overview

- what if you want to perform actions on AWS directly from your app code?
  - without using the CLI
- you can use the SDK
- official SDKs come in many languages
- we have to use the AWS SDK when coding against the AWS services such as dynamodb
- AWS SDK credentials security
  - recommended to use the default crediential provider chain
    - works seemlessly with:
      - AWS creds
      - instance profile creds
      - env variables
  - overall, never store AWS creds in your code
- exponential backoff
  - any api that fails because of too many calls needs to be retried with exponential backoff
  - these apply to rate limited API
  - retry mechanism included in SDK API calls

#

## Quiz 9: CLI & SDK Quiz

-

#
