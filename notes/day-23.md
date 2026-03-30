# Day 23 - March 30

## Choosing the Right Database
- Questions to choose the right databases on AWS:
  - Read-heavy, write-heavy, or balanced workload? Throughput needs? Will it change? Does it need to scale or fluctuate during the day?
  - How much data to store and for how long? Will it grow? Average object size? How are they accessed?
  - Data durability? Source of truth for the data?
  - Latency requirements? Concurrent users?
  - Data model? How will you query the data? Joins? Structured? Semi-structured?
  - Strong schema? More flexibility? Reporting? Search? RDBMS/NoSQL?
  - License costs: switch to a cloud-native DB such as Aurora?

## Database Types
- RDBMS (SQL/OLTP): RDS, Aurora - great for joins
- NoSQL database: no SQL, no joins: DynamoDB (~JSON), ElastiCache (key value), Neptune (graphs), DocumentDB (MongoDB), Keyspaces for Apache Cassandra
- Object store: S3 (for big objects) / Glacier for backups and archives
- Data warehouse: S3 (for big objects) / Glacier for backups and archives
- Search: OpenSearch (JSON) - free-text, unstructured searches
- Graphs: Amazon Neptune - displays relationships between data
- Ledger: Amazon Quantum Ledger Database
- Timeseries: Amazon Timestream

### Amazon RDS - Summary
- Managed PostgreSQL/MySQL/Oracle/SQL Server/DB2/MariaDB/custom
- Provisioned RDS instance size and EBS volume type and size
- Auto scaling capability for storage
- Support for read replicas and Multi-AZ
- Security through IAM, security groups, KMS, SSL in transit
- Automated backup with point-in-time restore feature (up to 35 days)
- Manual DB snapshot for longer-term recovery
- Managed and scheduled maintenance (with downtime)
- Support for IAM authentication, integration with Secrets Manager
- RDS Custom for access to and customization of the underlying instance (Oracle and SQL Server)
- Use case: store relational datasets (RDBMS/OLTP), perform SQL queries and transactions

## Amazon Aurora Summary
- Compatible API for PostgreSQL and MySQL, separation of storage and compute
- Storage: data is stored in 6 replicas across 3 AZs - highly available, self-healing, and auto scaling
- Compute: cluster of DB instances across multiple AZs, auto scaling of read replicas
- Cluster: custom endpoints for writer and reader DB instances
- Same security, monitoring, and maintenance features as RDS
- Know the backup and restore options for Aurora
- Aurora Serverless - for unpredictable or intermittent workloads, no capacity planning
- Aurora Global: up to 16 DB read instances in each region, < 1 second storage replication
- Aurora Machine Learning: perform ML using SageMaker and Comprehend on Aurora
- Aurora Database Cloning: new cluster from an existing one, faster than restoring a snapshot
- Use case: same as RDS but with less maintenance, more flexibility, more performance, and more features

## Amazon ElastiCache Summary
- Managed Redis/Memcached in-memory data store (same offering as RDS but for caches)
- In-memory data store, sub-millisecond latency
- Select an ElastiCache type (e.g. cache.m6g.large)
- Support for clustering (Redis) and Multi-AZ read replicas (sharding)
- Security through IAM, security groups, KMS, Redis auth
- Backup/snapshot/point-in-time restore feature
- Managed and scheduled maintenance
- Require some application code changes to be leveraged
- Use case: key/value store, frequent reads, fewer writes, cache results for DB queries, store session data for websites, cannot use SQL

## Amazon DynamoDB Summary
- AWS proprietary technology, managed serverless NoSQL database, millisecond latency
- Capacity modes: provisioned capacity with optional autoscaling or on-demand capacity
- Can replace ElastiCache as a key/value store (storing session data, for example, using the TTL feature)
- Highly available, Multi-AZ by default, reads and writes are decoupled, transaction capability
- DAX cluster for read cache, microsecond read latency
- Security, authentication, and authorization are done through IAM
- Event processing: DynamoDB Streams to integrate with AWS Lambda or Kinesis Data Streams
- Global table feature: active-active setup
- Automated backups up to 35 days with PITR (restore to new table) or on-demand backups
- Export to S3 without using RCU within the PITR window, import from S3 without using WCU
- Great for rapidly evolving schemas
- Use case: serverless application development (small documents, 100s KB), distributed serverless cache

## Amazon S3 - Summary
- S3 is a key/value store for objects
- Great for bigger objects, not so great for many small objects
- Serverless, scales infinitely, max object size is 5 TB, versioning capability
- Tiers: S3 Standard, S3 Infrequent Access, S3 Intelligent-Tiering, S3 Glacier + lifecycle policy
- Features: versioning, encryption, replication, MFA-delete, access logs
- Security: IAM, bucket policies, ACLs, access points, Object Lambda, CORS
- Encryption: SSE-S3, SSE-KMS, SSE-C, client-side, TLS in transit, default encryption
- Batch operations on objects using S3 Batch; listing files using S3 Inventory
- Performance: multipart upload, S3 Transfer Acceleration, S3 Select
- Automation: S3 event notifications (SNS, SQS, Lambda, EventBridge)
- Use cases: static files, key/value store for big files, website hosting

## DocumentDB
- Aurora is an AWS implementation of PostgreSQL/MySQL
- DocumentDB is the same for MongoDB (which is a NoSQL database)
- MongoDB is used to store, query, and index JSON data
- Similar "deployment concepts" as Aurora
- Fully managed, highly available with replication across 3 AZs
- DocumentDB storage automatically grows in increments of 10 GB
- Auto scales to workloads with millions of requests per second

## Amazon Neptune
- Fully managed graph database
- A popular graph dataset would be a social network
  - Users have friends
  - Posts have comments
  - Comments have likes from users
  - Users share and like posts
- Highly available across 3 AZs with up to 15 read replicas
- Build and run applications working with highly connected datasets, optimized for these complex and hard queries
- Can store up to billions of relations and query the graph with ms latency
- Highly available with replication across multiple AZs
- Great for knowledge graphs (Wikipedia), fraud detection, recommendation engines, and social networking

## Amazon Neptune Streams
- Real-time ordered sequence of every change to your graph data
- Changes are available immediately after writing
- No duplicates, strict order
- Stream data is accessible in an HTTP REST API
- Use cases:
  - Send notifications when certain changes are made
  - Maintain your graph data synchronized in another data store (e.g. S3, OpenSearch, ElastiCache)

## Amazon Keyspaces for Apache Cassandra
- Apache Cassandra is an open-source NoSQL distributed database
- A managed Apache Cassandra-compatible database service
- Serverless, scalable, highly available, fully managed by AWS
- Automatically scales tables up/down based on the application's traffic
- Tables are replicated 3 times across Multi-AZ
- Uses the Cassandra Query Language (CQL)
- Single-digit millisecond latency at any scale, 1000s of requests per second
- Capacity on demand or provisioned mode with auto scaling
- Encryption, backup, point-in-time recovery up to 35 days
- Use cases: store IoT device info, time-series data

## Amazon Timestream
- Fully managed, fast, scalable, serverless time-series database
- Automatically scales up and down to adjust capacity
- Store and analyze trillions of events per day
- 1,000x faster and 1/10th of the cost of relational databases
- Scheduled queries, multi-measure records, SQL compatibility
- Data storage tiering: recent data kept in memory and historical data kept in cost-optimized storage
- Built-in time-series analytics functions (helps identify patterns in your data in near real time)
- Encryption in transit and at rest
- Use cases: IoT apps, operational applications, real-time analytics
