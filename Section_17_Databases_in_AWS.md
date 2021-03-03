## 192. Choosing the Right Database

- questions to ask for choosing the right db based on your arch:
  - is your workload read heavy? write heavy? balanced workload
  - throughput needs? will it change? does it need to scale or fluctuate through the day?
  - how much data to store and for how long? will it grow? object size?
  - how are they accessed?
  - data durability? source of truth for your data?
  - latency requirements? num of concurrent users?
  - data model? how will you query the data? joins? structured? semi-structured?
  - strong schema? more flexibility? reporting? searching? RBDMS? NoSQL?
  - license cost? switch to cloud native db like Aurora?
- Database types
  - RDBMS(SQL/OLTP-online transaction processing): RDS, Aurora - great for joins. RDS, PG, MySQL, oracle, aurora
  - NoSQL database: DynamoDB(JSON), ElastiCache(key/value), Neptune(graph). No joins, no sql
  - object store(for big objects): S3, Glacier
  - Data warehouse(sql analytics): redshift(OLAP), Athena
  - search(json): free text, unstructured searches
  - graphs: neptune - displays relationship between data

#

## 193. RDS

- overview
  - managed db - PG, mysql, oracle, sql server
  - must provision an EC2 instance, volume type, size
  - support for read replicas and multiAZ
  - security through IAM, secgroups, KMS, SSL intransit
  - backups, snapshot, point in time recovery feature
  - managed and scheduled maintenance
  - monitoring through cloudwatch
  - use case: store relational datasets(RDBMS, OLTP), perform SQL queries, transactional inserts/update/delete is available
- SA perspective - compare to the 5 pillars
  - operations: small downtime when failover happens, when maintenance happens, scaling in read replicas/ec2 instances/restore EBS implies manual intervention, application changes
  - security: AWS responsible for OS security, we are responsible for setting up KMS, secgroups, IAM policies, authorizing users in DB, using SSL
  - reliability: multiAZ feature, failover in case of failures
  - performance: depends on EC2 instance type, EBS vol type, ability to add read replicas. Doesn't autoscale
  - cost: pay per hour based on provisioned EC2 and EBS

#

## 194. Aurora

- overview
  - compatible API for PG/mysql
  - data is held in 6 replicas, across 3 AZs
  - auto healing capability
  - multi AZ, auto scaling read replicas
  - read replicas can be global
  - aurora db can be global for DR or latency purposes
  - auto scaling of storage from 10gb to 64tb
  - define EC2 instance type for aurora instances
  - same security/monitoring/maintenance features as RDS
  - aurora serverless option
- use case: same as RDS, but with less maintenance/more flexibility/more performance
- SA perspective - compare to the 5 pillars
  - operations: less operations, auto scaling storage
  - security: AWS responsible for OS sec, we responsible for setting up KMS, secgroups, IAM plicies, authorizing users in DB, SSL
  - reliability: multi AZ, highly available, possibly more than RDS, aurora serverless option
  - performance: 5x performance according to AWS due to architectural optimizations. Up to 15 read replicas(only 5 for RDS)
  - cost: pay per hour, based on EC2 and storage usage.

#

## 195. ElastiCache

- managed Reds/memcached(similar offering as RDS, but for caches)
- inmemory data store, high performance
- must provision an EC2 instance type
- support for clustering(Redis) and multiAZ, read replicas(sharding)
- security through IAM, secgroups, KMS, Redis Auth
- backup/snapshot/point in time restore
- managed and scheduled maintenance
- monitoring through cloudwatch
- use cases: key/value store, frequent reads, less writes, cache results for db queries, store session data for websites, cannot use SQL
- SA perspective - compare to the 5 pillars
  - operations: same as RDS
  - security: aws responsible for os sec, we are responsible for KMS, secgroups, IAM policies,users(redis auth), SSL
  - reliability: clustering, multiAZ
  - performance: sub-millisecond performance, in-memory, read replicas for sharding, very popular cache option
  - cost: pay per hour based on EC2 and storage use

#

## 196. DynamoDB

- AWS proprietary, managed nosql db
- serverless, provisioned capacity, autoscaling, on demand capacity
- can replace ElastiCache as a key/value store(storing session data for example)
- highly available, multiAZ by default, reads/writes are decoupled, DAX for read cache
- reads can eventually consistent or strongly consistent
- security, authentication, authorization is done through IAM
- can use DDB Streams to integrate with AWS lambda
- backup/restore feature, global table feature
- monitoring through cloudwatch
- can only query on primary key, sort key, indexes
- use cases: need serverless, distributed serverless cache
- SA perspective - compare to the 5 pillars
  - operations: no operations needed, autoscaling capable, serverless
  - security: full sec through IAM policies, KMS encryption, SSL inflight
  - reliability: multiAZs, backups
  - performance: single digit millisecond performance, DAX for caching reads, performance doesnt degrade if your app scales
  - cost: pay per provisioned capacity and storage usage. can use auto scaling

