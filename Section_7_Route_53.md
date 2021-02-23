## 78. Route 53 Overview

- R53 is a mananged DNS - domain name system
- DNS is a collection of rules/records which help clients understand how to reach a server through it's domain name
- in AWS 4 common records:
  - A: hostname to IPv4
  - AAAA: hostname to IPv6
  - CNAME: hostname to hostname
  - Alias: hostname to AWS resource
- diagram for how DNS works:
  - browser makes a request to R53 for the hostname of website
  - R53 sends back the ip addy of the site
  - browser sends a request to the website
  - the website returns the site
- R53 can use:
  - public domain names you own(buy)
  - private domain names that can resolved by your instances in your VPC
- R53 has advanced features:
  - load balancing - through DNS also called client load balancing
  - Health Checks (although limited)
  - Routing policy: simple, failover, geolocation, latency, weighted, multivalue
- you pay $0.50 per month per hosted zone(50 cents)

#

## 79. Route 53 Hands On

- go to R53 console
- global service, no region selection required
- go to registered domains and choose your domain
- add to cart, purchase
- go to hosted zones
- click on your domain and create your record
- command dig xxxx.com will give you more info

#

## 80. Route 53 - EC2 Setup

- creating a bunch of EC2 instances in diff regions
- create a load balancer - ALB
- add it the 3 AZs

## 81. Route 53 - TTL

- a way for browsers and clients to cache a response from a DNS query
- browser requests an ip from R53. R53 responds with the A record and a TTL
- mandatory for each DNS record

#

## 82. Route 53 CNAME vs Alias

- AWS resources(load balancer, CloudFront...) exose an AWS hostname:
  - ex: lb1-1234-useast2elb.amazon.aws.com and you want myapp.mydomain.com
- CNAME:
  - points a hostname to any other hostname (app.mydomain.com -> blbah.anything.com)
  - only for non root domains(ex something.mydomain.com)
- ALIAS:
  - points a hostname to an AWS resource (app.mydomain.com -> blah.amazonaws.com)
  - works for root domain and non root domain
  - free of charge
  - native health checks

#

## 83. Routing Policy - Simple

- use when you need to redirect to a single resource
- you cant attach health checks to simple routing policy
- if multiple values are returned, a random one is chose by the client
- you do this by adding multiple IP addresses when setting the A record

#

## 84. Routing Policy - Weighted

- control the % of the requests that go to a specific endpoint
  - ex: three instances - one has 70%, one has 20%, one has 10%
- helpful to test certain percentage of traffic on new app version for example
- helpful to split traffic between 2 regions
- can be associated with health checks

#

## 85. Routing Policy - Latency

- redirected to the server that has the least latency close to the us
- super helpful when latency of users is a priority
- latency is evaluated in terms of user to designated AWS region
- Germany may be directed to the US (if thats the lowest latency)

#

## 86. Route 53 Health Checks

- unhealthy if fails X amount of health checks - default is 3
- default health check interval is 30s. can set to 10s for a higher cost
- about 15 health checkers will check the endpoint health
- default is one req every 2 seconds on average(assume 30s default)
- can have HTTP, TCP, HTTPS, health checks(no SSL verifcation)
- possibility of integrating the health check with CloudWatch
- health checks can be linked to Route53 DNS queries

#

## 87. Routing Policy - Failover

- ex: 2 instances:
  - 1 primary
  - 1 secondary
- R53 sends DNS requests from browser to primary until it fails health checks
  - if it fails, sends requests to the secondary
- set primary or secondary in the record set routing policy in R53
- must associate with health check

#

## 88. Routing Policy - Geolocation

- routing based on user location
- create a default policy in case there is no match
- routing based on user's location, diff from latency
- choose continent/country when creating your record set

#

## 89. Routing Policy - Multi-Value

- route traffic to multiple resources
- want to associate R53 health checks w/records
- up to 8 healthy records are returned for each multi-value query
- multi-value is not a substitute for having an ELB

#

## 90. 3rd Party Domains & Route 53

- R53 as a Registrar
  - a domain name registrar is an org that manages the reservation of internet domain names
  - ex: godaddy, google domains, etc
- domain registration is diff from DNS
- 3rd party registrar with AWS Route 53
  - if you buy your domain from another website
  - create a hosted zone in R53
  - update NS on 3rd party site to use R53 name servers
  - domain registrar is not DNS, but each domain registrar usually comes with some DNS features

#

## 91. Section Cleanup

- deleting all the records from the hands on

#

## Quiz 6: Route 53 quiz

-

#
