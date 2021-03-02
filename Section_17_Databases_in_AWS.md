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

- 

#

## 194. Aurora

-

#

## 195. ElastiCache

-

#

## 196. DynamoDB

-

#

## 197. S3

-

#

## 198. Athena

-

#

## 199. Redshift

-

#

## 200. Neptune

-

#

## 201. ElasticSearch

-

#

## Quiz 16: Databases Quiz

-

#
