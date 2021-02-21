## 68. AWS RDS overview

- RDS - relational database service
- managed db service for DB using SQL as a query language
- allows you to create dbs in the cloud that are managed by AWS
  - Postgres
  - MySQL
  - MariaDB
  - Oracle
  - Microsoft SQL Server
  - Aurora - AWS proprietary database
- advantages over using RDS versus deploying db on EC2 instance
  - RDS is a managed service:
    - automated provisioning, OS patching
    - continuous backups and restore to specific timestamp (point in time restore)
    - monitoring dashboards
    - read replicas for improved read performance
    - multi AZ setup for DR(disaster recovery)
    - maintenance windows for upgrades
    - scaling capability - vertical and horizontal
    - storage backed by EBS(gp2 or io1)
  - but you cannot SSH into your instance
- RDS backups
  - automated backups:
    - daily full bkup of the db(during maintenance window)
    - transaction logs are backed-up by RDS every 5 minutes
    - ability to restore to any point in time(from oldest to 5 minutes ago)
    - 7 day retention(can be increased to 35 days)
  - DB snapshots:
    - manually triggered by the user
    - retention of backup for as long as you want

#

## 69. RDS Read Replicas vs Multi-AZ

- Read Replicas for read scalability
  - up to 5 read replicas
  - replicas can be within same AZ, cross AZ, or cross region
  - replication is async, so reads are eventually consistent
  - replicas can be promoted to their own db
  - applications must update the connection string to leverage read replicas
- use cases:
  - ex of a prod db that reads/writes. another dept wants to run analytics on the data, you set up a read replica they hit to gather data. only read, no write
- read replicas - network cost:
  - in AWS there is a cost when data is replicated from one AZ to another
  - to reduce cost, you can have your read replicas in the same AZ
- RDS multi-AZ disaster recovery
  - SYNC replication
  - one DNS name - automatic app failover to standby
  - increase availability
  - failover in case of loss of AZ, loss of network, loss of instance or storage
  - no manual intervention in apps
  - not used for scaling
- Read replicas can be setup as multi-AZ for disaster recovery

#

## 70. AWS RDS Hands On

- go to RDS console
- create db
- engine options
- Aurora is not free tier eligible, using MySQL for lecture
- templates
- db instance identifier - region unique
- username/password
- db instance size
- storage
- storage autoscaling
- availability and durability
  - multi-AZ
- connectivity
  - select VPC
  - publicly accessible?
- VPC sec group
- AZ
- database port
- backups - enable
- backup window
- monitoring
- maintenance
- deletion protection
- using sqlelectron to connect to db

#

## 71. RDS Encryption + Security

- encryption
- at rest encryption
  - possible to encrypt the master and read replicas with AWS KMS(AES-256) encryption
  - encryption has to be defined at launch time
  - if the master is not encrypted, the read replicas cannot be encrypted
  - transparent data encryption(TDE) available for Oracle and SQL Server
- in-flight encryption
  - SSL certs to encrypt data to RDS in-flight
  - provide SSL options with trust certificate when connecting to db
  - to enforce SSL:
    - Postgres is a parameter group
    - MySQL is a SQL command within the db
  - RDS encryption operations
  - encrypting RDS backups
    - snapshots of unencrypted RDS dbs are unencrypted
    - snapshots of encrypted RDS dbs are encrypted
    - can copy a snapshot into an encrypted one
  - to encrypt an unencrypted RDS db
    - create snapshot of unencrypted db
    - copy the snapshot and enable encryption on the new snapshot
    - restore the db from the new encrypted snapshot
    - migrate apps to the new db, delete the old db
  - RDS security - network and IAM
  - network security
    - RDS dbs are usually deployed within a private subnet, not a public one
    - RDS security works by leveraging security groups(same concept as EC2 instances)
    - it controls which IP/sec groups can communicate with the RDS
  - Access mgmt
    - IAM policies help control who can manage AWS RDS(through the RDS API)
    - traditional username/password can be used to log into the database
    - IAM-based auth can be used to log into a RDS MySQL and PG database
  - RDS - IAM Authentication
    - IAM db auth works with MySQL and PG
    - you dont need a password, just an auth token obtained through IAM & RDS API calls
    - auth token has lifetime of 15 mins
    - benefits:
      - network in/out must be encrypted using SSL
      - IAM to centrally manage users instead of db
      - can leverage IAM roles and EC2 instance profiles for easy integration
  - RDS security - summary
    - encryption at rest
      - is done only when you first create the db instance
      - or unencrypted db->snapshot->copy snap and encrpyt-> create new db from encrypted snapshot
    - your responsibility
      - check the ports/IP/security group inbound rules in Db's security group
      - in database user creation and permissions or manage through IAM
      - creating a db with or without public access
      - ensure parameter groups or DB is configured to only allow SSL connections
    - AWS responsibility
      - no ssh allowed
      - no manual db patching allowed
      - no manual OS patching allowed
      - no way to audit the underlying instance

#

## 72. Aurora Overview

-

#

## 73. Aurora Hands On

-

#

## 74. ElastiCache Overview

-

#

## 75. ElastiCache Hands On

-

#

## 76. ElastiCache for Solution Architect

-

#

## Quiz 5: RDS/Aurora/ElastiCache Quiz

-

#

## 77. List of Ports to be familiar with

-

#
