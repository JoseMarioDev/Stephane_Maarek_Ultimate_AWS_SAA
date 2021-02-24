## 92. Solutions Architecture Discussions Overview

- how the diff technologies all fit together
- section focuses on solution architecture, focusing on case studies

#

## 93. WhatsTheTime.com

- stateless web application

- website that eventually scales to multiAZ with ELB and ASG using an alias record on rt 53 to connect to the ELB. Also using security groups on the EC2 instances to make sure they only communicate with the ELB

- focuses on the 5 pillars of a well architected framework

1. cost
2. performance
3. reliability
4. security
5. operational excellence

#

## 94. MyClothes.com

- adds on the arch from previous lecture
- can use ELB stickiness
- introduces Elaticache for storing user sessions
- can also store them in DynamoDB
- introduces RDS for storing user info(address, etc)
- read replicas to improve performance
- MultiAZ standby for DR
-

#

## 95. MyWordPress.com

- adds Aurora DB to have easy MultiAZ and read-replica
- storing data in EBS is okay for single instance but:
- storing data in EFS is better for distributed applications when we have multiple instances/AZ
-

#

## 96. Instantiating Applications Quickly

-

#
