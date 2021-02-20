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

- supports:
  - TCP - layer 4
  - HTTP, HTTPS
- health checks are TCP or HTTP based
- fixed hostname
  - XXXX.region.elb.amazonaws.com

#

## 46. Application Load Balancer (ALB) with Hands On

- ALB is layer 7 (HTTP)
- load balancing to multiple HTTP applications across multiple machines(target groups)
- load balancing to multiple applications on the same machine (ex: containers)
- support for HTTP/2 and WebSockets
- supports redirects(HTTP to HTTPS)
- routing features:
  - supports routing tables to diff target groups
  - routing based on path in url -> example.com/users or example.com/posts
  - routing based on hostname -> one.example.com or other.example.com
  - routing based on query string, headers -> example.com/users?id=123&order=false
- ALBs are a great fit for microservices and container based applications(ex: Docker and AWS ECS)
- has a port mapping feature to redirect to a dynamic port in ECS
- in comparison, we'd need multiple CLBs per application
- Target Groups can be composed of:
  - EC2 instances(can be managed by an ASG - HTTP)
  - ECS tasks(managed by ECS itself-HTTP)
  - Lambda functions-HTTP request is translated into a JSON event
  - IP addresses - must be private IPs
- ALB can route to multiple target groups
- Health Checks are at the target group level
- Good to know:
  - fixed hostname (xxx.region.elb.amazonaws.com)
  - the application servers dont see the IP of the client directly. it sees the IP of the load balancer
  - the true IP of the client is inserted in the header X-Forward-For
  - we can also get the port of the client in the header X-Forward-Port and protocol in X-Forward-Proto

#

## 47. Network Load Balancer (NLB) with Hands On

- NLBs communicate at Layer 4
- properties:
  - forward TCP and UDP traffic to your instance
  - handle millions of requests per second
  - less latency(~100ms vs 400ms for ALB)
- NLBs have one stic IP address per AZ
- support assigning Elastic IPs - helpful for whitelisting specific IPs
- NLBs are used for extreme performance, TCP or UDP traffic
- not included in the free tier
- Hands On:
  - similar to setting up an ALB, but:
  - no SG attached
  - you have to create a rule in the SG to allow traffic

#

## 48. Elastic Load Balancer - Stickiness

- implement stickiness so the same client is always redirected to the same instance behind a load balancer
- works for CLB and ALB
- uses a cookie that has an expiration date you control
- use case: make sure user doesnt lose his session data
- enabling stickiness may bring imbalance to the load over the backend EC2 instances
- Hands On:

  - change stickiness at Target Group level

  #

## 49. Elastic Load Balancer - Cross Zone Load Balancing

- with cross zone load balancing:
  - each LB instance distributes evenly across all registered instances in all AZs
- without:
  - each LB instance distributes requests evenly across the registered instances in its AZ only
- good ex in lecture - see slide
- Billing and defaults:
  - CLB
    - disabled by default
    - no charge for inter AZ data is enabled
  - ALB
    - always on - cant disable
    - no charges for inter AZ data
  - NLB
    - disabled by default
    - you pay charges for inter AZ data if enabled
-

#

# 50. Elastic Load Balancer - SSL Certifcates

-