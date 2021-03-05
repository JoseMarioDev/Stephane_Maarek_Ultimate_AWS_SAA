## 223. AWS Security - Section introduction

- introduction
- focal part of the exam

#

## 224. Encryption 101

- encryption in flight(SSL)
  - data is encrypted before sending and decrypted after receiving
  - SL certs helps w/encryption(HTTPS)
  - encryption in flight ensures no MITM attack
- Server side encryption at rest
  - data is encrypted after being rec'd by server
  - dta is decrypted before being sent
  - stored in encrypted form thanks to a key(usually a data key)
  - enryption/decrpytion keys must be managed somewhere and the server must have access to it
- client side encryption
  - data is encrypted by the client and never decrypted by the server
  - data will be decrypted by a receiving client
  - the server should not be able to decrypt the data
  - could leverage envelope encryption

#

## 225. KMS Overview

- Key Mgmt Service
  - easy to control access to your data, AWS manages keys for us
  - fully integrated w/IAM for authorization
  - seamlessly integrated into: EBS, S3, Redshift, RDS, SSM
  - you can also use CLI/SDK
- KMS - Customer Master Key(CMK) Types
  - Symmetric(AES-256 keys)
    - single encryption key used for encrypt/decrypt
    - AWS services that are integrated with KMS use Symmetric CMKs
    - necessary for envelope encryption
    - you never get access to the key unencrypted(must call KMS API to use)
  - Asymmetric (RSA & ECC Key pairs)
    - public(encrypt) and Private(decrypt) key pair
    - used for encrypt/decrypt or sign/verify operations
    - public key is downloadable, but you cant access the private key unencrypted
    - use case: encryption outside of AWS by users who cant call the KMS API
- able to fully manage keys and policies:
  - create
  - rotation policies
  - disable
  - enable
- able to audit key usage using cloudtrail
- three types of CMK
  - AWS managed
  - user keys created in KMS
  - user keys imported
- KMS 101
  - anytime you need to share sensitive information use KMS
  - database passwords
  - creds to external services
  - private key of SSL certs
- value in KMS is that the CMK used to encrypt data can never be retrieved by the user, and the CMK can be rotated for extra security
- KMS can only help in encryption up to 4kb of data per call
- KMS keys are bound to a specific region
- key policies
  - similar to S3 bucket policies
  - you cannot control access without them
  - default KMS key policies
    - created if you dont specify a specific policy
    - complete access to root user

#

## 226. KMS Hands On w/CLI

- 

#

## 227. SSM Parameter Store Overview

-

#

## 228. SSM Parameter Store Hands On(CLI)

-

#

## 229. SSM Parameter Store Hands On(AWS Lambda)

-

#

## 230. AWS Secrets Manager - Overview

-

#

## 231. AWS Secrets Manager - Hands On

-

#

## 232. CloudHSM

-

#

## 233. Shield - DDoS Protection

-

#

## 234. Web Application Firewall (WAF)

-

#

## 235. WAF & Shield - Hands On

-

#

## 236. Shared Responsibility Model

-

#

## Quiz 19: Security & Encryption Quiz

-

#
