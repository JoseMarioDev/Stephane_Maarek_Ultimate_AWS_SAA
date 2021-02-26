## 142. CloudFront Overview

- is a CDN - content delivery network
- improves read performance, content is cached at the edge
- 216 points of presence globally(edge locations)
- also gives you:
  - DDoS protection
  - integration with Shield
  - AWS web application firewall
- can expose external HTTPS and can talk to internal HTTPS backends
- distributes your reads all around the world at these edge locations
- CloudFront Origins
  - S3 bucket
    - for distributing files and caching them at the edge
    - enhanced security with CF Origin Access Identity(OAI)
      - allows S3 bucket to only allow communication from CF only
    - use as an Ingress to upload files to S3
  - custom origin(HTTP)
    - application load balancer
    - EC2 instance
    - S3 website
    - any HTTP backend you want
- CF at a high level
  - edge locations all around the globe
  - connected to the origin we defined(S3 bucket or HTTP endpt)
  - client sends HTTP request to CF
  - edge location forwards request to origin
  - origin res to edge location
  - edge caches res
  - returns res to client
  - next time, the edge location checks cache before hitting origin
- CF - S3 as an origin
  - edge locations fetch data from S3 bucket over private AWS network
  - CF using an OAI as an IAM role to get access to the files in the S3 bucket
- CF - ALB or EC2 as an origin
  - instances must be public
  - users access edge, edge access instance
  - instance must allow public IP of edge locations. there is a list
  - if using an ALB, must be public
  - use sec group, the backend instances can be private
  - the ALB must allow the public IPs of the edge locations
- CF Geo restriction
  - you can restrict who can access your distribution
    - whitelist
    - blacklist
  - the country is determined using a 3rd party geo-ip database
  - use case: copyright laws to control access to content
- CF vs S3 Cross Region Replication
  - CF
    - global edge network
    - files are cached for a TTL
    - great for static content
  - S3 CRR
    - must be setup for each region you want replication to happen
    - files are updated in near real time
    - great for dynamic content
    - read only

#

## 143. CloudFront with S3 - Hands On

- create an S3 bucket
- create a cloudfront distribution
- create an OAI - user for the CF and bucket
- we'll limit the S3 bucket to accessed only by the OAI

- cloudfront console
  - create distribution
  - origin domain name is the bucket you'll be using
  - restrict bucket yes -> limit access only to OAI
  - create new OAI or use existing one
  - grant read permissions
  - view options
  - create distribution
  - you see the policy created in the bucket permissions

#

## 144. Cloudfront Signed URL/Cookies

- 

#
