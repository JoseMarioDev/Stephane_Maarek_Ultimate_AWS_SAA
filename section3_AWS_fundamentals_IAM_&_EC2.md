## 13. IAM Introduction

- IAM is a global service
- three main types:
  - Users
  - Groups
  - Roles
- Users are put into Groups
- Groups are assigned permissions
- Roles are assigned to machines and given permissions
- permissions are JSON object
- should use the "least privilege principle" . give user the minimum amount of access to get their job done
- can use IAM Federation for big enterprises. Users can use their current Active Directory to log into AWS
- Brain Dump
  - one IAM user per person
  - one IAM role per application
  - never use the root account, create once and don't use again

#

## 14. IAM Hands-on

- creating user acct
- create admin group, apply admin policy to group
- add created user to group
- change alias to company name
- log in as new user
- voila!

#

## 15. changes to the EC2 console

- nothing here, doc showing new changes to look of EC2 console

#

## 16. EC2 introduction

- capable of:
  - renting virtual machines (EC2)
  - storing data on virtual drives (EBS)
  - Distributing load across machines (ELB)
  - Scaling the services using an auto-scaling group (ASG)

#

## 17. SSH Overview

- summary table, which OSs support SSH or putty.
- can also use instance connect

#

## 18. How to SSH using Linux or Mac

- what is SSH?
  - allows you to control a remote machine, using the command line
  - ssh into instance
  - chmod 0400 the .pem file
  - run ssh -i nameofkeyfile.pem ec2-user@ipaddress

#

## 19. How to SSH using Windows

- can use putty

#

## 20 How to SSH using Windows 10

- how to use powershell/or putty if you dont have SSH

#

## 21 SSH Troubleshooting

- list of common problems and how to fix

#

## 22 SSH EC2 Instance Connect

- browser based SSH connection
- needs to recent with AMI

#

## 23. Introduction to Security Groups

- fundamental part of network security in AWS
- control how traffic is allowed in/out of our EC2 machines
- fundamental skill to learn to troubleshoot networking issues
- allow, inbound, outbound ports
- edit security groups
  - must allow ports
  - implicit deny?

#

## 24. Security Groups Deep Dive

- SGs act as a "firewall" on EC2 instances
- they regulate:
  - access to ports
  - authorized IP ranges - v4 and v6
  - control of inbound network traffic( outside world -> instance)
  - control of outbound network traffic( instance -> outside world)
- good to know:
  - SGs can be attached to multiple instances
  - locked down to a region/VPC combo
  - live outside of the EC2 instance - if traffic is blocked, the EC2 instance won't see it
  - good to maintain one separate SG for SSH access (Maarek opinion)
  - timeout errors - SG issue
  - connection refused errors - application error
  - **all inbound traffic is blocked by default**
  - **all outbound traffic is authorized by default**

#

## 25. Private vs Public vs Elastic IP

- networking has 2 types of IP addresses: v4 and v6
- v4 allows for 3.7 billion diff addresses in public space
- public IP
  - machine can identified on the internet
  - must be unique across the web
  - can be geolocated easily
- Private IP
  - machine can only be identified on private network
  - IP must be unique across the private network
  - but 2 diff private networks can have the same IP
  - machines connect to WWW via internet gateway
  - only a specified range of IPs can be used as private
- Elastic IP
  - when you start/stop an EC2 instance, it can change it's public IP address
  - if you need a fixed public IP for your instance, you can use an Elastic IP
  - an elastic IP is a public v4 IP address as long as you don't delete it
  - you can remap from one instance to another
  - you can only have 5 Elastic IPs in your acct
  - you can ask AWS to increase that amt
  - overall, try to avoid using Elastic IPs
    - reflect poor architectural decisions
    - instead use a random public IP and register a DNS name to it
    - or use a load balancer and dont use public IP
- by default EC2 instances come with:
  - a private IP for internal AWS network
  - public IP for WWW
  - when we SSH into instance:
    - cant use private IP because not on same network
    - only use public
  - if your machine is stopped, then re-started:
  - **the public ip can change**
  #

## 26. Private vs Public vs Elastic IP Hands On

- stop/start instances will assign you a new IP address
- Elastic IPs will not change if you stop/start an instance

#

## 27. Install Apache on EC2

- install Apache web server to display a web page
- create an index.html page
- ran sudo to give full privileges
- yum updates to machine
- installed httpd -> yum install -y httpd.x86_64
- started the httpd service and remain across reboots
- ran curl on localhost:80
- change the SG to allow port 80
- updated the index.html at /var/www/html

#

## 28. EC2 User Data

- possible to bootstrap EC2 instances using EC2 User Data script
- bootstrapping means launching commands when an instance starts
- script runs once at the start of instance
- use EC2 user data to automate tasks such as:
  - installing updates
  - installing software
  - download any common files from internet
- script runs with root user

- there's a file w/the commands in the course resources
#