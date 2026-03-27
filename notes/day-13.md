# Day 13 \- March 20

## Amazon S3 Object Encryption
- You can encrypt objects in Amazon S3 buckets using one of four methods.
- Server-side Encryption
- Server-side encryption with Amazon S3 managed keys (SSE-S3) - enabled by default
    - Encrypts S3 objects using keys handled, managed, and owned by AWS.
- Server-side encryption with AWS KMS managed keys (SSE-KMS)
    - Leverages AWS Key Management Service (KMS) to manage encryption keys.
- Server-side encryption with customer-provided keys (SSE-C)
    - Use this when you want to manage your own encryption keys.
- Client-side Encryption
## Amazon S3 Encryption SSE-S3
- Encryption using keys handled, managed, and owned by AWS.
- Object is encrypted server side.
- Encryption type is AES-256.
- Must set header "x-amz-server-side-encryption" to "AES256".
- Enabled by default when you create a bucket
## Amazon S3 Encryption SSE-KMS
- Encryption using keys handled, managed, and owned by AWS KMS.
- KMS advantages: user control plus auditing key usage using CloudTrail.
- Object is encrypted server side.
- must set header "x-amz-server-side-encryption" to "aws:kms"
## SSE-KMS Limitations
- If you use SSE-KMS, you may be impacted by KMS limits.
- When you upload, it calls the GenerateDataKey API.
- When you download, it calls the KMS Decrypt API.
- Count toward the KMS quota per second (5,500, 10,000, or 30,000 req/s depending on the Region).
- You can request a quota increase using the Service Quotas console.
## Amazon S3 Encryption SSE-C
- Server-side encryption using keys fully managed by the customer outside of AWS.
- Amazon S3 does not store the encryption key you provide.
- HTTPS must be used.
- The encryption key must be provided in HTTP headers for every HTTP request made.
## Amazon S3 Encryption Client Side Encryption
- Use client libraries such as the Amazon S3 client-side encryption library.
- Clients must encrypt data themselves before sending to Amazon S3.
- Clients must decrypt data themselves after retrieving from Amazon S3.
- The customer fully manages the keys and encryption cycle.
## Amazon s3 encryption in transit (SSL/TLS)
- Encryption in flight is also called SSL/TLS.
- Amazon S3 exposes two endpoints:
  - HTTP Endpoint - non encrypted
  - HTTPS Endpoint - encryption using in flight
- HTTPS is recommended
- HTTPS is mandatory for SSE-C
- Most clients would use the HTTPS endpoint by default.
## Amazon S3 Default Encryption vs Bucket Policy
- SSE-S3 encryption is automatically applied to new objects stored in an S3 bucket.
- Optionally, you can "force encryption" using a bucket policy and refuse any API call to PUT an S3 object without encryption headers (SSE-KMS or SSE-C).
- Note: bucket policies are evaluated before default encryption.
## CORS (Cross-Origin Resource Sharing)
- Origin = scheme protocol + host + port
  -example: http://www.example.com (implied port is 443 for https and 80 for http)
- Web browser based mechanism to allow requests to other origins while visiting the main origin
- Same origin: http://example.com/app1 and http://example.com/app2  
- different origin: http://example.com and http://www.example.com
- The request will not be fulfilled unless the origin allows it using CORS headers (for example, `Access-Control-Allow-Origin`).
## Amazon CORS
- If a client makes a cross-origin request to our S3 bucket, we need to enable the correct CORS headers.
- It is a popular exam question.
- You can allow a specific origin or `*` for all origins.
## Amazon S3 MFA Delete
- MFA (multi-factor authentication) forces users to generate a code on a device, usually a mobile phone app (Google Authenticator, Authy, etc.), before performing important operations on S3.
- MFA will be required to:
  - Permanently delete an object version
  - Suspend versioning on the bucket
- MFA will not be required to:
  - enable versioning
  - list deleted versions
- To use MFA delete, versioning must be enabled on the bucket
- Only the bucket owner (root account) can enable MFA Delete.
## S3 Access Logs
- For audit purposes, you may want to log all access to your S3 bucket.
- Any request made to S3, from any account, authorized or denied, will be logged to another S3 bucket.
- That data can be analyzed using data analysis tools.
- The target logging bucket must be in the same AWS Region.
## S3 Access Logs: Warning
- Do not set your logging bucket to be the monitored bucket.
- It will create a logging loop and your bucket will grow exponentially.
## Amazon S3 pre signed URLs
- Generate presigned URLs using the S3 console, AWS CLI, or SDK.
- URL expiration
  - S3 console: 1 minute up to 720 minutes (12 hours)
  - AWS CLI: configure expiration with `--expires-in` in seconds (default 3,600 seconds, max. 604,800 seconds, about 168 hours)
  - Users given a presigned URL inherit the permissions of the user that generated the URL for GET/PUT
- Examples: allow only logged in users to download file
## S3 Glacier Vault Lock
- Adopt a WORM (write once, read many) model.
- Create a vault lock policy.
- Lock the policy for future edits so it can no longer be changed or deleted.
- Helpful for compliance and data retention.
## S3 object Lock (versioning must be enabled)
- Adopt a WORM model.
- Block an object version from being deleted for a specified amount of time.
- Retention mode - Compliance:
  - Object versions cannot be overwritten or deleted by any user, including the root user.
  - Object retention modes cannot be changed, and retention periods cannot be shortened.
- Retention mode - governance:
- Most users cannot overwrite or delete an object version or alter its lock settings.
- Some users have special permissions to change the retention or delete the object.
- Retention period: protect the object for a fixed period; it can be extended.
- Legal hold:
- Protect the object indefinitely, independent of the retention period.
- Can be freely placed and removed using the `s3:PutObjectLegalHold` IAM permission.
## S3 Access points
- Access points simplify security management for S3 buckets.
- Each access point has:
  - Its own DNS name (internet origin or VPC origin)
  - An access point policy (similar to a bucket policy) for managing security at scale
## Access points vpc origin
- We can define the access point to be accessible only from within the VPC.
- You must create a VPC endpoint to access the access point (gateway or interface endpoint).
- The VPC endpoint policy must allow access to the target bucket and access point.
## S3 object lambda
- Use AWS Lambda functions to change the object before it is retrieved by the caller application.
- Only one S3 bucket is needed, on top of which we create S3 access point and S3 Object Lambda access points.
- Use cases: redacting personally identifiable information for analytics or non-production environments, converting across data formats such as converting XML to JSON.
