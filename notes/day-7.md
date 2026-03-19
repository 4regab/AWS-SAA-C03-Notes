# Day 7 \- March 14

**Amazon RDS (Relational Database Service)**

- It is a managed DB service for databases that use SQL  
- It allows you to create databases in the cloud that are managed by AWS.  
- It supports: PostgreSQL, MySQL, MariaDB, Oracle, SQL Server, IBM DB2, Aurora (AWS proprietary database)  
- Advantages: automated provisioning, OS patching, continuous backups and restore to a specific timestamp (point-in-time restore), monitoring dashboards, read replicas for improved performance, Multi-AZ setup for DR (disaster recovery), scaling capability (vertical and horizontal), storage backed by EBS. But you cannot SSH to your instance

**RDS - Storage Auto Scaling** 

- Helps you increase storage on your RDS DB instance dynamically  
- When RDS detects you are running out, it scales automatically  
- Avoid manually scaling; you have to set a maximum storage threshold limit for the DB  
- Useful for unpredictable workloads  
- Supports all RDS database engines

**RDS Read Replicas for Read Scalability**

- It helps you scale your reads  
- Up to 15 read replicas  
- Within an AZ, across AZs, or across Regions  
- Replication is asynchronous, so reads are eventually consistent, and replicas can be promoted to their own databases  
- Applications must update the connection string to leverage read replicas

**Read Replica Use Cases**

- You have a production database that is taking normal load  
- You want to run a reporting application to perform analytics  
- You create a read replica to run the new workload there  
- Only used for SELECT statements  
- The production application is unaffected

**RDS Read Replicas Network Cost**

- If in the same Region but different AZs, there is no fee  
- Cross-Region replication incurs replication fees

**RDS Multi-AZ Disaster Recovery**

- Synchronous replication  
- One DNS name and automatic failover  
- Increases availability  
- Failover in case of loss of AZ, loss of network, instance, or storage failure  
- No manual intervention in applications  
- Not used for scaling  
- Read replicas can be set up as Multi-AZ for disaster recovery

**RDS from Single AZ to Multi AZ**

- Zero-downtime operation (no need to stop the DB)  
- Only need to click Modify for the database  
- The following happens internally: a snapshot is taken, a new DB is restored, synchronization is established between the two
