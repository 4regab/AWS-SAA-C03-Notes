# Day 16 \- March 23

## AWS Snowball
- Highly secure, portable devices to collect and process data at the edge and migrate data into and out of AWS
- Helps migrate up to petabytes of data to AWS
- Two types of devices: Snowball Edge Storage Optimized 210 TB and Snowball Edge Compute Optimized 28 TB
- Use case: limited network connectivity, large data transfer, edge computing, and data collection in remote locations
## Edge computing
- Process data while it is being created at an edge location
  - A truck on the road, a ship at sea, or a mining station underground
- These locations may have limited internet and no access to computing power
- We set up a Snowball Edge device to do the computing
  - Snowball Edge Compute Optimized (dedicated for that use case) and Storage Optimized
  - Run EC2 instances or Lambda functions at the edge
- Use cases: preprocess data, machine learning, and transcoding media
## Solution Architecture: Snowball into Glacier
- Snowball cannot import to Glacier directly
- You must use Amazon S3 first in combination with an S3 lifecycle policy
## Amazon FSx - Overview
- Launch third-party high-performance file systems on AWS
- Fully managed service
- Four types: FSx for Lustre, FSx for Windows File Server, FSx for NetApp ONTAP, FSx for OpenZFS
## Amazon FSx for Windows (File Server)
- FSx for Windows is a fully managed Windows file system share drive
- Supports SMB protocol and Windows NTFS file system
- Microsoft Active Directory integration, ACLs, and user quotas
- Can be mounted on Linux EC2 instances
- Supports Microsoft's Distributed File System (DFS) namespaces (group files across multiple file systems)
- Scales up to tens of Gb/s, millions of IOPS, and hundreds of PB of data
- Storage options: SSD and HDD
- Can be accessed from your on-premises infrastructure (VPN or Direct Connect)
- Can be configured to be Multi-AZ for high availability
- Data is backed up daily to S3
## Amazon FSx for Lustre
- Lustre is a type of parallel distributed file system for large-scale computing
- The name Lustre is derived from Linux and cluster
- Machine learning, high-performance computing, video processing, financial modeling, and electronic design automation
- Scales up to hundreds of GB/s, millions of IOPS, and sub-ms latencies
- Storage options: SSD and HDD
- Seamless integration with S3: can read S3 as a file system through FSx
- Can be used from on-premises servers (VPN or Direct Connect)
## FSx File System Deployment Options
- Scratch File System
  - Temporary storage
  - Data is not replicated
  - High burst
  - Usage: short-term processing, cost optimization
- Persistent File System
  - Long-term storage
  - Data is replicated within the same AZ
  - Replace failed files within minutes
  - Usage: long-term processing, sensitive data
## Amazon FSx for NetApp ONTAP
- Managed NetApp ONTAP on AWS
- File system compatible with NFS, SMB, and iSCSI protocols
- Move workloads running on ONTAP or NAS to AWS
  - Works with Linux, Windows, macOS, VMware Cloud on AWS, Amazon WorkSpaces and AppStream, Amazon EC2, EKS, and ECS
- Storage shrinks or grows automatically
- Snapshots, replication, low-cost compression, and data deduplication
- Point-in-time instantaneous cloning (helpful for testing new workloads)
## Amazon FSx for OpenZFS
- Managed OpenZFS file system on AWS
- File system compatible with NFS
- Move workloads running on ZFS to AWS
  - Works with Linux, Windows, macOS, VMware Cloud on AWS, Amazon WorkSpaces and AppStream, Amazon EC2, EKS, and ECS
- Up to 1M IOPS with < 0.5 ms latency
- Snapshots, compression, and low cost
- Point-in-time instantaneous cloning (helpful for testing new workloads)
## Hybrid Cloud for Storage
- AWS is pushing for "hybrid cloud"
  - Part of your infrastructure is on premises and part of it is in the cloud
- This can be due to long cloud migrations, security requirements, compliance requirements, or IT strategy
- S3 is a proprietary storage technology (unlike EFS/NFS), so how do you expose the S3 data on premises?
  - AWS Storage Gateway
## AWS Storage Gateway Overview
- Bridge between on-premises data and cloud data
- Use cases: disaster recovery, backup and restore, tiered storage, on-premises cache, and low-latency file access
- Types of Storage Gateway: S3 File Gateway, Volume Gateway, Tape Gateway
## AWS Storage File Gateway
- Configured S3 buckets are accessible using the NFS and SMB protocols
- Most recently used data is cached in the file gateway
- Supports S3 Standard, S3 Standard-IA, S3 One Zone-IA, and S3 Intelligent-Tiering
- Transition to S3 Glacier using a lifecycle policy
- Bucket access using IAM roles for each file gateway
- SMB protocol has integration with Active Directory (AD) for user authentication
## Volume Gateway
- Block storage using iSCSI protocol backed by S3
- Backed by EBS snapshots, which can help restore on-premises volumes
- Cached volumes: low-latency access to the most recent data
- Stored volumes: the entire dataset is on premises, with scheduled backups to S3
## Tape Gateway
- Some companies have backup processes using physical tapes
- With Tape Gateway, companies use the same processes but in the cloud
- Virtual Tape Library (VTL) backed by Amazon S3 and Glacier
- Back up data using existing tape-based processes and the iSCSI interface
- Works with leading backup software vendors
## AWS Transfer Family
- A fully managed service for file transfers into and out of Amazon S3 or Amazon EFS using the FTP protocol
- Supported protocols: AWS Transfer for FTP, FTPS, SFTP
- Managed infrastructure, scalable, reliable, and highly available (Multi-AZ)
- Pay per provisioned endpoint per hour plus data transfers in GB
- Store and manage users' credentials within the service
- Integrates with existing authentication systems (Microsoft Active Directory, LDAP, Okta, Amazon Cognito, custom)
- Use cases: file sharing, public datasets, CRM, ERP
## AWS DataSync
- Move large amounts of data to and from places
  - On premises/other cloud to AWS (NFS, SMB, HDFS, S3 API...) needs agent
  - AWS to AWS -- no agent needed
- Can synchronize to:
  - Amazon S3 (any storage class, including Glacier)
  - Amazon EFS
  - Amazon FSx (Windows, Lustre, NetApp, OpenZFS)
- Replication tasks can be scheduled hourly, daily, or weekly
- File permissions and metadata are preserved (NFS, POSIX, SMB)
- One agent task can use 10 Gbps and can set a bandwidth limit
## Storage Comparison
- S3: object storage
- S3 Glacier: object archival
- EBS volumes: network storage for one EC2 instance at a time
- Instance Storage: physical storage for your EC2 instance (high IOPS)
- EFS: network file system for Linux instances, POSIX filesystem
- FSx for Windows: network file system for Windows servers
- FSx for Lustre: high-performance computing Linux file system
- FSx for NetApp ONTAP: high OS compatibility
- FSx for OpenZFS: managed ZFS file system
- Storage Gateway: S3 and FSx File Gateway, Volume Gateway (cached and stored), Tape Gateway
- Transfer Family: FTP, FTPS, SFTP interface on top of Amazon S3 or Amazon EFS
- DataSync: schedule DataSync from on premises to AWS or AWS to AWS
- Snowcone/Snowball/Snowmobile: to move large amounts of data to the cloud physically
- Database: for specific workloads, usually with indexing and querying
