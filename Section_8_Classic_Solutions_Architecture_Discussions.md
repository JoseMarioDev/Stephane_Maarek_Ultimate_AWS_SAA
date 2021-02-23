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

- stateful web application
-

#
