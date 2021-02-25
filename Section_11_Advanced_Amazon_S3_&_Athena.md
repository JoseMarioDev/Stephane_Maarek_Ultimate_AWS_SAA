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

- 

#

## 132. S3 Storage Classes + Glacier - Hands On

-

#

## 133. S3 Lifecycle Rules

-

#

## 134. S3 Lifecycle Rules - Hands On

-

#

## 135. S3 Performance

-

#

## 136. S3 Select & Glacier Select

-

#

## 137. S3 Event Notifications

-

#

## 138. S3 Event Notifications - Hands On

-

#

## 139. Athena Overview

-

#

## 140. Athena Hands On

-

#

## 141. S3 Lock Policies & Glacier Vault Lock

-

#

## Quiz 10: S3 Advanced & Athena - Quiz

-

#
