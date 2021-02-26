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

#
