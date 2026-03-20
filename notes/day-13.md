# Day 13 \- March 20

## Amazon S3 Object Encryption
- you can encrypt objects in amazon s3 buckets using one of 4 methods
- Server-side Encryption
  - Server side encryption with Amazon S3 managed keys (SSE-S3) - enabled by default
    - Encypts s3 objects using keys handled, managed and owned by AWS
  - Server side encryption with AWS KMS managed keys (SSE-KMS)
    - Leverage AWS Key Management Service (KMS) to manage encryption keys
  - Server side encryption with customer provided keys (SSE-C)
    - when you want to manage your own encryption keys
- Client-side Encryption
## Amazon S3 Encryption SSE-S3
- Encryption using keys handled, managed and owned by AWS
- Object is encrypted server side
- encryptoin type is AES-256
- must set header "x-amz-server-side-encryption" to "AES256"
- Enabled by default when you create a bucket
## Amazon S3 Encryption SSE-KMS
- ENcryption using keys handled, managed and owned by AWS KMS
- kms advantages: user control + audit key usage using cloudtrail
- object is encrypted server side
- must set header "x-amz-server-side-encryption" to "aws:kms"
## SSE-KMS Limitations
- if you use SSE-KMS, you may be impacted by KMS Limits
- when you upload, it calls the GenerateDataKey API
- when you download, it calls the Decrypt API KMS API
- count towards the KMS qoute per second (5500, 10000, 30000 req/s based on region)
- you can request a qouta increase using the service qoutas console
## Amazon S3 Encryption SSE-C
- Server side encryption using keys fully managed by the customer outside of AWS
- amazon s3 does not store the encryption key you provide
- HTTPS must be used
- encryption key must be provided in HTTP headers for every HTTP request made
## Amazon S3 Encryption Client Side Encryption
- Use client libraries such as Amazon s3 client side encryption library
- client must encrypt data themselves before sending to amazon s3
- clients must decrypt data themselves after retrieving from amazon s3
- customer fully manages the keys and encryption cycle
## Amazon s3 encryption in transit (SSL/TLS)
- Encryption in flight is also called SSL/TLS
- Amazon S3 exposes two endpoints:
  - HTTP Endpoint - non encrypted
  - HTTPS Endpoint - encryption using in flight
- HTTPS is recommended
- HTTPS is mandatory for SSE-C
- Most client would use the HTTPS endpoint by default
## Amazon S3 Default Encryption vs Bucket Policy
- SSE-S3 encryption is automatially applied to new objects stored in S3 bucket
- optionally you can "force encryption" using a bucket policy and refuse any API call to PUT an S3 object without encryption headers (SSE-KMS or SSE-C)
- note: bucket policies are evaluated before "default encryption"
## CORS (Cross-Origin Resource Sharing)
- Origin = scheme protocol + host + port
  -example: http://www.example.com (implied port is 443 for https and 80 for http)
- Web browser based mechanism to allow requests to other origins while visiting the main origin
- Same origin: http://example.com/app1 and http://example.com/app2  
- different origin: http://example.com and http://www.example.com
- the request wont be fulfilled unless the origin allows for the request using CORS headers (example: Access-Control-Allow-Origin)
## Amazon CORS
- if a client makes a cross-origin request on our S3 bucket we need to enable the correct CORS headers
- it's a popular exam question
- you can allow for a specific origin or for * (all origins)
## Amazon S3 MFA Delete
- MFA (multi factor authentication) forces users to generate code on a device usually a mobile phone app (google authenticator, authy, etc) before doing important operations on S3
- MFA will be required to:
  - Permanently delete an object version
  - Suspend versioning on the bucket
- MFA wont be required to:
  - enable versioning
  - list deleted versions
- To use MFA delete, versioning must be enabled on the bucket
- only the bucket owner (root account) can enable MFA delete 
## S3 Access Logs
- For audit purpose, you may want to log all access to your S3 bucket
- any request made to s3, from any account, authorized or denied, will be logged in to another s3 bucket
- that data can be analyzed using data analysis tools...
- the target logging bucket must be in the same AWS region
## S3 Access Logs: Warning
- do not set your logging bucket to be in the monitored bucket
- it will create a logging loop and your bucket will grow exponentially
## Amazon S3 pre signed URLs
- generate pre signed urls using the S3 console, aws cli, or sdk
- URL expiration
  - s3 console 1min up to 720mins (12hrs)
  - AWS cli configure expiration with ==expires-in parameter in seconds (default 3600 secs, max. 604700 secs ~ 168hrs)
  - Users are given a pre signed url inherit the permissions of the user that generated the URL for GET/PUT
- Examples: allow only logged in users to download file
## S3 Glacier Vault Lock
- Adopt a WORM (write once read many)model 
- create a vault lock policy
- lock the policy for future edits (can no longer be changed or deleted)
- helpful for complience and data retention
## S3 object Lock (versioning must be enabled)
- Adopt a owrm model
- block an object version deletion for a specified amount of time
- Retention mode - Compliance:
  - objects versions cant be over written or deleted by any user, inccluding the root user
  - objects retention modes cant be changed, andretention periods cant be shortened
- Retention mode - governance:
  - most users cant overwrite or delete an object version or alter its lock settings
  - some users have special permissions to change the retention or delete the object
- retention period: protect the object for a fixed period, it can be extended 
- Legal hold:
  - protect the object indefintely, independent from retention period
  - can be freely placed and remoed using the s3:PutObjectLegalHold IAM permission
## S3 Access points
- access point simplify security management for s3 buckets
- each access point has:
  - its own DNS name (internet origin or VPC origin)
  - an access point policy (similar to bucket policy) - manage security at scale
## Access points vpc origin
- we can define the access point to be accessible only from within the vpc 
- you must create a vpc endpoint to access the Access Point (gate way or interface endpoint)
- The vpc endpoint policy must allow access tothe target bucket and access point\
## S3 object lambda
- use AWS lambda functions to change the object before it is retrieved by the caller application
- only one s3 bucket is needed on top of which we create s3 access point and s3 object lambda access points
- use cases: redacting personally identifiable information for analytics or non production environments, converting across data formats such as conveting xml to json