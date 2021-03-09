## 273. Other Services Section Introduction

- not a deepdive, overview of a few services
- might be a question or 2 on the exam

#

## 274. CI/CD Introduction

- C/I
  - devs push code to a repo(github,codecommit,etc)
  - testing/build server checks the code as soon as it pushed(codebuild, jenkins,etc)
  - dev gets feedback about the tests and checks if pass/fail
  - find bugs early, fix bugs
  - deliver faster
  - deploy often
- C/D
  - ensure that software can be released reliably when needed
  - ensures deployments happen often and are quick
  - tools like codedeploy, jenkins, spinnaker
- tech stack for ci/cd
  - code, build, test, deploy, provision
  - codecommit -> codebuild-> beanstalk or (codedeploy/cloudformation)
  - orchestrate aws pipeline
  - ![CICD flow](img/24-CICD.png)

#

## 275. CloudFormation Intro

- infrastructure as code
  - code would deploy/crud our infrastructure
  - declarative way of outlining your AWS infrastructure
  - CF creates your config for you in the right order
- ability to destroy/recreate infrastructure on the fly
- templates have to be uploaded in S3 then referenced in CF
- to update a template, we cant edit previous ones. we have to reupload a new version of the template to AWS
- stacks are id'd by name
  deploy: edit template using YAML file
- components: resources, parameters, mappings, outputs, conditionals, metadata
- references and functions

#

## 276. CloudFormation Hands On

- CF in the console
- create stack
- specify template
- can view in designer, is gui for templates
- create stack
- can view status

#

## 277. CloudFormation - Extras

- CF StackSets
- CUD stacks across multiple regions and accts with a single operation
- admin acct to create stacksets
- trusted accts to CUD stack instances from stacksets
- when you update a stackset, all associated stack instances are updated throughout all accts and regions

#

## 278. ECS Introduction

- Elastic Container Service
- helps run Docker containers on EC2 instances
- diff services of ECS
  - ECS core: running ECS on user-provisoned EC2 instances
  - fargate: ECS on AWS provisioned machines(serverless)
  - EKS: running ECS on AWS powered Kubernetes(running on EC2)
  - ECR: docker container registry hosted by AWS
- ECS and Docker are very popular for microservices
- currently exam only ECS Core and ECR in scope
- slides
- ![slide](img/24-ecsconcepts.png)
- ![slide](img/24-ecsconfig.png)
- ![slide](img/24-ecsalb.png)
- ![slide](img/24-ecsecr.png)

#

## 279. ECS - Extras

- IAM task roles
  - the EC2 instance should have an IAM role allowing it access to the ECS service
  - each ECS task should have an ECS IAM task role to perform their API calls
- Fargate
  - ![fargate deploys containers serverlessly for us](img/24-ecsfargate.png)

#

## 280. EKS - Overview

- Elastic Kubernetes Service
- way to manage kubernetes clusters in AWS
- Kubernetes is open source software for auto deployment, scaling, and mgmt of containerized applications
- ![EKS](img/24-ecskubernetes.png)

#

## 281. Step Functions & SWF

- way to orchestrate lambda functions
- step functions is newer and recommended vs swf
- only use swf if you need to pass processes from children to parent
- slides
- ![slide](img/24-step.png)
- ![slide](img/24-stepworkflow.png)
- ![slide](img/24-simpleworkflow.png)

#

## 282. EMR

- Elastic map reduce
- helps create hadoop clusters for processing large data
- ![slide](img/24-emr.png)

#

## 283. AWS Glue

- newer service, fully managed ETL service
- extract, transform, load
- move data from one source to target and change the data format so it's compatible
- ![slide](img/24-glue.png)

#

## 284. OpsWorks

- think chef/puppet
- similar to SSM
- use when your onprem is using chef/puppet
- configuration as code
- ![slide](img/24-opsworks.png)
- ![slide](img/24-chefpuppet.png)

#

## 285. ElasticTranscoder

- convert media files stored in S3 into various formats
- optimize bitrate, thumbnail, watermarks, captions, etc
- 4 components
  - job,pipeline,presets,notifications
- pay for what you use, scales, fully managed

#

## 286. AWS Workspaces

- managed secure cloud desktop
- on demand pay per usage
- secure,encrypted, network isolation
- integrated with MS AD
- manage on prem VDI (virtual desktop interface)

#

## 287. AppSync

- store sync data across mobile app in real time
- uses graphql
- client code can generate automatically

#

## 288. Other Services: cheat sheet

- Other Services: Cheat Sheet
  Here's a quick cheat-sheet to remember all these services:

CodeCommit: service where you can store your code. Similar service is GitHub

CodeBuild: build and testing service in your CICD pipelines

CodeDeploy: deploy the packaged code onto EC2 and AWS Lambda

CodePipeline: orchestrate the actions of your CICD pipelines (build stages, manual approvals, many deploys, etc)

CloudFormation: Infrastructure as Code for AWS. Declarative way to manage, create and update resources.

ECS (Elastic Container Service): Docker container management system on AWS. Helps with creating micro-services.

ECR (Elastic Container Registry): Docker images repository on AWS. Docker Images can be pushed and pulled from there

Step Functions: Orchestrate / Coordinate Lambda functions and ECS containers into a workflow

SWF (Simple Workflow Service): Old way of orchestrating a big workflow.

EMR (Elastic Map Reduce): Big Data / Hadoop / Spark clusters on AWS, deployed on EC2 for you

Glue: ETL (Extract Transform Load) service on AWS

OpsWorks: managed Chef & Puppet on AWS

ElasticTranscoder: managed media (video, music) converter service into various optimized formats

Organizations: hierarchy and centralized management of multiple AWS accounts

Workspaces: Virtual Desktop on Demand in the Cloud. Replaces traditional on-premise VDI infrastructure

AppSync: GraphQL as a service on AWS

SSO (Single Sign On): One login managed by AWS to log in to various business SAML 2.0-compatible applications (office 365 etc)

---

Remember, you should only get high level questions on these technologies

Hope that helps!

#

## Quiz 23: Other Services: Quiz

-

#
