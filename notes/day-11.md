# Day 11 \- March 18

**Amazon S3**

- S3 is one of the main building blocks of AWS  
- It's advertised as infinitely scalable storage  
- Many websites and AWS services use Amazon S3 as a backbone for integration  
- Use cases: backup and storage, DR, archive, hybrid cloud storage, application hosting, media hosting, data lakes, etc.

**Amazon S3 Buckets**

- Amazon S3 allows you to store objects/files in buckets (directories)  
- Buckets are defined at the Region level  
- S3 looks like a global service, but buckets are created in a Region  
- No uppercase letters, no underscores, 3-63 characters long, not an IP, must not start with a prefix, must not end with a suffix

**Amazon S3 Objects** 

- Objects (files) have a key  
- The key is the full path: `s3://my-bucket/my_file.txt`  
- The key is composed of prefix + object name  
- There's no concept of directories within buckets, although the UI may trick you into thinking otherwise  
- Just keys with very long names that contain slashes  
- Max object size is 5 TB. If you need more, you must use multipart upload

**Amazon S3 Security**

- User-based: IAM policies attached to a specific user from IAM  
- Resource-based:  
  - Bucket policies: bucket-wide rules from the S3 console; allow cross-account access  
  - Object Access Control List (ACL): finer-grained, can be disabled  
  - Bucket Access Control List (ACL): less common, can be disabled

**Amazon S3 Object Access** 

- An IAM principal can access an S3 object if the user IAM permissions allow it OR the resource policy allows it, and there is no explicit deny.  
- Encryption: encrypt objects in Amazon S3 using an encryption key 

**S3 Bucket Policies**

- JSON-based policies  
  - Resources: buckets and objects  
  - Effect: allow/deny  
  - Actions: set of APIs to allow or deny  
  - Principal: the account or user to apply the policy to  
- Use S3 bucket policies to:  
- Grant public access to the bucket  
- Force objects to be encrypted at upload  
- Bucket settings for block public access. These settings were created to prevent company data leaks  
- Grant access to another account (cross account)

**S3 Bucket Policies**

- JSON-based policies

**Amazon S3 Static Website Hosting**

- S3 can host static websites and make them accessible on the internet  
- The website URL will depend on the Region  
- If you get 403, make sure the bucket allows public read

**Amazon S3 Versioning**

- You can version your files in Amazon S3  
- It is enabled at the bucket level  
- Overwriting the same key changes the version: 1, 2, 3...  
- It is best practice to version your buckets to protect against unintended deletes (ability to restore a version)  
- Easy rollback to a previous version  
- Any file that is not versioned will have version null

**Amazon S3 Replication (CRR/SRR)**

- Must enable versioning in source and destination buckets  
- Cross-region replication (CRR)  
- Same-region replication (SRR)  
- Buckets can be in different AWS accounts  
- Copying is asynchronous  
- Must give proper IAM permissions to S3  
- Use cases: CRR - compliance, lower-latency access, replication across accounts. SRR - log aggregation, live replication between production and test accounts  
- After you re-enable replication, only new objects are replicated  
- Optionally, you can replicate existing objects using S3 Batch Replication  
  - Replicates existing objects and objects that failed replication  
- For delete operations: can replicate delete markers from source to target (optional)  
- Deletions with a version ID are not replicated (to avoid malicious deletes)  
- There's no chaining of replication  
  - If bucket 1 has replication to bucket 2, which has replication into bucket 3  
  - Then objects created in bucket 1 are not replicated to bucket 3

**S3 Storage Classes**

- **S3 Standard - General Purpose**  
- **S3 Standard-Infrequent Access (IA)**  
- **S3 One Zone-Infrequent Access**  
- **S3 Glacier Instant Retrieval**   
- **S3 Glacier Flexible Retrieval**   
- **S3 Glacier Deep Archive**  
- **S3 Intelligent-Tiering**  
- Can move between classes manually or using lifecycle configuration

**S3 Durability and Availability**

- **Durability:**   
  - High durability (99.999999999): 11 9s of objects across multiple AZs  
  - If you store 10,000,000 objects with Amazon S3, you can expect to incur a loss of a single object once every 10,000 years on average  
  - Same for all storage classes  
- **Availability:**   
  - Measures how readily available a service is  
  - Varies depending on storage class  
  - Example: S3 Standard has 99.99% availability, meaning it is not available for about 53 minutes a year

**S3 Standard - General Purpose**

- 99.99% availability  
- Used for frequently accessed data  
- Low latency and high throughput  
- Sustain two concurrent facility failures  
- Use cases: big data analytics, mobile and gaming applications, content distribution, ...

**S3 Storage Classes - Infrequent Access**

- For data that is less frequently accessed but requires rapid access when needed  
- Lower cost than S3 Standard  
- **Amazon S3 Standard-Infrequent Access (S3 Standard-IA):** 99.99% availability, use cases: DR, backups  
- **Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)**  
  - High durability (99.99999999999), single AZ: data is lost when the AZ is destroyed  
  - 99.5% availability  
  - Use case: storing secondary backup of on-premise data or data you can recreate

**S3 Glacier Storage Classes**

- Low-cost object storage meant for archiving/backup  
- Pricing: price for storage + object retrieval cost   
- **Amazon S3 Glacier Instant Retrieval:** millisecond retrieval, great for data accessed once a quarter, minimum storage duration of 90 days  
- **Amazon S3 Glacier Flexible Retrieval:** Expedited (1-5 mins), Standard (3-5 hrs), Bulk (5-12 hrs) - free    
- **Amazon S3 Glacier Deep Archive:** for long-term storage. Standard (12 hrs), Bulk (48 hrs), minimum storage duration of 180 days

**S3 Intelligent-Tiering**

- Small monthly monitoring and auto-tiering fee  
- Moves objects automatically between access tiers based on usage  
- There are no retrieval charges in S3 Intelligent-Tiering  
- Frequent Access tier (automatic): default tier  
- Infrequent Access tier (automatic): objects not accessed for 30 days   
- Archive Instant Access tier (automatic): objects not accessed for 90 days   
- Archive Access tier (optional): configurable from 90 days to 700+ days   
- Deep Archive Access tier (optional): configurable from 180 days to 700+ days

**S3 Express One Zone**

- High-performance single-AZ storage class  
- Objects stored in a directory bucket (bucket in a single AZ)  
- Handles 100k requests per second with single-digit ms latency  
- Up to 10x better performance than S3 Standard (50% lower costs)  
- High durability and 99.95% availability  
- Use cases: latency-sensitive apps, data-intensive apps, AI and ML training, financial modeling, media processing, HPC  
- Best integrated with SageMaker model training, Athena, EMR, Glue...
