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

-

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