#

## 197. S3

- is a key/value store for objects
- great for big objects, not so great for smaller objects
- serverless, scales infinitely, max object size is 5TB
- eventually consistency for overwrites, deletes
- tiers: standard, IA, one zone iA, glacier, etc
- features: versioning, encryption, crr, etc
- encryption: SSE-S3, SSE-KMS, SSE-C, client side, SSL in transit
- use cases: static files, key/value store for big files, website hosting
- SA perspective - compare to the 5 pillars
  - operations: no operations needed
  - security: IAM, bucket policies, ACL, encryption(server/client requirements), SSL
  - reliability: eleven nines durability, 99.99% availability, multiAZ, CRR
  - performance: scales to thousands reads/writes per second, transfer acceleration/multipart for big files
  - cost: pay per storage, network costs, requests number

#

## 198. Athena

- query engine on top of S3
- fully serverless query engine with SQL capabilities
- used to query data in S3
- pay per query
- output results back to S3
- secured through IAM
- use cases: one time sql queries, serverless queries on S3, log analytics
- SA perspective - compare to the 5 pillars
  - operations: no operations needed, serverless
  - security: IAM + S3 security
  - reliability: managed service, uses Presto engine, HA
  - performance: queries scale based on data size, scales accordingly
  - cost: pay per query, per TB of data scanned, serverless

#

## 199. Redshift

- based on PG, but not used for OLTP
- not a replacement for RDS
- its OLAP - online analytical processing(analytics and data warehousing)
- 10x better performance than other data warehouse technologies scales to PB of data
- data is stored in columns. Columnar(instead of rows)
- massively parallel query execution MPP, highly available
- pay as you go, based on instance provisoned
- has an SQL interface for performing queries
- compatible with BI tools such as AWS quicksight, or Tableau integration with it
- data is loaded from S3, DDB, DMS, or other dbs...
- from 1 to 128 nodes, up to 160gb of space per node
- leader node: for query planning, results aggregation
- compute nodes: for performing queries, sends results to leader
- redshift spectrum: perform queries directly against S3(no need to load data)
- backup and restore, security, VPC/IAM/KMS/monitoring
- redshift enhanced VPC routing: copy/unload goes through VPC - enhanced security
- snapshots: point in time backups of cluster, stored in S3. incremental
- you can restore a snapshot to a new cluster
- automated or manual
- can copy snapshots(auto or manual) to another region
- spectrum: query data in S3 without loading. must have redshift cluster
  - query is submitted to spectrum nodes. see slide
- SA perspective - compare to the 5 pillars
  - operations: similar to RDS
  - security: IAM,VPC,KMS,SSL (similar to RDS)
  - reliability: HA, auto healing features
  - performance: 10x performance vs other data warehouse, compression
  - cost: per node provisoed, 1/10th cost of other data warehouses
- remember: redshift = analytics, BI, data warehousing

#

## 200. Neptune

- fully managed graph database
- high relationship data
- ex: social networking
- knowledge graphs(wikipedia)
- HA across 3 AZs, up to 15 read replicas
- point in time recovery, continuos backup to S3
- support for KMS encryption at rest + HTTTPS
- SA perspective - compare to the 5 pillars
  - operations: similar to RDS
  - security: IAM,VPC,KMS,SSL(similar to RDS), + IAM authentication
  - reliability: multiAZ, clustering
  - performance: best suited for graphs, clustering to improve performance
  - cost: pay per node provisioned(similar to RDS)
- remember: Neptune = Graphs

#

## 201. ElasticSearch

- example: in DDB, you can only find data by primary key or indexes
- with elasticSearch, you can search any field, even partial matches
- common to use as a compliment to another db
- used for big data applications
- can provision a cluster of instances
- built in integration: kinesis data firehouse, IoT,Cloudwatch logs for data ingestion
- security through Cognito, IAM, KMS, SSL, VPC
- comes with Kibana(visualization tool) and Logstash(log ingestion) - ELK stack
- SA perspective - compare to the 5 pillars
  - operations: similar to RDS
  - security: cognito, IAM, VPC, KMS, SSL
  - reliability: multiAZ, clustering
  - performance:based on elasticsearch project(open source), petabyte scale
  - cost: pay per node provisioned(similar to RDS)
- remember: elasticsearch = search/indexing

#

## Quiz 16: Databases Quiz

-

#
