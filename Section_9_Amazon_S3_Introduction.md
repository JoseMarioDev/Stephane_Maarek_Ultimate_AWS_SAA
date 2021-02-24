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

-

#
