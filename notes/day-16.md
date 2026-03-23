# Day 16 \- March 23

## AWS Snowball
- highly secure, portable devices to collect and process data at the edge and migrate data into and out of AWS
- helps migrate up to Petabytes of data to AWS
- Two types of devices: Snowball Edge Storage Optimized 210 TB and Snowball Edge Compute Optimized 28 TB
- use case: limited network connectivity, large data transfer, edge computing, and data collection in remote locations
## Edge computing
- Process data while it's being created on an edge location
  - A truck on the road, a ship at sea, or a mining station underground
- these locations may have limnited internet an no access to computing power
- we setup a snowball edge device to do the computing
  - snowball edge compute optimized (dedicated for that use case) & storage optimized
  - run ec2 instances or lambda functions at the edge
- use cases: prepprocess data, machine learning and transcoding media
## Solution Architecture: Snowball into glacier
- Snowball cannot import to glacier directly
- You must ues amazon s3 first in combinaion with an s3 life policy
## Amazon FSx - Overview
- Launch 3rd party high performance file systems on AWS
- fully managed service
- Four types: FSx for Lustre, FSx for Windows File Server, FSx for NetApp ONTAP, FSx for OpenZFS
## Amazon FSx for windows (file server)
- Fsx for windows is a fully managed  windows file system share drive
- Supports SMB protocol and windows NTFS file system
- microsoft active directory integration, ACL, and user quotas
- Can be mounted on linux ec2 instances
- supports microsoft's distributed file system (DFS) namespaces (group file across multiple FS)
- scale up to 10s of Gb/s millions of IOPS, 100s PB of data
- Storage options: SSD and HDD
- Can be access from your on premises infrastracture (VPn or Direct Connect)
- Can be configured to be Multi-AZ for high availability
- Data is backed up daily to S3
## Amazon FSx for Lustre
- Lustre is a type of parallel distributed file system for large scale computing
- the name lustre derived from linux and cluster
- machine learning, high performance computing, video processing, financial modeling, and electronic design automation
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
- storage options: SSD and HDD
- seamless integration with S3: can read s3 as file system (through FSx)
- Can be usedde from on premises servers (VPN or Direct Connect)
## FSx File system deployment options
- Scratch File System 
  - Temporary storage
  - Data is not replicated 
  - High Burst
  - Usage: short term processing, optimize costs
- Persistent File System
  - Long term storage
  - data is replicated within same AZ
  - replace failed files within minutes
  - Usage: long term processing, sensitive data
## Amazon FSx for NetApp ONTAP
- Managed netapp ontap on AWS
- file system compatible with NFS, SMB, iSCSI protocol
- move worlloads riunning on ONtap or NAS to aws
  - works with: linux, windows, macos, vm ware cloud on aws, amazon workspaces & appstream, amazon ec2, eks, ecs
- Storage shirnks or grow automatically
- snapshots replication lowcost compression and data deduplication
- point in time instantaneous cloning (helpful for testing new workloads)
## Amazon FSx for OPENZFS
- managed OpenZFS file system on AWS
- file system compatible with NFS
- move workloads running on ZFS to aws
  - works with: linux, windows, macos, vm ware cloud on aws, amazon workspaces & appstream, amazon ec2, eks, ecs
- up to 1M IOPS with < 0.5 ms latency
- snapshots compression and low cost
- point in time instantaneous cloning (helpful for testing new workloads)
## hybrid cloud for storage
- AWS is pushing for "hybrid cloud"
  - part of your infrastructure is on premises and part of it is on CLOUD
- this can be due to: long cloud migrations, security requirements, compliance requirements, IT strategy
- S3 is a propretary storage technology (unlike EFS/NFS) so how do you expose the s3 data on premises?
  - AWS Storage Gateway!
## AWS Storage Gateway overview
- bridge between onpremises data and cloud data
- usecases: disaster recovery, backup and restore, tiered storage, on premises cache & low latency files access
- types of storage gateway: S3 file gateway, volume gateway, tape gateway
## AWS Storage File Gateway
- configured s3 buckets are accessible using NFS and SMB protocol 
- most recently used data is cached in the file gateway
- supports s3 standard, s3 standard IA, s3 one zone A, S3 intelligent tiering
- transition to s3 glacier using a lifecycle policy
- bucket access using IAM roles for each file gateway
- SMB protocol has integration with active directory (AD) for user authentication
## Volume Gateway
- Block storage using iSCSI protocol backed by S3
- backed by ebs snapshots which can help restore on premises volumes
- cached volumes: low latency access to most recent data
- stored volumes: entire datasets is on premise, scheduled backups to s3
## Tape Gateway
- Some companies have backup processes using physical tapes 
- with tape gateway, companies use the same processes but in the cloud
- virtual tape library (VTL) backed by amazon S3 and glacier
- back up data using existing tape-based processes (and iSCSI interface)
- works with leading backup software vendors
## AWS Transfer Family
- A fully managed service for file transfers into and out of amazon s3 or amazon efs using the ftp protocol
- Supported protocols: AWS transfer for FTP, FTPS, SFTP
- managed infrastracture, scalable, reliable, highly available (multi az)
- pay per provisioned end point per hour + data trasnfers in GB
- Store and manage users' credentials within the service
- integrate with existing authentication systems (microsoft active directory, LDAP, okta, amazon cognito, custom)
- usage: sharing file, public datasets, CRM, ERP
## AWS Datasync
- move large amount of data to and from places
  - ON premises/ other cloud to AWS (NFS, SMB, HDFS, S3 API...) needs agent
  - AWS to AWS -- no agent needed
- Can synchronize to:
  - Amazon s3 (any storage classes - including glacier)
  - Amazon efs
  - amazon FSx (windows, lustre, netapp, openfs)
- replication tasks can be scheduled hourly, daily, weekly
- file permissions and metadata are preserved (nfs, posix, smb)
- one agent task can use 10gbps can setup a bandwith limit
## Storage Comparison
- S3: object storage
- S3 Glacier: object archival
- EBS volumes: network storage for one ec2 instance at a time
- Instance Storage: physical storage for your ec2 instance (high IOPS)
- EFS: network file system for linux instances, POSIX filesystem
- FSx for windows: network file system for windows servers
- Fsx for lustre: high performance computing linux file system
- Fsx for netapp ontap: high os compatibility
- FSX for OpenZFS: managed ZFS file system
- storage gateway: s3 & fsx file gateway, volume gateway (cached & stored), tape gateway
- transfer family: FTP, FTPs, SFTP interance on top of amazon s3 or amazon efs
- Datasync: schedule datasync form on premises to aws or aws to aws'
- snowcone/snowball/snowmobile: to move large amount of data to the cloud physically
- database: for specific workloads usually with indexing and querying
