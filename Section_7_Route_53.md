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
