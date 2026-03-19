# Day 8 \- March 15

**RDS Custom** 

- Manage Oracle and Microsoft SQL Server databases with OS and database customization  
- RDS automates setup, operation, and scaling of databases in AWS  
- Custom: access to the underlying database and OS so you can configure settings and install patches, and use SSH or SSM Session Manager to access the underlying EC2 instance  
- Deactivate automation mode to perform your customization; it is better to take a DB snapshot first  
- RDS vs. RDS Custom:   
  - RDS: the entire database and the OS are managed by AWS  
  - RDS Custom: full admin access to the underlying OS and the database

**Amazon Aurora** 

- A proprietary technology from AWS (not open source)  
- PostgreSQL and MySQL are both supported as Aurora DBs (meaning your drivers will work as if Aurora were a PostgreSQL or MySQL database)  
- Aurora is AWS cloud optimized and claims 5x performance improvement over MySQL on RDS and over 3x the performance of PostgreSQL on RDS  
- Aurora storage automatically grows in increments of 10 GB up to 256 TB  
- Aurora can have up to 15 replicas, and the replication process is faster than MySQL   
- Failover in Aurora is instantaneous. It is HA native  
- Aurora costs about 20% more than RDS, but it is more efficient

**Aurora High Availability and Read Scaling**

- It stores 6 copies of your data across 3 AZs  
- Self-healing with peer-to-peer replication  
- Storage is striped across hundreds of volumes  
- Automated failover for the master in less than 30 seconds  
- Master plus up to 15 Aurora read replicas serve reads  
- Supports cross-region replication

**Aurora DB Cluster**

- Writer endpoint (pointing to the master)  
- Read replicas can have auto scaling  
- Shared storage volume, from 10 GB to 256 TB  
- **Reader Endpoint (connection load balancing): same exact features, but it helps with connection load balancing and automatically connects all the read replicas. So every time a client connects to the reader endpoint it will connect to one of the read replicas and load balancing will occur**

**Features of Aurora**

- Automatic failover  
- Backup and recovery  
- Isolation and security  
- Industry compliance  
- Push-button scaling  
- Automated patching with zero downtime  
- Advanced monitoring

**Aurora Replicas - Auto Scaling**

- Auto scaling adds read replicas, and the reader endpoint is extended to cover these new replicas, bringing down the overall CPU usage

**Aurora - Custom Endpoints**

- Define a subset of Aurora instances as a custom endpoint  
- Example: run analytical queries on specific replicas

**Aurora Serverless**

- Automated database instantiation and auto scaling based on actual usage  
- Good for infrequent, intermittent, unpredictable workloads  
- No capacity planning needed  
- Pay per second can be more cost effective

**Global Aurora**

- Aurora cross-region read replica: useful for disaster recovery, simple to put in place  
- Aurora global database (recommended):  
- 1 primary Region (read/write)  
- Up to 10 secondary (read-only) Regions, replication lag is less than 1 second  
- Up to 16 read replicas per secondary Region  
- Promoting another Region for disaster recovery has an RTO of < 1 minute  
- **Typical cross-region replication takes less than 1 second**

**Aurora Machine Learning**

- Enables you to add ML-based predictions to your application via SQL  
- Simple, optimized, and secure integration between Aurora and AWS ML services  
- Supported services: Amazon SageMaker, Amazon Comprehend  
- You do not need to have ML experience  
- Use cases: fraud detection, ad targeting, sentiment analysis, product recommendations

**Babelfish for Aurora PostgreSQL**

- Allows Aurora PostgreSQL to understand commands targeted for MS SQL Server (e.g. T-SQL)  
- Therefore, MSSQL-based applications can work on Aurora PostgreSQL  
- Requires little to no code changes  
- The same application can be used after a migration of your database (using AWS SCT or DMS)

**RDS Backups**

- Automated backups  
  - Transaction logs are backed up by RDS every 5 minutes  
  - Daily full backup of database (during the backup window)  
  - Ability to restore any point in time from oldest backup to 5 minutes ago  
  - 1 to 35 days of retention, set to 0 to disable  
- Manual DB snapshots  
- Trick: in a stopped RDS database, you will still pay for storage. If you plan on stopping it for a long time, you should snapshot and restore instead

**Aurora Backups**

- Automated backups cannot be disabled, supports point-in-time recovery  
- Manual DB snapshot, retain backups for as long as you want

**RDS & Aurora Restore Options**

