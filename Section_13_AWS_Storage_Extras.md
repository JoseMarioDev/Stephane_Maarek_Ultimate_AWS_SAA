## 147. Snowball Overview

- physical data transport solution that helps move TBs or PBs of data in or out of AWS
- alternative to moving data over network(and paying network fees)
- secure, KMS-256 encyption
- track using SNS and txt msgs. E-ink shipping label
- pay per data xfer job
- use cases: large data cloud migrations, DC decommission, disaster recovery
- if it takes more than a week to xfer over the network, use Snowball devices
- snowball process
  - request device from console
  - install client on your servers
  - connect the snowball to your servers and copy files to client
  - ship back device when you're done(goes to AWS facility)
  - data will be loaded into an S3 bucket
  - Snowball is completely wipes
  - tacking is done using SNS, txt messages
- Snowball Edge:
  - adds computational capability
  - 100TB capacity - storage optimized or compute optimized
  - supports a custom EC2 AMI so you can perform processing on the go
  - supports custom Lambda functions
  - very useful to pre-process the data while moving
  - use cases: data migration, image collation, IoT capture, machine learning
- AWS Snowmobile
  - xfer exabytes of data
  - each snowmobile has 100 pb of capacity
- cannot import into Glacier directly
  - must use S3 first, then S3 lifecycle policy into Glacier

#

## 148. Snowball Hands On

- snowball console
- create job
- plan your job
- shipping details
- shipping speed
- job details
- choose device
- select bucket name to write data to
- can load AMI onto Snowball
- security
- encryption
- notification options
- job status
- create job

# 149. Storage Gateway Overview

- Hybrid Cloud for storage
  - part of your infrastructure is on the cloud
  - part is onprem
- how do you expose the S3 data onprem?
  - AWS Storage Gateway!
- AWS Storage Cloud Native Options
  - Block Storage - EBS, Instance Store
  - File - EFS
  - Object - S3, Glacier
- Storage Gateway
  - is a bridge between onprem data and cloud data in S3
  - use cases: DR, backup, restore, tiered storage
- 3 types of storage gateway:
  1. File Gateway
  2. Volume Gateway
  3. Tape Gateway
- know diff between all 3
- File Gateway
  - configured S3 buckets are accessible using the NFS and SMB protocol
  - supports S3 Standard, IA, One Zone IA
  - bucket access using IAM roles for each File Gateway
  - most recently used data is cached in the file gateway
  - can be mounted on many servers
  - see diagram on slide
- Volume Gateway
  - Block storage using iSCSI protocol backed by S3
  - backed by EBS snapshots which can help restore onprem volumes
  - cached volumes: low latency access to most recent data
  - stored volumes: entire dataset is onprem, scheduled backups to S3
  - see diagram
- Tape Gateway
  - Virtual Tape Library backed by S3 and Glacier
  - backup data using existing tape based processes(and iSCSI interface)
  - works with leading backup software vendors
- Storage Gateway Summary
  - on prem data to cloud general -> Storage Gateway
  - file access/NFS -> file Gateway
  - volume/block/iSCSI -> Vol Gateway
  - VTL Tape -> Tape Gateway

#

# 150. Storage Gateway - File Gateway Hardware Appliance

- can install a FGHA onprem
- go on amazon.com and buy it
- helpful for daily NFS backups in small data center
- where you cant do virtualization backups

#

## 151. Storage Gateway - Hands On

- storage gateway console
- create gateway
- choose type
- choose options
- select host platform
- activate
- configure local disks

#

## 152. Amazon FSx - Overview

- EFS is POSIX for Linux
- cant use with Windows
- FSx for Windows is a fully managed Windows file system share drive
- supports SMB protocol and Windows NTFS
- supports Active Directory integration, ACLs, user quotas
- built on SSD, scales large
- can be accessed from your onprem infrastructure
- can be configured to be multiAZ
- data is backed up daily to S3
- Amazon FSx for Lustre
  - Lustre is a type of parallel distributed file system for large scale computing
  - stands for "Linux" plus "Cluster"
  - machine learning, High Performance Computing(HPC)
  - video processing, financial modeling, automation design
  - scales large
  - seamless integration with S3
    - can read S3 as file system
    - can write output of computations back to S3
  - can be used from onprem servers

#

## 153. Amazon FSx - Hands On

- FSx console
- create file system
- options - FS or Lustre
- name
- deployment type
- storage capacity
- throughput capacity
- network and security
- VPC, sec group, subnet
- Windows auth
- encryption
- maintenance options

#

## 154. All AWS Storage Options Compared

- storage comparison
- S3
  - object storage
- Glacier
  - object archival
- EFS
  - network file system for Linux, POSIX file system
- FSx
  - network file system for Windows
- FSx for Lustre
  - high perform computing Linux file system
- EBS Volumes
  - network storage for 1 EC2 instance at a time
- Instance Storage
  - physical storage for your EC2 instance
- Storage Gateway
  - file,vol,tape for onprem hybrid
- Snowball/Snowmobile
  - move large amounts of data to cloud physically
- database
  - for specific workloads needing indexing, querying

#

## Quiz 12: AWS Storage Extras - Quiz

-

#
