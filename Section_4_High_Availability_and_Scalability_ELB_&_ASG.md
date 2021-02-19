## 42. High Availability and Scalability

- scalability means that an app/system can handle greater loads by adapting
- 2 kinds of scalability:
  1. vertical
  2. horizontal. known as elasticity
- scalability is linked but diff than HA
- vertical scalability:
  - means increasing size of an instance
  - ex: running app on t2.micro, upgrading to t2.large
  - common for nondisttributed systems such as a database
  - RDS, ElastiCache are services that scale vertically
  - usually a limit to how much you can vertically scale(hardware limit)
- horizontal scalability:
- means increasing the number of instances/systems in your application
- implies distributed systems
- common for web applications/modern applications
- easy to horizontal scale thanks to services like EC2
- High Availability
- usually goes hand in hand w/horizontal scaling
- HA means running your app/system in at least 2 data centers(AZ)
- goal of HA is to survive a data center(AZ) loss
- HA can be passive -> ex: RDS multi AZ
- can also be active -> ex: horizontal scaling
- summary:
  - vertical scaling aka scale up/down
  - horizontal scaling increase number of instances(scale out/in)
    - ASG
    - load balancer
  - HA -> runs instances for the same app across multiple AZs
    - ASG multi AZ
    - LB multi AZ

#
