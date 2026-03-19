# Day 4 \- March 11

**EBS Volume**

- Elastic Block Store (EBS) is a **network drive** you can attach to an instance.  
- It allows your data to persist even after termination.  
- Can only be mounted to one instance at a time (CCP level)  
- They are bound to a specific Availability Zone (AZ)  
- Can be detached from one instance and attached to another quickly  
- Locked to an AZ; to move a volume across AZs, you need to snapshot it  
- Supports the delete-on-termination attribute

**EBS Snapshots**

- Make a backup of your EBS volume at a point in time  
- It is not necessary to detach the volume to take a snapshot, but it is recommended  
- Can copy snapshots across AZs or Regions

**Features of EBS snapshots:** 

- Snapshot archive \- move the snapshot to the "archive tier" that is 75% cheaper. It takes 24-72 hours to restore from the archive  
- Recycle bin \- set up rules to retain deleted snapshots so you can recover them after an accidental deletion (specify retention from 1 day to 1 year)  
- Fast snapshot restore \- force full initialization of a snapshot to have no latency on the first use

**AMI (Amazon Machine Image)**

- AMIs are customizations of an EC2 instance. You can add your own software, config, OS, monitoring, etc.  
- Faster boot/config time because all your software is prepackaged  
- AMIs are built for a specific Region and can be copied across Regions  
- You can use your own OS or the AWS-provided AMI

**EC2 Instance Store**

- EBS has limited performance. If you need high-performance hardware disks, use EC2 Instance Store for better I/O performance and great throughput.  
- If you stop the instance, its storage will be lost (ephemeral)  
- Good for buffer/cache/scratch data/temporary content  
- Risk of data loss if hardware fails  
- Provides the best disk I/O performance. Good if you do not mind losing cache upon termination of the instance  
- Backup and replication are your responsibility

**EBS Volume Types**

- gp2/gp3 (SSD): general purpose and balanced price and performance for a wide variety of workloads. Cost-effective storage and low latency. 1 GB to 16 TB. Max IOPS is 16k  
- io1/io2 Block Express (SSD): highest-performance SSD for mission-critical, low-latency or high-throughput workloads. Great for database workloads. For applications that need more than 16k IOPS. Supports EBS Multi-Attach  
- st1 (HDD): low-cost HDD volume for frequently accessed, throughput-intensive workloads. Throughput optimized HDD, big data, data warehouse, log processing, max throughput 500 MB/s, max IOPS 500  
- sc1 (HDD): lowest-cost HDD volume for less frequently accessed workloads. Cold HDD. Max throughput 250 MB/s, max IOPS 250  
- Only gp2/gp3 and io1/io2 Block Express can be used as boot volumes

**EBS Multi Attach io/io2 family**

- Lets you attach the same EBS volume to multiple EC2 instances in the same AZ  
- Each instance has full read and write permissions to the high-performance volume. Use case: higher application availability in clustered Linux applications, e.g. Teradata  
- Up to 16 EC2 instances at a time. Must use a file system that is cluster-aware (not xfs, ext4, etc.)

**EBS Encryption**

- Data at rest is encrypted inside the volume  
- All data in flight between the instance and the volume is encrypted. All snapshots are encrypted  
- All volumes created from a snapshot are encrypted  
- Encryption and decryption are handled transparently (you do not need to do anything)  
- Has a minimal impact on latency  
- EBS encryption leverages keys from KMS (AES-256)  
- Copying an encrypted snapshot preserves encryption  
- Snapshots of encrypted volumes are encrypted

**EFS (Elastic File System)**

- Managed NFS (network file system) that can be mounted on many EC2 instances  
- EFS works with EC2 instances in multiple AZs  
- Highly available, scalable, expensive (3x gp2), pay per use  
- EC2 instances in different AZs can connect to the same network file system through EFS  
- Use case: content management, web serving, data sharing, WordPress  
- Uses the NFSv4.1 protocol  
- Uses security groups to control access to EFS  
- It is only compatible with Linux-based AMIs  
- You can enable encryption at rest in your EFS drive using KMS  
- File system scales automatically (pay per use)  
- Mounting 100s of instances across AZs

**EFS Performance & Storage Classes**

- EFS scale: grow to petabyte-scale network file system automatically  
- Performance mode (set at EFS creation time): general purpose (default) for latency-sensitive use cases, MAX I/O for higher latency, throughput, highly parallel, big data  
- Throughput mode: bursting 1 TB = 50 MB/s + burst up to 100 MB/s, provisioned set your throughput regardless of storage size, elastic automatically scales throughput up or down based on your workloads

**EFS Storage Classes**

- Storage tiers: standard for frequent access files. Infrequent access: lower cost to store, higher cost to retrieve. Archive: rarely accessed data (few times each year), 50% cheaper  
- Availability standard: multi-AZ. One Zone: one AZ  
- By using the right EFS storage classes, you can achieve up to 90% in cost savings
