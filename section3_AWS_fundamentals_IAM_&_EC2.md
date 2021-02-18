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

## 14. IAM Hands-on

- creating user acct
- create admin group, apply admin policy to group
- add created user to group
- change alias to company name
- log in as new user
- voila!

## 15. changes to the EC2 console

- nothing here, doc showing new changes to look of EC2 console

## 16. EC2 introduction
