## 123. S3 MFA Delete

- MFA forces users to generate a code on a device before doing important operations on S3
- to use MFA-Delete, enable versioning on the S3 bucket
- you will need MFA to 
  - perm del an obj version
  - suspend versioning on the bucket
- you wont need MFA for:
  - enabling versioning
  - listing deleted versions
- only the bucket owner(root acct) can enable/disable MFA-Delete
- MFA-Delete currently can only be enabled using the CLI

#

## 124. S3 MFA Delete Hands On

- enabling mfa-delete through the CLI using a root account

#

## 125. S3 Access Logs

- for auditing purposes, you may want to log all access to S3 buckets
- any req made to S3, from any other acct, authorized or denied will be logged into another bucket
- that data can be analyzed using data analysis tools or Amazon Athena 
- S3 Access Logs: warning
  - do not set your logging bucket to be the monitored bucket.  dont use same bucket for both
  - it will create a logging loop, and your bucket will grow in size exponentially

#

## 126. S3 Access Logs - Hands On

- create a bucket for monitoring and a separate one for logging
- go to properties -> server access logging from the bucket you want monitored
- give it the target of the bucket used for logging
- it modifies the ACL to give access

#

## 127. S3 Replication(Cross Region and Same Region)

- must enable versioning in source and destination buckets
- buckets can be in diff accts
- copying is asynch
- must give proper IAM permissions to S3 via IAM Role
- CRR use cases: compliance, lower latency access, replication across accts
- SRR use cases: log aggregation, live replica between prod and test accts
- S3 Replication Notes:
  - after activating, only new objects are replicated - (not retroactive)
  - for delete operations, deleted objects are not replicated
  - there is no chaining of replication
  - if buck 1 -> buck 2 -> buck 3, bucket 3 does not have buck 1 objects


#

## 128. S3 Replication - Hands On

- create 2 buckets in diff regions
- enable versioning in each bucket
- management tab -> replication rules
- can limit scope to what objects you want replicated
- define source/destination buckets
- choose IAM role

#

## 129. S3 Pre-signed URLs

- can generate presigned URLs using SDK or CLI
  - for downloads you can use CLI - easier
  - for uploads you must use SDK - harder
- valid for 3600 seconds - 1 hour by default
  - can change timeout with --expires-in argument
- ex: only want logged-in users to download a premium video on your S3 bucket
- ex: everchanging list of users to download files by generating URLs dynamically
- 

#

## 130. S3 Pre-signed URLs - Hands On

- to generate a presigned url from the cli
  - run aws s3 presign s3://bucket/object --region

#

## 131. S3 Storage Clases + Glacier

- storage classes
  - S3 standard
  - S3 Infrequent Access
  - S3 One Zone Infrequent Access
  - S3 Intelligent Tiering
  - Glacier
  - Glacier Deep Archive

- 1. S3 Standard - general purpose
  - high durability of objects across multiple AZs - 11 nines.  99.999999999%
  - 99.99% availability over a given year
  - sustain 2 concurrent facillity failure
  - use cases: big data analytics, mobile and gaming apps, content distribution
- 2. S3 Standard - infrequent Access
  - suitable for data that is less frequently accessed, but requires rapid access when needed
  - high durability - 11 nines
  - 99.9% availability
  - lower cost compared to S3 Standard
  - sustain 2 concurrent facility failures
  - use cases: data store for DR, backups
- 3. S3 One Zone - Infrequent Access
  - same as IA, but data is stored in a single AZ
  - high durability - 11 nines of objects in one AZ, but data is lost if AZ is destroyed
  - 99.5% availability
  - low latency, high throughput performance
  - supports SSL for data in transit and at rest
  - lower cost compared to IA - 20% cheaper
  - use cases: secondary backup copies of onprem data, or storing data you can recreate
- 4. S3 Intelligent Tiering
  - same latency and high throughput of S3 Standard
  - small monthly monitoring and auto-tiering fee
  - automatically move objects between 2 access tiers based on changing access patterns(S3 General and IA)
  - durability - 11 nines mulitple AZs
  - resilient against events that impact an entire AZ
  - designed for 99.9% availiability over a given year
