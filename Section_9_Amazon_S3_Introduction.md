# 99. Amazon S3 - Section Introduction

- powers some of the biggest websites in the world
- one of the main building blocks of AWS
- advertised as an "infinitely scaling" storage
- widely popular

#

## 100. S3 Buckets and Objects

- S3 allows ppl to store objects(files) in "buckets" (directories)
- buckets must have a globally unique name
- buckets are defined at the region level
- naming convention
  - no uppercase
  - no underscore
  - 3-63 characters long
  - not an IP
  - must start w/lowercase letter or number
- objects are files - must have a key
- Objects overview:
  - objects(files) have a key
  - key is full path: ex: s3://mybucket/myfile.txt
  - ex: s3://mybucket/myfolder/myfile.txt
    key is composed of prefix+object name
    - ex: /myfolder/myfile.txt
    - there's no concept of "directories" within buckets
      - the UI will trick you to think otherwise
      - "flat"
    - just keys with very long names that contains slashes
- overview (cont'd)
  - object values are the content of the body:
    - max object size is 5TB
    - if uploading more than 5GB, must use multi-part upload
  - metadata(list of text key/value pairs - system or user metadata)
  - tags - useful for security/lifecycle
  - version ID(if versioning is enabled)

#

## 101. S3 Buckets and Objects - Hands On

- move to S3 console
- choose globally unique name
- choose region(console is global, but S3 is regional)
- block public access settings
- bucket versioning
- create bucket
- create an object
- upload object
- notice you can't view it using the public URL because the bucket isnt public

#

## 102. S3 Versioning

- you can version your files
- enabled at bucket level
- same key overwrite will increment the version - 1,2,3, etc
- best practice to version your buckets
  - protect against unintended deletes(ability to restore)
  - easy rollback to previous version
- any file that is not versioned prior to enabling versioning will have a "null" version
- suspending versioning doesn't delete the previous versions

#

## 103. S3 Versioning - Hands On

- properties tab
- enable versioning
- toggle "List versions" slider
- every time we upload a file, it keeps the previous version and assigns a diff version ID
- try to delete file, adds delete maker
  - doesnt actually delete file
- restoring file -> delete the delete maker

- to perm delete, select the object and actions -> delete

#

## 104. S3 Encryption

- 4 methods of enrcrypting objects in S3
1. SSE-S3: encrypts S3 objects using keys handled and managed by AWS
2. SSE-KMS: leverage AWS Key Management Service to manage encryption keys
3. SSE-C: when you want to manage your own encryption keys
4. client side encryption

- SSE-S3:
  - encryption using keys handled/managed by S3
  - object encryption is server side
  - AES-256 encryption type
  - can be HTTP or HTTPS
  - must set the header: "x-amz-server-side-encryption":"AES256"

- SSE-KMS:
  - encryption using keys handled/managed by AWS
  - KMS advantages: user control and audit trail
  - object encryption is server side
  - can be HTTP or HTTPS
  - must set header: "x-amz-server-side-encryption": "aws:kms"
- SSE-C:
  - server side encryption using data keys fully managed by the customer
  - Amazon S3 does not store the encryption key you provide
  - HTTPS must be used
  - encryption key must be provided in HTTP headers for every HTTP request made
  - good illustration in the lesson
- client side encrpytion
  - encryption is handled/managed fully by the client prior to uploading to S3
  - can use libraries such as Amazon S3 Encryption Client
  - clients must encrpyt data prior to sending to S3
  - clients must decrpyt data themselves when retrieving from S3
- encryption in transit(SSL/TLS)
  - Amazon S3 exposes:
    - HTTP endpoint: not encrypted
    - HTTPS endpoint: encrpytion in flight
  - free to use either, but HTTPS is recommended
  - most clients would use the HTTPS endpoint by default
  - HTTPS is mandatory for SSE-C

#

## 106. S3 Security & Bucket Policies

-

#

## 107. S3 Bucket Policies Hands On

-

#

## 108. S3 Websites

-

#

## 109. S3 CORS

-

#

## 110. S3 CORS Hands On

-

#

## 111. S3 Consistency Model

-

#

## Quiz 8: AWS S3 Quiz

-

#
