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

## 43. Elastic Load Balancing(ELB) Overview

- what is a load balancer?
  - servers that forward internet traffic to multiple servers (EC2 instances) downstream
- why use a load balancer?
  - spread load across multiple downstream instances
  - expose a single point of access(DNS) to your application
  - seamlessly handle failures of downstream instances
  - do regular health checks on your instances
  - provide SSL termination(HTTPS) for your websites
  - enforce stickiness with cookies
  - high availability across zones
  - separate public traffic from private traffic
- why use an EC2 load balancer?
  - an ELB is a managed load balancer
  - AWS guarantees that it will be working
  - AWS takes care of upgrades, maintenance, HA
  - AWS provides only a few config options
  - it costs less to setup your own lb, but it will be a lot more effort on your end
  - it is integrated with many AWS services/offerings
- Health Checks
  - crucial for LBs
  - enable the LB to know if instances it forwards traffic to are available to reply to requests
  - health check is done on a port and route (/health is common)
  - if the response is 200(ok), then the instance is healthy
  - by default, check every 5 seconds
- types of LBs
  - all managed
  - Classic LB v1
    - v1 - old generation 2009
    - HTTP, HTTPS, TCP
  - Application Load Balancer
    - v2 - new generation 2016
    - HTTP, HTTPS, WebSockets
  - Network Load Balancers
    - V2 - new generation 2017
    - TCP, TLS(secure TCP), UDP
  - overall, it is recommended to use new generation LBs as they provide more features
  - you can setup internal or external ELBs
- LB sec groups
  - you can setup a SG to allow any source IP address access to port 80 and port 443 to access the LB
  - then, setup an SG on the EC2 instances to only allow traffic from the Load Balancer's security group. because the instance is only expecting traffic from the LB
- good to know:
  - LBs can scale but not instantaneously - contact AWS for a "warm up"
  - troubleshooting
    - 4xx errors are client induced errors
    - 5xx errors are application induced errors
    - load balancer errors 503 means at capacity or no registered target
    - if the LB can't connect to your application, check your security groups
  - Monitoring
    - ELB access logs will log all access requests (so you can debug per request)
    - CloudWatch Metrics will give you aggregate statistics ex: connection count

#

## 44. About the Gateway Load Balancer

- Nov 2020 AWS released a Gateway Load Balancer
- not believed to be on the exam
- [link for more info](https://aws.amazon.com/blogs/aws/introducing-aws-gateway-load-balancer-easy-deployment-scalability-and-high-availability-for-partner-appliances/)

#

## 45. Classic Load Balancer with Hands On
