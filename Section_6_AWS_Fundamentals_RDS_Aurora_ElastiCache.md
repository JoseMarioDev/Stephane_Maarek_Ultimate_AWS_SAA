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

- AWS propriety
- compatible with mySQL and PG
- "cloud optimized" claims 5x performance improvement over MySQL and 3x improvement over PG
- storage automatically grows in increments of 10gb up to 64tb
- Aurora can have 15 replicas while mysql has 5. replication process is faster
- failover in Aurora is instantaneous. HA native
- cost more then RDS - 20% more, but is efficient
- Aurora HA and read scaling
  - 6 copies of your data across 3 AZs
  - 4 copies out of 6 needed for writes
  - 3 copies out of 6 needed for reads
  - self healing with peer-to-peer replication
  - storage is striped across 100s of volumes
- one Aurora instance takes writes(master)
- automated failover for master in less than 30 seconds
- can have master plus up to 15 read replicas to serve reads
- support for cross region replication
- Aurora DB Clusters:
  - there is a writer endpoint(dns name) that points to the master for read/writes
  - there is a reader endpoint(dns name) that handles connection load balancing for all the reader replicas
  - reader replicas can auto scale up to 15 read replicas
  - shared volume that auto expands 10gb to 64tb
  - features:
    - automatic failover
    - backup and recovery
    - isolation and security
    - industry compliance
    - push button scaling
    - automated patching with zero downtime
    - advanced monitoring
    - routine maintenance
    - backtrack: restore data at any point of time w/o using backup
  - security:
    - same as RDS because they use the same engine
    - encryption at rest using KMS
    - automated backups, snapshots, and replicas are also encrypted
    - encryption in-flight using SSL(same process as MySQL or Postgres)
    - possibility to authenticate using IAM token(same as RDS)
    - you are responsible for protecting the instance with security groups
    - you can't SSH
  - Aurora Serverless
    - automated database instantiation and autoscaling based on actual usage
    - good for infrequent, intermittent, or unpredictable workloads
    - no capacity planning needed
    - pay per second, can be more cost effective
    - managed by a proxy fleet(managed by Aurora)
  - Global Aurora - 2 methods
    - Aurora cross region read replicas
      - useful for DR
      - simple to implement - just put read-replica in another region
    - Aurora Global Database(recommended)
    - 1 primary region where read/writes happen
    - 5 secondary read only regions for replication. lag is less than 1 sec
    - up to 16 read replicas per secondary region
    - help for decreasing latency
    - promoting of another region(for DR) has a RTO of < 1 minute

#

## 73. Aurora Hands On

- RDS console
- standard create
- choose Aurora with either MySQL or PG compatibility
- choose version
- location - regional/global
- database features - 4 modes(config writers/readers)
- template - prod/test
- settings
  - identifier
  - credential settings
  - db instance size
  - availability and durability
    - multi-AZ deployment
  - connectivity
    - vpc
    - subnet
    - sec group
  - encryption
  - backtrack
  - deletion protection

#

## 74. ElastiCache Overview

- used to get managed cache - Redis or MemCached
- caches are in-memory databases with really high performance, low latency
- helps reduce load off of db for read intensive workloads
- helps make your application stateless
- write scaling using sharding
- read scaling using read replicas
- multiAZ with failover capacity
- AWS takes care of OS maintenance/patching, optimizations, setup, configuration, monitoring, failure recovery and backups
- architecture:
  - app queries ElastiCache, if not there(miss) gets from RDS and stores in ElastiCache
  - helps relieve load in RDS
  - cache must have an invalidation strategy to make sure only the most current data is there
- common use case:
  - user sessions
  - user logs into any instances of the app
  - app writes session data to ElastiCache
  - user hits a diff instance
  - instance retrieves session data from ElastiCache and the user is already logged in
- Redis vs MemcacheD
- Redis
  - multi AZ with auto failover
  - read replicas to scale reads and have HA
  - data durability using AOF persistence
  - backup and restore features
- memcacheD
  - multinode for partitioning data(sharding)
  - non persistent
  - no backup and restore
  - multi-threaded architecture

#

## 75. ElastiCache Hands On

- ElastiCache console
- choose engine redis vs memcached
- name
- desc
- port
- node type
- num of replicas
- multi-AZ setting
- subnet group
- sec group
- encryption
- backups
- maintenance

#

## 76. ElastiCache for Solution Architect

- cache security
  - support SSL in flight
  - do not support IAM auth
  - IAM policies are only used for AWS api-level security
- Redis AUTH
  - you can set a password/token when you create Redis cluster
  - extra level of security for your cache on top of sec groups
- memcacheD
  - supports SASL based auth - advanced
- patterns:
  - lazy loading - all the read data is cached, can become stale
  - write through - adds or update data in the cache when written to a dB(no stale data)
  - session store - store temporary session data in a cache - using TTL features

#

## Quiz 5: RDS/Aurora/ElastiCache Quiz

-

#

## 77. List of Ports to be familiar with

- List of Ports to be familiar with
  Here's a list of standard ports you should see at least once. You shouldn't remember them (the exam will not test you on that), but you should be able to differentiate between an Important (HTTPS - port 443) and a database port (PostgreSQL - port 5432)

Important ports:

FTP: 21

SSH: 22

SFTP: 22 (same as SSH)

HTTP: 80

HTTPS: 443

vs RDS Databases ports:

PostgreSQL: 5432

MySQL: 3306

Oracle RDS: 1521

MSSQL Server: 1433

MariaDB: 3306 (same as MySQL)

Aurora: 5432 (if PostgreSQL compatible) or 3306 (if MySQL compatible)

Don't stress out on remember those, just read that list once today and once before going into the exam and you should be all set :)

Remember, you should just be able to differentiate an "Important Port" vs an "RDS database Port".

#
