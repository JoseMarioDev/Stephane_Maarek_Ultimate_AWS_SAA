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

- incremental - only backup changed blocks
- EBS backups use IO and you shouldn't run them while your application is handling a lot of traffic
- snapshots are stored in S3, but you won't directly see them
- not necessary to detach volume to do snapshot, but recommended
- max 100,000 snapshots
- can copy snapshots across AZ or regions
- can make image(AMI) from snapshot
- EBS volumes restored by snapshots need to be pre-warmed(using fio or dd command to read the entire volume)
- snapshots can be automated using Amazon Data Lifecycle Manager

#

## 60. EBS Operation: Volume Migration

- EBS volumes are locked to a specific AZ
- to migrate it to a diff AZ(or region):
  - snapshot the volume
  - (optional) - copy the volume to a diff region
  - create a volume from the snapshot in the AZ of your choice

#

## 61. EBS Operation: Volume Encryption

- when you encrypt an EBS volume, you get the following:
  - data at rest is encrypted inside the volume
  - all the data in-flight moving between the instance and the volume is encrypted
  - all snapshots are encrypted
  - all volumes created from the snapshot are encrypted
- encryption/decryption are handled transparently - you dont have to do anything
- encryption has a minimal impact on latency
- EBS encryption leverages keys from KMS(AES-256)
- copying an unencrypted snapshot allows encryption
- snapshots of encrypted volumes are encrypted
- how to encrypt an unencrypted EBS volume
  1. create an EBS snapshot of the volume
  2. encrypt the EBS snapshot(using copy)
  3. create new EBS volume from the snapshot(volume will also be encrypted)
  4. now you can attach the encrypted volume to the original instance

#

## 62. EBS vs Instance Store

- some instances do not come with EBS root volumes instead:
- they come with "instance store" - ephemeral storage(temp, not lasting)
- instance store is physically attached to the machine(EBS is a network drive)
- pros:
  - better I/O performance
  - good for buffer/cache/scratch data/temporary content
  - data survives reboot
- cons:
  - on stop/termination, the instance store is lost
  - you can't resize the instance store
  - backups must be operated by user
- instance store details:
  - physical disk attached to the physical server where your EC2 instance is
  - very high IOPS(because it's physically attached)
  - disks up to 7.5tb(can change), can be striped to reach 30tb(can change over time)
  - block storage(like EBS)
  - cannot be increased in size
  - risk of data loss if hardware fails

#

## 63. EBS RAID Configurations

- EBS is already redundant storage(replicated within an AZ)
- but what if you want to increase IOPS to say 100k IOPS?
- what if you want to mirror your EBS volumes?
- you would mount volumes in parallel in RAID settings
- RAID is possible as long as your OS supports it - Linux/Windows
- some RAID options:
  1. RAID 0
  2. RAID 1
  3. RAID 5/RAID 6 - not recommended for EBS, see documentation
- for the exam and this lecture, focus on RAID 0/RAID 1
- RAID 0 - increase performance
  - combining 2/more vols and getting the total disk space and I/O - one logical volume
  - but if one disk fails, all the data fails
  - use cases: app that needs lots of IOPS and doesnt need to be fault tolerant
  - database that has replication already built in
  - using this, we have a very big disk with lots of IOPS
  - ex: 2 500gb EBS IO1 vols with 4k provisioned IOPS each will create:
    - 1000gb RAID 0 array with available bandwidth of 8000 IOPS and 1000 mb/s throughput
- RAID 1 - increase fault tolerance
  - one logical vol with 2 EBS volumes
  - EBS vol 1 mirrors -> vol 2
  - if one disk fails, our logical vol still works
  - we have to send the data to 2 EBS vols at the same time(2x network)
  - use case: app that needs increase volume fault tolerance
  - application where you need to service disks
- you have to configure this on your OS, not done on AWS console

#

## 64. EFS Overview

- Elastic File System
- managed NFS(network file system) that can be mounted on many EC2 instances
- EFS works with EC2 instances in multi-AZ
- HA, scalable, expensive, but you pay per use
- img in slide showing three instances in three diff AZs all connecting to the same EFS through a sec group
- use cases: content management, web serving, data sharing, wordpress
- uses NFSv4.1 protocol
- uses security groups to control access to EFS
- compatible with Linux bases AMIs - not Windows
- encryption at rest using KMS
- POSIX file system(Linux) that has a standard file API
- file system scales automatically, pay-per-use, no capacity planning
- considerations:
  - EFS scale - scales big
  - performance modes:
    - general purpose - default(web sever, CMS, etc)
    - max I/O - higher latency, throughput, highly parallel(big data, media processing)
  - storage tiers - lifecycle management feature - move file after N days
    - standard: for frequently accessed files
    - infrequent access(EFS-IA): cost to retrieve files, lower price to store

#

## 65. EFS Hands On

- move to EFS console, create FS
- general settings
  - name
  - backups
  - lifecycle mgmt
  - performance mode - general purpose/maxIO
  - throughput mode - bursting/provisioned
  - encryption
  - tags
- network settings
  - select VPC
  - can mount to diff AZs
  - define an SG for each AZ
  - review and create
- ssh into instance
- install efs-utils-package
- create efs directory
- mount efs drive using TLS encryption
- sec group needs to allow inbound traffic from EC2 to EFS

#

## 66. EBS & EFS - Section Cleanup

- cleanup from the hands on
- deleting the EFS
- deleting the instances
- deleting the EBS volumes
- deleting snapshots
- can also delete the SG not being used

#

## 67. EFS vs EBS

- EBS volumes
  - can be attached to only 1 instance at a time
  - scoped to one AZ at a time
  - types:
    - gp2
    - IO1
  - to migrate, take a snapshot, restore snapshot into another AZ
  - EBS backups use I/O. shouldnt run them wile app is handling traffic
  - root EBS vols get terminated by default if the EC2 instance gets terminated(can be disabled)
- EFS elastic file system
  - mounts to 100s of instances across multiple AZs
  - EFS share website files - wordpress
  - only for Linux
  - more expensive than EBS
  - can leverage EFS-IA for cost savings
  - only billed for what you use

#

## Quiz 4: EC2 Data Management - EBS & EFS Quiz

-

#
