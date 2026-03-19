## Day 12 \- March 19

###  Amazon S3 - Moving between storage classes
- You can transition objects between storage classes
- for infrequently accessed object you can move them to Standard IA
- for arcvhive objects that you dont need fast access to glacier or glacier deep archive
- moving objects can be automated using lifecycle rules
### Amazon S3 -  Lifecycle Rules
- Transition Actions - configure objects to transition to another class
  -move objects to standard IA class 60 days after creation
  - move to glacier for achiving after 6 months
- Expiration Actions - configure objects to expire (delete) after some time
  - Acess log files can be set to delete after 365 days
  - can be used to delete old versions of files (if versioning is enabled)
  - Can be used to delete incomplete multi-part uploads
- Rules can be created for a certain prefix (example:s3://mybucket/mp3/*)
- Rules can be created for a certain objects Tags (example:Department:finance)

Example Scenario:
#### A rule in your company staes that you should be able to recover your deleted S3 objects immediately for 30 days although this may happen rarely. After thistime and for up to 365 days, deleted objects should be recoverable within 48 hours.

#### Enable S3 Versioning in order to have object versions so that "deleted" are in fact hidden by a "delete marker" and can be recovered. Transition the "noncurrent versions" of the object to standard IA

### Storage S3 analytics - storage class analysis
- Help you decide when to transition objects to the right storage class
- Recommendations for standard and Standard IA. Does not work for one zone IA or glacier
- report is updated daily
- 24-48 hrs to start seeing data analysis
### S3 - Requester pays
- In general, bucket owners pay for all Amazon s3 storage and data transfer cost associated with their bucket
- With Requester pays buckets, the requester instead of the bucket owner pays the cost of the request and the data download from the bucket
- helpful when you want to share large data sets with other accounts
- the requester must be authenticated in AWS (cannot be anonymous)
### S3 Event Notifications
- S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication...
- Use case: generate thumbnails uploaded to s3
- Can create as many "s3 events" as desired
- S3 event notifications typically delivers events in seconds but can sometimes take a minute or longer
### S3 Event Notifications - IAM permissions
- for the s3 event notification to work, we need to have IAM permissions 
- we need to attach SNS Resource (access) policy, SQS Resource (access) policy, Lambda Resource (access) policy
### S3 Event Notifications with Amazon Event Bridge
- Advanced filtering options with Json Rules (metadata, object size, name)
- multiple destinations - ex step functions, kinises streams/firehose...
- Event bridge capabilities - archive, replay events, reliable delivry
### S3 Baseline Performance
- Amazon s3 automatically scales to high request rates, latency 100-200 ms
- your application can achieve at least 3500 PUT/POST/DELETE or 5500 GET requests per second per prefix in a bucket
- there are no limits to the number of prefixes you can have in a bucket.
- Example (object path => prefix)
  - bucket1/folder/sub1/file1 => folder/sub1
- if you spread read across all four prefixes evenly youc an achive 22,000 read request per second for GET and HEAD
### S3 Performance
- Multi Part Upload
  - recomended for files > 100mb, must use for files > 5gb
  - can help parallelize the upload and reduce time
- S3 Transfer Acceleration
  - Increase transfer speed by transferring file to an WAS edge location which will forward the data to the s3 bucket in the target region
  - compatible with multi part upload
### S3 Performance - S3 Byte range fetches
- Parallelize GETs by requesting byte ranges
- better resilience in case of failures
- can be used to speed up downloads
- can be used to retrieve only partial data (for example the head of a file)
### S3 Batch operations
- Perform bulk operations on existing s3 objects with a signle request, Example:
  - modify object metadata and properties
  - copy objects between buckets or within the same bucket
  - encrypt unencrypted objects
  - modify acls, tags
  - restore objects from s3 glacier
  - invoke lamda funcions to perform custom action on each object
- A job consists of a list of objects the action to perform and optional parameters
- s3 batch operations manages retries, tracks progress, sends completion notifications, generate reports..
- you can use s3 inventory to get objects list and use athena to query and filter your objects
### S3 - Storage Lens
- Understand analyze and optimize storage across entire aws organization
- discover anomalies, identify cost efficiencies and apply data protection best practices across entire organization (30 days usage & activity metrics)
- aggregate data for organization, specific accounts, regions, buckets or prefixes
- default dashboard or creat your own dashboard
- can be configure to export metrics daily to s3 bucket (CSV, Parquet)
### S3 - Default Dashboard
- Visualize summarized insights and trends for both free and advanced metrics
- default dashboard shows mutli region and multiaccount data preconfigure by amazon s3
- cant be deleted but can be disabled
### S3 - metrics
- Summary metrics
  - general insights about your s3 storage
  - StorageBytes, ObjectCount..
  - use cases: identify fastest growing or not used buckets and prefixes
- Cost optimization metrics 
  - provide insights to manage and optmize your storage costs
  - NonCurrentVersionStorageBytes, IncompleteMultipartUploadStorageBytes...
- Data Protection metrics
  - provide insights for data protection features
  - VersioningEnabledBucketCount, MFAdeleteEnabled BucketCount..
  - use cases: identify buckets that are not using data protection features and apply best practices
- Access management metrics
  - provide insights for s3 object ownership
  - usecases: identify which object ownership your buckets use 
- Event metrics
  - provide insights about s3 events notifications
- Performance metrics
  - provide insights about s3 transfer acceleration
- Activity metrics
  - provide insights about how your storage is requested
- Detailed Status Code Metrics
  - Provide Insights for HTTP status codes
### S3 lens Free vs Paid
- Free metrics
  - autoamtically available for all customers
  - contains around 28 usage metrics
  - data availble for queries for 14 days
- Advanced metrics and recommendations
  - additional paid metrics and features
  - advanced metrics: activity, advanced csost, optimization, advanced data protection, status code
  - Cloudwatch publishing
  - prefix aggregations
  - data is available for queries for 15 months
- the default dashboard has data across multi acc and multi regions
-