- Restoring an RDS/Aurora backup or a snapshot creates a new database  
- Restoring a MySQL RDS database from S3  
  - Create a backup of your on-premises database  
  - Store it on Amazon S3 object storage  
  - Restore the backup file onto a new RDS instance running MySQL  
- Restoring a MySQL RDS cluster from S3  
  - Create a backup of your on-premises database using Percona XtraBackup  
  - Store it on Amazon S3 object storage  
  - Restore the backup file onto a new Aurora cluster running MySQL

**Aurora Database Cloning**

- Create a new Aurora DB cluster from an existing one  
- Faster than snapshot and restore  
- Very fast and cost effective  
- Uses copy-on-write protocol  
  - Initially the new DB cluster uses the same data volume as the original DB cluster (fast and efficient)  
  - When updates are made to the new DB cluster data, then additional storage is allocated and data is copied to be separated  
- Useful to create a "staging" database from a "production" database without impacting the production database

**RDS & Aurora Security**

- At-rest encryption: database master and read replicas encryption using AWS KMS  
- In-flight encryption: TLS ready by default, use the AWS TLS root certs client-side  
- IAM Authentication: IAM roles to connect to DB  
- Security Groups: control network access  
- No SSH available; except on RDS Custom  
- Audit logs can be enabled and sent to CloudWatch

**Amazon RDS Proxy**

- Fully managed proxy for RDS  
- Allows apps/Lambda functions to pool and share DB connections with the database  
- Improves database efficiency by reducing the stress on database resources (e.g. CPU, RAM) and minimizes open connections  
- Serverless, autoscaling, highly available  
- Reduces RDS and Aurora failover time by up to 66%  
- Supports RDS and Aurora  
- No code changes required  
- Enforces IAM Authentication for DB and securely stores credentials in AWS Secrets Manager  
- RDS Proxy is never publicly accessible

**Amazon ElastiCache Overview**

- The same way RDS provides managed relational databases, ElastiCache provides managed Redis or Memcached  
- Caches are in-memory databases with really high performance and low latency  
- Helps reduce load on databases for read-intensive workloads  
- Helps your application stay stateless  
- AWS takes care of OS maintenance/patching, optimizations, setup and config, monitoring, failure recovery, and backups  
- Using ElastiCache involves heavy code changes

**ElastiCache Solutions Architecture - DB Cache**

- Application queries ElastiCache; if not available, get from RDS and store in ElastiCache   
- Helps relieve load on RDS  
- Cache must have an invalidation strategy to make sure only the most current data is used

**ElastiCache Solutions Architecture - User Session Store**

- Users log into any of the application   
- The application writes the session data into ElastiCache  
- The user hits another instance of our application  
- The instance retrieves the data and the user is already logged in

**ElastiCache - Redis vs. Memcached**

- **Redis**: Multi-AZ with auto-failover, read replicas to scale reads and high availability, data durability using AOF persistence, backup and restore features, supports sets and sorted sets  
- **Memcached**: multi-node for partitioning and data sharding, no high availability, non-persistent, backup and restore serverless, multi-thread architecture

**ElastiCache - Cache Security**

- **ElastiCache** supports IAM Authentication for Redis  
- IAM policies on ElastiCache are only used for AWS API-level security  
- Redis auth: you can set a password token when you create a Redis cluster; this is an extra level of security for your cache. Supports SSL in-flight encryption   
- Memcached: supports SASL-based authentication

**ElastiCache Patterns**

- **Lazy load**   
- **Write through**   
- **Session store**

**ElastiCache - Redis Use Case**

- **Gaming Leaderboard**  
- **Redis sorted sets guarantee both uniqueness and element ordering**  
- **Each time a new element is added, it is ranked in real time and then added in the correct order**

**List of Ports to be familiar with**  
A list of **standard** ports you should see at least once. You should be able to differentiate between an important port (HTTPS - port 443) and a database port (PostgreSQL - port 5432). 

**Important ports:**

* FTP: 21  
* SSH: 22  
* SFTP: 22 (same as SSH)  
* HTTP: 80  
* HTTPS: 443

**RDS database ports:**

* PostgreSQL: 5432  
* MySQL: 3306  
* Oracle RDS: 1521  
* MSSQL Server: 1433  
* MariaDB: 3306 (same as MySQL)  
* Aurora: 5432 (if PostgreSQL compatible) or 3306 (if MySQL compatible)
