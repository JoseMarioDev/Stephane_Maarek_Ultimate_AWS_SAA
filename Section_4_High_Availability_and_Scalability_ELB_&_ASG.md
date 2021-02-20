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

# 50. Elastic Load Balancer - SSL Certificates

- SSL/TLS - Basics
  - SSL certificate allows traffic between your client and your LB to be encrypted in transit aka in-flight encryption
  - SSL refers to Secure Socket Layer, used to encrypt connections
  - TLS refers to Transport Layer Security, which is a newer version
  - nowadays, TLS certs are mainly used, but people still refer to as SSL
  - public SSL certs are issued by Certificate Authorities(CA)
    - ex: Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt, etc
  - SSL certs have an expiration date you set and must be renewed
- architecture example:
  - users connect to LB over HTTPS, LB talks to instances via HTTP over a private VPC
- load balancer uses an X.509 certificate - that is a SSL/TLS server certificate
- you can manage certificates using ACM (AWS Certificate Manager)
- you can create/upload your own certificates alternatively
- HTTPS listeners:
  - you must specify a default certificate
  - you can add an optional list of certs to support multiple domains
  - clients can use SNI(server name indication) to specify the hostname they reach
- SNI - sever name indication
  - solves problem of loading multiple SSL certs onto a web server(to serve multiple websites)
  - newer protocol, requires the client to indicate the hostname of the target server in the initial SSL handshake
  - the server will then find the correct certificate, or return the default one
  - only works for ALB and NLB and CloudFront
  - does not work for older CLB
- supports:
  - CLB
    - supports only one SSL cert
    - must use multiple CLBs for multiple hostnames with multiple SSL certs
  - ALB
    - supports multiple listeners with multiple SSL certs
    - uses SNI - server name indication to make it work
  - NLB
    - supports multiple listeners with multiple SSL certs
    - uses SNI to make it work
- Hands on:
  - you configure this on the listener when you add a new listener

#

## 51. Elastic Load Balancer - Connection Draining

- naming:
  - in CLBs known as Connection draining
  - in Target Groups(ALB and NLB) known as Deregistration Delay
- it the time it takes to complete "in-flight requests" while the instance is de-registering or unhealthy
- stops sending requests to the instance which is de-registering
- between 1 - 3600 seconds, default is 300 seconds
- can be disabled, set value to 0
  set to a low value if your requests are short

#

## 52. Auto Scaling Groups (ASG) Overview

- What is an ASG?
- a way to create and get rid of servers quickly to manage the changing load of your website/app
- Goals of an ASG:
  - scale out(add EC2 instances) to match an increase workload
  - scale in (remove EC2 instances) to match a decrease workload
  - ensure we have a min/max number of instances running
  - automatically register new instances to a LB
- parameters:
  - minimum size - at least how many instances I want running
  - desired capacity - how many I want running/are currently running
  - max capacity - how many I want running at the most
- ASG in AWS w/Load Balancer
- the load balancer can automatically connect to these instances, direct traffic, and perform health checks
- ASG and ELB work hand-in-hand
- ASGs have the following attributes:
  - a launch configuration
    - AMI + instance type
    - EC2 User Data
    - EBS Volumes
    - Security Groups
    - SSH Key Pair
  - min size/max size/initial capacity/desired capacity
  - network + subnet information
  - load balancer information
  - scaling policies - what triggers a scale out/scale in
- Auto scaling alarms
  - possible to scale an ASG based on CloudWatch alarms
  - an alarm monitors a metric(such as Average CPU usage)
  - metrics are computed for the overall ASG instances
  - Based on an Alarm we can:
    - create scale out policies - increase num of instances
    - create scale in policies - decrease num of instances
- ASG new rules:
  - now possible to define "better" auto scaling rules that are directly managed by EC2
    - target avg CPU usage
    - num of requests on the ELB per instance
    - avg network in
    - avg network out
  - these rules are easier to setup and can make more sense
- auto scaling custom metric
  - we can auto scale based on a custom metric(ex: num of connected users)
  - send custom metric from app on EC2 to CloudWatch
  - create CloudWatch alarm to react to low/high values
  - use CloudWatch alarm as the auto scaling policy for ASG
- ASG Brain Dump:
  - scaling policies can be on CPU, network, or even custom metrics or based on a schedule if you know your users patterns
  - ASG uses Launch Configurations or Launch Templates(newer - recommended)
  - to update an ASG, you must provide a new Launch Configuration/Launch Template
  - IAM roles attached to an ASG will get assigned to EC2 instances
  - ASGs are free. You pay for the underlying resources being launched
  - having instances under an ASG means that if they get terminated for whatever reason, the ASG will automatically create new ones as a replacement
    ASGs can terminate an instance marked as unhealthy by an ELB(and hence replace them)

#

## 53. Auto Scaling Groups Hands On

- choose a launch template or configuration
- templates are newer
- templates describe how to create EC2 instances
- select the instance type you want ex t2.micro
- select key/pair
- select VPC
- select SG
- select storage
- at the bottom, User Data
- click create
- purchase options - ondemand or spot fleet
- select subnets
- specify load balancing/health checks
- choose target group to add instances to
- health checks
  - 2 types EC2 and ELB
- group size and scaling policies
  - min/max/desired
  - scaling policies

#

## 54. Auto Scaling Groups - Scaling Policies

- diff types of scaling policies:
  1. target tracking scaling
  - most simple and easy to setup
  - ex: I want the avg CPU usage to stay around 40%
  2. Simple/step scaling
  - when a CloudWatch alarm is triggered(ex: CPU usage > 70%) add 2 instances
  - when a CloudWatch alarm is triggered(ex: CPU usage < 30%) remove 1 instance
  3. Scheduled Actions
  - anticipate based on known usage patterns
  - ex: increase the min capacity to 10 at 5pm on Fridays
- scaling cooldowns
  - cooldown period helps to ensure ASG doesn't launch/terminate instances before the prev scaling activity kicks in
  - in addition to the default cooldown, we can create cooldowns that apply to a specific simple scaling policy
  - a scaling specific cooldown overrides the default cooldown period
  - common use case: scale-in policy that terminates instances based on a specific criteria. yo can use a lower cooldown period to reduce costs
  - if your app is scaling up/down multiple times each hour, modify the ASG cool down timers and the CloudWatch Alarm Period that triggers the scale in

#

## 55. Auto Scaling Groups - for Solutions Architects

- things to know:
  - ASG Default Termination Policy(simplified)
    - find the AZ which as the most num of instances
    - if there are multiple instances in the AZ to choose from, delete the one with the oldest launch configuration
    - ASG tries to balance the num of instances across AZ by default
  - Lifecycle Hooks
    - by default as soon as an instance is launched it is available
    - you have ability to perform extra steps while it's in pending/terminating state
    - useful for logs
  - launch templates vs launch configuation
    - both:
      - define your instances
    - configuration(legacy)
      - must be recreated every time
    - templates(newer)
      - can have multiple versions
      - create parameter subsets(partial configuration for reuse and inheritance)
      - provision using both ondemand and spot instances(or mix)
      - can use T2 unlimited burst feature
      - newer, recommended by AWS going forward

#

## 56. Quiz 3: Fundamentals 2 Quiz

#
