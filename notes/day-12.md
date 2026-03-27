## Day 12 \- March 19

### Amazon S3 - Moving Between Storage Classes
- You can transition objects between storage classes
- For infrequently accessed objects, you can move them to Standard IA.
- For archive objects that you do not need fast access to, use Glacier or Glacier Deep Archive.
- Moving objects can be automated using lifecycle rules.
### Amazon S3 - Lifecycle Rules
- Transition Actions - configure objects to transition to another class
  - Move objects to the Standard IA class 60 days after creation.
  - Move to Glacier for archiving after 6 months.
- Expiration Actions - configure objects to expire (delete) after some time
  - Access log files can be set to delete after 365 days.
  - Can be used to delete old versions of files if versioning is enabled.
  - Can be used to delete incomplete multipart uploads.
- Rules can be created for a certain prefix (example: `s3://mybucket/mp3/*`).
- Rules can be created for certain object tags (example: `Department:finance`).

Example Scenario:
#### A rule in your company states that you should be able to recover your deleted S3 objects immediately for 30 days, although this may happen rarely. After this time and for up to 365 days, deleted objects should be recoverable within 48 hours.

#### Enable S3 Versioning in order to have object versions so that "deleted" objects are in fact hidden by a "delete marker" and can be recovered. Transition the "noncurrent versions" of the object to Standard IA.

### S3 Analytics - Storage Class Analysis
- Helps you decide when to transition objects to the right storage class.
- Recommendations are available for Standard and Standard IA. This does not work for One Zone IA or Glacier.
- The report is updated daily.
- It takes 24-48 hours to start seeing data analysis.
### S3 - Requester Pays
- In general, bucket owners pay for all Amazon S3 storage and data transfer costs associated with their bucket.
- With Requester Pays buckets, the requester, instead of the bucket owner, pays the cost of the request and the data download from the bucket.
- Helpful when you want to share large data sets with other accounts.
- The requester must be authenticated in AWS and cannot be anonymous.
### S3 Event Notifications
- S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication...
- Use case: generate thumbnails uploaded to s3
- Can create as many "s3 events" as desired
- S3 event notifications typically deliver events in seconds, but they can sometimes take a minute or longer.
### S3 Event Notifications - IAM permissions
- For the S3 event notification to work, we need IAM permissions.
- we need to attach SNS Resource (access) policy, SQS Resource (access) policy, Lambda Resource (access) policy
### S3 Event Notifications with Amazon Event Bridge
- Advanced filtering options with Json Rules (metadata, object size, name)
- Multiple destinations - e.g. Step Functions, Kinesis Streams, Firehose...
- EventBridge capabilities - archive, replay events, reliable delivery
### S3 Baseline Performance
- Amazon S3 automatically scales to high request rates, with latency of 100-200 ms.
- Your application can achieve at least 3,500 PUT/POST/DELETE or 5,500 GET requests per second per prefix in a bucket.
- There are no limits to the number of prefixes you can have in a bucket.
- Example (object path => prefix)
  - bucket1/folder/sub1/file1 => folder/sub1
- If you spread reads across all four prefixes evenly, you can achieve 22,000 read requests per second for GET and HEAD.
### S3 Performance
- Multi Part Upload
  - Recommended for files > 100 MB, and required for files > 5 GB.
  - Can help parallelize the upload and reduce time.
- S3 Transfer Acceleration
  - Increase transfer speed by transferring a file to an AWS edge location, which forwards the data to the S3 bucket in the target Region.
  - Compatible with multipart upload.
### S3 Performance - S3 Byte range fetches
- Parallelize GETs by requesting byte ranges
- better resilience in case of failures
- can be used to speed up downloads
- Can be used to retrieve only partial data (for example, the head of a file).
### S3 Batch operations
- Perform bulk operations on existing S3 objects with a single request. Example:
  - Modify object metadata and properties.
  - Copy objects between buckets or within the same bucket.
  - Encrypt unencrypted objects.
  - Modify ACLs and tags.
  - Restore objects from S3 Glacier.
  - Invoke Lambda functions to perform a custom action on each object.
- A job consists of a list of objects, the action to perform, and optional parameters.
- S3 Batch Operations manages retries, tracks progress, sends completion notifications, and generates reports.
- You can use S3 Inventory to get an object list and use Athena to query and filter your objects.
### S3 - Storage Lens
- Understand, analyze, and optimize storage across your entire AWS organization.
- Discover anomalies, identify cost efficiencies, and apply data protection best practices across the organization (30 days of usage and activity metrics).
- Aggregate data for the organization, specific accounts, Regions, buckets, or prefixes.
- Use the default dashboard or create your own dashboard.
- Can be configured to export metrics daily to an S3 bucket (CSV, Parquet).
### S3 - Default Dashboard
- Visualize summarized insights and trends for both free and advanced metrics
- The default dashboard shows multi-Region and multi-account data preconfigured by Amazon S3.
- It cannot be deleted, but it can be disabled.
### S3 - metrics
- Summary metrics
  - General insights about your S3 storage.
  - StorageBytes, ObjectCount..
  - Use cases: identify the fastest-growing or unused buckets and prefixes.
- Cost optimization metrics 
  - Provide insights to manage and optimize your storage costs.
  - NonCurrentVersionStorageBytes, IncompleteMultipartUploadStorageBytes...
- Data Protection metrics
  - Provide insights into data protection features.
  - VersioningEnabledBucketCount, MFAdeleteEnabled BucketCount..
  - Use cases: identify buckets that are not using data protection features and apply best practices.
- Access management metrics
  - Provide insights for S3 object ownership.
  - Use cases: identify which object ownership mode your buckets use.
- Event metrics
  - Provide insights about S3 event notifications.
- Performance metrics
  - Provide insights about S3 Transfer Acceleration.
- Activity metrics
  - Provide insights about how your storage is accessed.
- Detailed Status Code Metrics
  - Provide insights into HTTP status codes.
### S3 lens Free vs Paid
- Free metrics
  - Automatically available for all customers
  - Contains around 28 usage metrics
  - Data available for queries for 14 days
- Advanced metrics and recommendations
  - Additional paid metrics and features
  - Advanced metrics: activity, advanced cost, optimization, advanced data protection, status code
  - CloudWatch publishing
  - Prefix aggregations
  - Data is available for queries for 15 months
- The default dashboard has data across multiple accounts and multiple Regions.
