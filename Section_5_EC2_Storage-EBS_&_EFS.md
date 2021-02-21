## 56. EBS Intro

- concept:
  - an EC2 machine loses it root volume(main drive) when it is manually terminated
  - unexpected terminations might happen from time to time(AWS will email you)
  - sometimes, you need a way to store your instance data elsewhere
  - an EBS(elastic block store) volume is a network drive you can attach to your instances while they run
  - it allows your instances to persist data
- what is an EBS volume?
  - it's a network drive(i.e. not a physical drive)
  - it uses the network to communicate with the instance, which means there might be a bit of latency
  - it can be detached from the EC instance and attached to another one quickly
  - it is locked to an Availability Zone
    - meaning it's AZ scoped
    - to move a volume across AZs, you first need to make a snapshot
  - have to provision a capacity(size in GBs and IOPS)
    - you get billed for all the provisioned capacity, not just what you're using
    - you can increase over time
- EBS volume types
  - GP2(SSD): general purpose SSD volume that balances price and performance for a wide variety of workloads
  - IO 1(SSD): Highest peform SSD for mission critical low latency or high throughput workloads
  - ST 1(HDD): low cost HDD volume designed for frequent accessed, throughput intensive workloads
  - SC1(HDD: lowest cost HDD volume designed for less frequently accessed workloads
- EBS volumes are characterized in size, throughput -> IOPS - input output ops per second
- when in doubt, consult AWS documentaion, it's good!
- only GP2 and IO 1 can be used at boot volumes

#

## 57. EBS Intro Hands On

- add an EBS volume when creating an EC2 instance, in addition to the root volume
- attach a sec group
- launch it
- SSH into instance
- using "lsblk" command, you see all drives
- need to mount/format the new EBS volume to access it
- look at documentation for instructions on how to do that

#

## 58. EBS Volume Types Deep Dive

- EBS Volume Types and use cases:
  - GP2
    - recommended for most workloads
    - system boot volumes
    - virtual desktops
    - low latency interactive apps
    - development and test environments
    - range in size from 1gb to 16tb
    - small gp2 vols can burst IOPS to 3000
    - max IOPS is 16,000
    - 3 IOPS per GB, means at 5,334GB we are at max IOPS
  - IO1
    - critical business applications that require sustained IOPS performance or more than 16k IOPS per volume(the gp2 limit)
    - large database workloads such as:
      - MongoDB, Cassandra, Microsoft SQL Server, MySQL, PostgreSQL, Oracle
    - 4gb to 16tb
    - IOPS is provisioned(PIOPS) - min 100 max 64,000(Nitro instances) else max is 32,000
    - max ratio of PIOPS to requested volume size is 50:1
  - ST1
    - streaming workloads requiring constant, fast throughput at a low price
    - big data warehouses, log processing
    - Apache Kafka
    - cannot be root volume
    - 500gb - 16tb
    - max IOPS is 500
    - max throughput of 500mb/s - can burst
  - SC1
    - throughput-oriented storage for large volumes of data that is infrequently accessed
    - scenarios where the lowest storage cost is important
    - cannot be boot volume
    - 500gb - 16tb
    - max IOPS is 250
    - max throughput of 250 mb/s - can burst
- summary:
  - GP2 general purpose volumes - cheap
  - IO1 provisioned IOPS - expensive
  - ST1 - throughput optimized HDD
  - SC1 - cold HDD - infrequently accessed data

#

## 59. EBS Operation: Snapshots

-

#

## 60. EBS Operation: Volume Migration

-

#

## 61. EBS Operation: Volume Encryption

-

#

## 62. EBS vs Instance Store

-

#

## 63. EBS RAID Configurations

-

#

## 64. EFS Overview

-

#

## 65. EFS Hands On

-

#

## 66. EBS & EFS - Section Cleanup

-

#

## 67. EFS vs EBS

-

#

## Quiz 4: EC2 Data Management - EBS & EFS Quiz

-

#