- 5. Amazon Glacier
  - low cost object storage for archiving/backup
  - data is retained long term(decades)
  - alternative to onprem magnetic tape
  - durabilility - 11 nines
  - cost per storage per month $.004/gb + retrieval cost
  - each item in Glacier is called an Archive(not object) up to 40TB
  - archives are stored in vaults
  - 3 retrieval options(wait times)
    - expedited 1-5 minutes
    - standard 3-5 hours
    - bulk 5-12 hours
  - min storage duration is 90 days
- 6. Amazon Glacier Deep Archive
  - long term storage - even cheaper
  - wait times
    - standard 12 hours
    - bulk 48 hours
  - min storage duration is 180 days
- good chart on the slide

#

## 132. S3 Storage Classes + Glacier - Hands On

- create bucket
- when uploading an object,
  - upload options
  - storage class options
  - must restore any objects in Glacier
  - can edit storage class and change it

#

## 133. S3 Lifecycle Rules

- you can transition objects between storage clasess
- for infrequently accesed objects, move them to Standard IA
- for objects you dont need in real time, move them to Glacier or Glacier Deep Archive
- moving objects can be automated using a lifecycle configuration
- S3 Lifecycle Rules
  - transition actions: defines when objects are moved to another storage class
    - ex: move object to standard IA 60 days after creation
    - ex: move to glacier for archiving after 6 months
  - expiration actions: config objects to expire (delete) after a certain time
    - ex: access logs delete after 365 days
    - can be used to delete old versions of files(if versioning is enabled)
    - can be used to delete incomplete multipart uploads
  - rules can be created for a certain prefix(folder)
  - rules can be created for a certain tag(ex:finance dept)


#

## 134. S3 Lifecycle Rules - Hands On

- management tab -> lifecycle rules
- create rule
- adjust settings based on your need
- 5 lifecycle rule actions to choose from

#

## 135. S3 Performance

- by default S3 scales to high requests and has low latency, 100-200ms
- no limit to the number of prefixes(folders) in a bucket
- your app can get at least 3500 put/copy/post/delete and 5500 get/head requests per second per prefix in a bucket
- S3 KMS limitation
  - if you use SSE-KMS you may be impacted by KMS limits
  - when you upload it calls the generatedatakey KMS api
  - download calls the decryptkmskey api
  - counts towards the KMS quota per second
  - as of today you cannot request a quota increase for KMS
- S3 performance
  - use multi-part upload
    - recommended for files > 100mb
    - must use for files > 5gb
    - can parallelize uploads (speeds up xfers)
  - S3 Transfer Acceleration
    - increase xfer speed by xferring file to AWS edge location which will forward the data to the S3 bucket in target region
    - compatible with multi-part upload
  - S3 Byte Range Fetches for GETs
    - parallelize GETs by requesting specific byte ranges
    - better resilience in case of failures
    - can be used to speed up downloads
    - can be used to retrieve only partial data(ex: head of a file)

#

## 136. S3 Select & Glacier Select

- retrieve less data using SQL by performing server side filtering
- can filter by rows/columns(simple SQL statements)
- less network xfer; less cpu cost client-side
- filtering of data server side to get less data back

#

## 137. S3 Event Notifications

- create event notification rules
- trigger some kind of event to SNS, SQS, or Lambda
- if 2 writes are made to a nonversioned object, you may get one notication
- to ensure all notifications enable versioning
#

## 138. S3 Event Notifications - Hands On

- create bucket
- properties -> event notifications
- create event notification
- general config
- event type
- choose destination - lambda function, SNS, SQS


#

## 139. Athena Overview

- serverless service to perform analytics directly against S3 files
-  normally you upload files from S3 to a db
- with Athena, you leave files in S3
- and query against them
- uses SQL
- charged per query and data scanned
- supports many files types - csv, json, orc, etc
- use cases: business intelligence, analytics, reporting, vpc flow logs, elb logs, cloudtrail, etc
- analyze data directly on S3 -> athena

#

## 140. Athena Hands On

- athena console
- setup query in Athena
- select bucket to run query on
- select location for query results
- create db to store queries
- create tables 
- run queries on data

#

## 141. S3 Lock Policies & Glacier Vault Lock

- S3 object lock
  - adopt WORM model
    - write once read many
    - block an object version deletion for a specified amt of time
- glacier vault lock
  - worm model
  - lock policy for future edits - can't be changed
  - helpful for compliance and data retention

#

## Quiz 10: S3 Advanced & Athena - Quiz

-

#
