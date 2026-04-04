# Day 28 - April 4

## Understanding CIDR - IPv4
- A CIDR consist of two components
- Base IP
  - represents an IP contained in range (xx.xx.xx.xx)
  - example: 10.0.0.0, 192.168.0.0,..
- Subnet Mask
  - defines how many bits can change in the IP
  - example: /0, /24, /32
  - Can take two forms:
    - /8 <> 255.0.0.0
    - /16 <> 255.255.0.0
    - /24 <> 255.255.255.0
    - /32 <> 255.255.255.255

## Understanding CIDR - Subnet Mas
- The subnet mask basically allows part of underlying IP to get additional next values from the BASE IP
- 192.168.0.0/32 <> allows for 1 IP (2^0)
- 192.168.0.0/28 <> allows for 16 IPs (2^4)
- 192.168.0.0/26 <> allows for 64 IPs (2^6)
- 192.168.0.0/25 <> allows for 128 IPs (2^7)
- 192.168.0.0/24 <> allows for 256 IPs (2^8)
- 192.168.0.0/16 <> allows for 65536 IPs (2^16)
- 192.168.0.0/8 <> allows for 16777216 IPs (2^24)

## Public vs Private IP (IPv4)
- The internet assigned numbers (IANA) established certain of blocks of IPv4 addresses for the ues of private (LAN) and public (internet) addresses
- Private IP can only allow certain values:
  - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8) - in big networks
  - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12) - AWS default VPC in that range
  - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16) - home networks
- All the rest of the IP address on the internet are public

## Default VPC walkthrough
- All new accounts have a default VPC
- New EC2 instances are launched into the default VPC if no subnet is specified
- Default VPC have internet connectivity and all EC2 instances inside it have public and private IPv4 addresses
- We also get a public and private DNS names
- We also get a network interface with a private IPv4 address from the range of the VPC

## VPC in AWS - IPv4
- VPC = Virtual Private Cloud
- you can have multiple VPCs in an AWS region (max. 5 pe region soft limit)
- MAX CIDR per VPC is 5 for each CIDR:
  - Min size is /28 (16 IP addresses)
  - Max size is /16 (65536 IP addresses)
- Because VPC is private, only the private IPv4 ranges are allowed:
  - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
  - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
  - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)
- Your VPC CIDR should NOT overlap with your other networks (ex: corporate)

## VPC - Subnet (IPv4)
- AWS reserves 5 IP addresses (first 4 and last 1) in every subnet
- These 5 IP addresses are not available for use and cannot be assigned to an EC2 instance
- Example: if CIDR block 10.0.0.0/24, then reserved IP addresses are:
  - 10.0.0.0 - Network Address
  - 10.0.0.1 - reserved by AWS for the VPC router
  - 10.0.0.2 - reserved by AWS for mapping to Amazon provided DNS
  - 10.0.0.3 - reserved by AWS for future use
  - 10.0.0.255 - Network Broadcast Address. AWS does not support broadcast in a VPC, therefore the address is reserve
- Tip: if you need 29 ip addresses for ec2 instances:
  -  you cant choose a subnet size of /27 (32 IP addresses, 32 - 5 = 27 < 29)
  - you need to choose a subnet of size /26 (64 IP Addresses, 64 - 5 = 59 > 29)
- Start with 10.0.0.0/24 because in a public subnet you wanna choose not too many IP addresses. Usually, this is reserved for your load balancer so the smaller size is good

## Internet Gateway (IGW)
- allows resources (e.g. EC2 instances) in a VPC connect to the internet
- it scales horizontally and is highly available and redundant
- must be created separately from a VPC
- one VPC can only be attached to one IGW and vice versa
- Internet Gateway on their own do not allow internet access, route tables must also be edited

## Bastion Hosts
- we can use a bastion host to SSH into our private EC2 instances
- the bastion is in the public subnet which is then connected to all other private subnets
- bastion host security group must allow inbound from the internet on port 22 from restricted CIRD for example the public CIDR of your corporation
- Security group of ec2 instances must allow the security group of the bastion host or the private IP of the bastion host

## NAT Instance ( Network address translation )
- allows EC2 instances in private subnets to connect to the internet
- must be launched in a public subnet
- must disable EC2 setting: Source/Destination check
- must have an elastic IP
- route tables must be configured to route traffic from private subnets to the NAT instance

## NAT Gateway
- AWS managed NAT, higher bandwidth, high availability, no administration
- pay per hour for usage and bandwidth
- NAT gateway is created in a specific availability
- Can't be used by EC2 instance in the same subnet (only from other subnets)
- requires an IGW (private subnet in same route table must point to this)
- 5Gbps of bandwidth with automatic scaling up to 45Gbps
- no security groups to manage / high availability within AZ

## NAT Gateway with high availability
- NAT gateway is resilient within a single AZ
- must create multiple NAT gateways in multiple AZs for fault tolerance
- do this by creating a NAT gateway in each AZ and update the route tables (one for each AZ)

## NAT Instance vs NAT Gateway
- NAT Gateway 
  - managed by AWS
  - NAT Gateway is highly available within a single AZ
  - NAT Gateway bandwidth up to 100gbs
  - No need to patch
  - No need to manage
  - more expensive
- NAT instance
  - use a script too manage failover between instances
  - bandwidth depends on instance type
  - you can choose the instance type
  - you must manage it
  - we can run a script to manage failover between instances
  - cheaper

## NACL (Network Access Control List)
- NACL are like a firewall which control traffic from and to subnets
- one NACL per subnet, new subnets are assigned the default NACL
- You define NACL Rules:
  - Rules have a number (1-32766), higher precedence with a lower number
  - First rule match will drive the decision
  - Example: if you define #100 ALLOW 10.0.0.10/32 and #200 DENY 10.0.0.10/32, the IP address
  - will be allowed because 100 has a higher precedence over 200
  - The last rule is an asterisk (*) and denies a request in case of no rule match
  - AWS recommends adding rules by increment of 100
- Newly created NACLs will deny everything
- NACL are a great way of blocking a specific IP at the subnet level
- Default NACL
  - accepts everything inbound/outbound with the subnets it's associated with
  - do not modify the Default NACL, instead create custom NACLs

## Ephemeral Ports
- For any two endpoints to establish a connection, they must use ports
- Clients connect to a defined port, and expect a response on an ephemeral port
- Different Operating Systems use different port ranges:
  - IANA & MS Windows 1024-65535
  - Many Linux Kernels 32768-60999

## Security Groups vs NACLs
- Security Groups
  - Operate at the instance level
  - Support allow rules only
  - Stateful: Return traffic is automatically allowed, regardless of any rules
  - All rules are evaluated before deciding whether to allow traffic
  - No deny rules
- NACLs
  - Operate at the subnet level
  - Support allow rules and deny rules
  - Stateless: Return traffic must be explicitly allowed by rules
  - Rules are evaluated in order when deciding whether to allow traffic
  - Explicit deny rules

## Network ACLs vs Security Groups
- NACLs are stateless, Security Groups are stateful
- NACLs are evaluated first, then Security Groups
- NACLs are evaluated in order, Security Groups are evaluated all at once
- NACLs can block IP addresses, Security Groups can block IP addresses but only in conjunction with Network ACL

## VPC Peering
- privately connect two vpc using AWS' network
- make them behave as if they were in the same network
- must not have overlapping CIDR blocks
- VPC peering connection is not transitive (must be established for each VPC that need to communicate with one another)
- You MUST UPDATE route tables in each VPC's subnets to ensure EC2 instances can communicate with each other

## VPC Peering good to know
- you can creat vpc peering connection between vpcs in different AWS accounts/regions
- you can reference a security group in a peered vpc (works cross account too)

## VPC Endpoints (AWS Private Link)
- every AWS service is publicly exposed (public url)
- VPC endpoints (powered by AWS PrivateLink) allows you to connect to AWS services using a private network instead of the public www network
- they're redundant and scale horizontally
- they remove the need of IGW, NAT, etc... to access AWS services
- in case of issues:
  - check DNS setting resolution in your VPC
  - check route tables

## Types of Endpoints
- Interface Endpoints (powered by private link)
  - provisions an ENI (private IP address) as an entry point (must attach a security group)
  - supports most AWS services
  - $ per hour + $ per GB of data processed
  - look for "supports interface endpoints" in the documentation
  - This endpoint is preferred access is required from on premises (site to site vpn or direct connect) a different vpc or different region
- Gateway Endpoints
  - provisions a gateway and must be used as a target in your route tables (does not use security groups)
  - supports both S3 and DynamoDB
  - free
  - look for "supports gateway endpoints" in the documentation
  - Gateway for S3

## VPC Flow Logs
- Capture information about IP traffic going into your interfaces
  - VPC flow logs
  - subnet flow logs
  - elastic network interface flow logs
- Helps to monitor & troubleshoot connectivity issues
- Flow logs data can go to S3, CloudWatch Logs, and Kinesis Data Firehose
- Can be tagged

## VPC Flow Logs Syntax
- srcaddr & dstaddr help identify problematic IP
- srcport & dstport help identify problematic ports
- Action: success or failure of the request due to security groups, NACL, etc...
- Can be used for analytics on usage patterns, or malicious behavior
- Query VPC flow logs using Athena on S3 or CloudWatch Logs Insights

## VPC FLow Logs - Architectures
- VPC flow logs => cloudwatch logs => cloudwatch insights
- VPC flow logs => cloudwatch logs => CW Alarm => SNS 
- VPC flow logs => S3 bucket => Athena => QuickSight

## VPC flow logs - cloudwatch permissions
- IAM service role associated with vpc flow logs must have the required permissions to publish logs to cloudwatch logs
- logs:CreateLogGroup, logs:CreateLogStream, logs:PutLogEvents

## AWS Site-to-site VPN
- Virtual Private Gateway (VGW)
  - VPN Concentrator on the AWS side of the VPN connection
  - VGW is created and attached to the VPC from which you want to create the site-to-site VPN connection
  - Possibility to customize the ASN
- Customer Gateway (CGW)
  - Software application or physical device on customer side of the VPN connection

## Site-to-site VPN Connections
- Customer Gateway Device (on-premises)
  - What IP address to use?
    - Public Internet-routable IP address for your customer gateway device
    - If it's behind a CGW, use the public IP of the NAT
- Important step: enable route propagation for the virtual private gateway in the route table that is associated with your subnets
- if you need to ping your EC2 instances from on-premises, make sure you add the ICMP protocol on the security group

## AWS VPN CloudHub
- provide secure communication between sites, if you have multiple VPN Connections
- low cost hub-and-spoke model for primary or secondary network connectivity between different locations (VPN only)
- it's a VPN connection so it goes over the public internet
- to set it up, connect multiple VPN connections on the same VGW, setup dynamic routing and configure route tables

## Direct Connect (DX)
- Provides an dedicated private connection from a remote network to your VPC
- Dedicated connection must be setup between your DC and AWS Direct Connect location
- You need to setup a Virtual Private Gateway on your VPC
- Access public resources (S3) and private (EC2) on same connection
- Use Case: Increase bandwidth throughput - working with large data sets - lower cost than site-to-site VPN
- Good for hybrid environments (on-premises + cloud)
- Good for high throughput workloads (HPC, transfers large amounts of data to the cloud)
- Good for more consistent network experience than internet-based connection

## Direct Connect Gateway
- if you want to setup a direct connect to one or more vpc in many different regions (same account), you must use a Direct Connect Gateway
- corporate data center 
## Direct Connect - Connection Types
- Dedicated Connections: 1Gbps, 10Gbps and 100 Gbps capacity
  - physical ethernet port dedicaed to a customer
  - request made to AWS first, then completed by AWS Direct connect partners
- Hosted connecions: 50Mbps, 500Mbps, to 10Gbps
  - connection requests are made via AWS Direct Connect Partners
  - capacity can be added or removed on demand
  - 1, 2, 5, 10 Gbps available at select AWS Direct Connect Partners
- Lead times are often longer than 1 month to establish a new connection

## Direct Connect - Encryption
- Data in transit is not encrypted but is private
- AWS Direct Connect + VPN provides an IPsec-encrypted private connection
- Good for an extra level of security, but slightly more complex to put in place

## Direct Connect - Resiliency
- High Resiliency for Critical Workloads
  - Setup a secondary connection in a different location
- Maximum Resiliency
  - Setup Direct Connect Gateway to have all on premises devices connect to all region
  
  
## Site-to-sit VPN Connection as backup
- in case of Direct Connect failure, you can setup a backup VPN connection

## transit gateway
- for having transitive peering between thousands of VPC and on premises hub and spoke star connection
- regional resource, can work cross region
- share cross account using resource acess manager (RAM)
- you can peer transit gateway across regions
- route table: limit which VPC can talk to other VPC
- works with Direct Connect Gateway, VPN connections
- supports IP multicast (not supported by any other AWS service)
- if you see multicast in the exam you should think of transit gateway

## Transit Gateway: Site to Site VPN ECMP
- another use case of transit gateway is to increase the bandwidth of your site to site VPN connection using ECMP
- ECMP = Equal-cost multi-path routing
- routing strategy to allow to forward a packet over multiple best path
- use case: create multiple Site-to-Site VPN connections to increase the bandwidth of your connection to AWS

## Transit Gateway: throughput with ECMP
- VPN to virtual private gateway
  - 1x = 1.25 Gbps
- VPN to transit gateway
  - 1x = 2.5GBps (ECMP) 2 tunnels used
  - 2x = 5 Gbps (ECMP)
  - 3x = 7.5 Gbps (ECMP)
  
## VPC - Traffic monitoring
- allows you to capture and inspect network traffic in your VPC
- route the rtaffic to security appliances that you manage
- Capture the traffic
  - from source: ENIs
  - to (targets): an ENI or a network load balance
- Capture all packets or capture the packets of your interest (optionally, truncate packets)
- Source and target can be in the same VPC or different VPCs (VPC peerinng)
- Use cases: content inspection, threat monitoring, troubleshooting

## What is IPv6?
- IPv4 desiged to provide 4.3 billion addresses (will be exausted soon)
- IPv6 designed to provide 3.4 x 10^38 unique IP addresses
- every ipv6 address in AWS is public and internet routable (no private range)
- format: x.x.x.x.x.x.x.x (x is hexadecimal, range can be from 0000 to ffff)
- Examples:
  - 2001:db8:3333:4444:5555:6666:7777:8888
  - 2001:db8:3333:4444:cccc:dddd:eeee:ffff
  - all 8 segments are zero 
  - 2001:db8:: the last 6 segments are zero
  - ::1234:5678 the first 6 segments are zero
  - 2001:db8::1234:5678 the middle 4 segments are zer

## IPv6 in VPC
- IPv4 cannot be disabled for your VPC (only IPv6 can be disabled)
- you can enable IPv6 (they're public ip addresses)
- your EC2 instances will get at least a private IPv4 and a public IPv6
- they can communicate using either IPv4 or IPv6

## IPv4 Troubleshooting
- IPv4 canno be disabled for your VPC and subnets 
- so if you cannot launch an ec2 instance in your subnet 
  - it's not because it cannot acquire an IPv6
  - it's because there are no available Ipv4 in your subnet
- solution: createa a new ipv4 cidr in your subnet

## Egress only internet Gateway
- used for IPv6 only
- similar to a NAT gateway but for IPv6
- allows instances in your VPC outbound connections over IPv6 while preventing the internet to initiate an IPv6 connection to your instances
- you must update the route tabl

## VPC Section Summary
- CIDR - IP range for your VPC, subnets, and others
- VPC - virtual private cloud: your own network in aws
- Subnets - tied to an AZ, a segment of your VPC's IP range
- Internet Gateway - at VPC level, provide IPv4 & IPv6 Internet Access
- NAT Gateway - give your private subnet access to the internet while remaining private
- NACL - Stateless, subnet rules for inbound and outbound, first layer of defense
- Security Groups - Stateful, operate at the EC2 instance level or ENI
- VPC Peering - connect two VPC with non overlapping CIDR, non-transitive
- VPC Endpoints - provide private access to AWS services (S3, DynamoDB) within a VPC
- VPC Flow Logs - can be setup at the VPC / Subnet / ENI level, for ACCEPT and REJECT traffic, helps to monitor and troubleshoot connectivity issues
- Site to Site VPN & Direct Connect - connect on-premises to AWS
- AWS VPN cloudhub - hub and spoke vpn model to connectyour sites
- Transit Gateway - connect thousands of VPC and on-premises networks together
- Traffic Mirroring - copy network traffic from ENIs for further analysis
- Egress-only Internet Gateway - like a NAT gateway, but for IPv6 
- Direct Connect - setup a virtual private gateway on VPC and establish a direct private connection to an AWS Direct Connect location
- Direct Connect Gateway - setup a Direct Connect to many VPCs in different AWS regions
- AWS PrivateLink - expose a service VPC to 1000s of customer VPCs
- ClassicLink - connect EC2-Classic instances privately to your VPC in the same region (deprecated)

## Networking Costs in AWS per GB - simplified
- Use Private IP instead of Public IP for good savings and better network performance
- Use same AZ for maximum savings (at the cost of high availability)

## Minimizing egress traffic network cost
- Egress traffic: outbound traffic from AWS to outside
- Ingress traffic: inboud traffic from outside to AWS
- try to keep as much internet traffic within AWS to minimize costs
- Direct Connect location that are co-located in the same AWS region result in lower cost egress networ

## S3 Data transfer pricing - analysis for USA
- S3 ingress: free
- S3 to internet: $0.09 per GB
- S3 Transfer Acceleration: $0.04 to $0.08 per GB
- S3 Data Transfer Out to other AWS services in the same region: free
- S3 to cloudfront 0.00 per gb
- cloudfront to internet: $0.085 per GB
- S3 cross region replication: $0.02 per GB

## Network protection on AWS
- To protect network on AWS weve seen
  - Network access control lists (NACLs)
  - amazon vpc security groups
  - aws waf
  - aws shield & shield advanced
  - aws firewall manager

## AWS network firewall 
- protect entire amazon VPC
- from layer 3 to 7 proteection
- any direction you can inspect
  - vpc to vpc traffic
  - outbound to internet
  - inbound from internet
  - to / from direct connect & site to site vpn
- internally the AWS network firewall uses the aws gateway load balancer 
- rules can be centrally managed cross account by AWS firewall manager to apply to many VPC

## network firewall - fine grained controls
- supports 1000s of rules
  - IP & port - example: 10ks of ips filtering
  - protocol - example: block the smb protocol for outbound communications
  - stateful domain list rule groups: only allow outbound traffic to my *.example.com or third party software rpo
  - general pattern matching using regex
- traffic filtering: allow, drop, alert for the traffic that matches rule
- active flow inspection to protect against network threats with intrusion prevention capabilities (like gateway load balancer, but all managed by AWS)
- send logs of rule matches to AMazon S3, cloudwatch logs, and kinesis data firehose

## Disaster Recovery Overview
- Any event that has a negative impact on a company's business continuity or finances is a disaster
- Disaster recovery (DR) is about preparing for and recovering from disaster
- What kind of disaster recovery?
  - on-premises => on-premises: traditional DR, and very expensive
  - on-premises => aws cloud: hybrid recovery
  - aws cloud region A => aws cloud region B
- Need to define two terms:
  - RPO: Recovery Point Objective
  - RTO: Recovery Time Objective

## Disaster Recovery Strategies
- Backup and Restore (High RPO)
- Pilot Light  
- Warm Standby
- Hot Site / Multi Site Approach

## Backup and Restore (High RPO)
- very high RPO
- RTO: hours to days to recover
- backups stored on S3, S3 IA, S3 Glacier + RDS snapshots, EBS snapshots, etc...
- restore the data to a new instance

## Pilot Light
- very low RPO
- a small version of the app is always running
- use for the critical core (pilot light)
- very similar to backup and restore
- faster than backup and restore as critical systems are already up

## Warm Standby
- full system is up and running but at minimum size
- upon disaster, we can scale to production load

## Multi Site / Hot Site Approach
- very low RTO
- very expensive
- full production scale running AWS and on-premises

## All AWS Multi-Region
- very low RTO
- very expensive
- full production scale running in AWS and across multiple regions
- good for critical applications

## Disaster Recovery Tips
- Backup
  - EBS Snapshots, RDS automated backups / snapshots, etc...
  - Regular pushes to S3, S3 IA, S3 Glacier, etc...
  - Database snapshots stored in S3
  - Lifecycle policy
  - Cross Region Replication
- High Availability
  - Use route53 to migrate DNS over from region to region
  - RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3
  - Site to Site VPN as a recovery from Direct Connect
- Replication
  - RDS Replication (cross region), AWS Aurora + Global Databases
  - Database replication from on-premises to RDS
  - Storage Gateway
- Automation
  - CloudFormation / Terraform replication of infrastructure
  - Recover / Reboot EC2 instances via SSM Automation Documents
  - AWS Lambda functions for customized automations
  - Step Functions to orchestrate recovery workflows
- Chaos
  - Netflix has a "simian-army" randomly terminating EC2

## AWS RTO and RPO Summary
- Backup and Restore: high RPO
- Pilot Light: lowest RTO, highest RPO
- Warm Standby: lowest RPO
- Multi Site / Hot Site: very low RTO and RPO 

## AWS Elastic Disaster Recovery (DRS)
- used to be named "CloudEndure Disaster Recovery"
- Quickly and easily recover your physical, virtual and cloud based servers into AW
S
- Continuous block level replication for your servers
- Example: protect your most critical databases (SQL, Oracle, MySQL, etc...)
- Continuous block level replication for your servers

## DMS - Database Migration Service
- Quickly and securely migrate databases to AWS, resilient, self healing
- The source database remains available during the migration
- Supports:
  - Homogeneous migrations: ex Oracle to Oracle
  - Heterogeneous migrations: ex Microsoft SQL Server to Aurora
- Continuous Data Replication using CDC
- You must create an EC2 instance to perform the replication tasks

## DMS Sources and  targets
- Sources:
  - On-Premise and EC2 instances (Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, DB2)
  - Azure: Azure SQL Database
  - Amazon RDS (all including Aurora) & Aurora Serverless
  - Amazon S3
- Targets:
  - On-Premise and EC2 instances (Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, SAP)
  - Amazon RDS, Amazon Redshift, Amazon DynamoDB, Amazon S3, DocumentDB, Amazon OpenSearch Service, Kinesis Data Streams, Apache Kafka
  - ElasticSearch Service

## AWS Schema Conversion Tool (SCT)
- Convert your Database's Schema from one engine to another
- Example OLTP: (SQL Server or Oracle) to MySQL, PostgreSQL, Aurora
- Example OLAP: (Teradata or Oracle) to Amazon Redshift
- You do not need to use SCT if you are migrating the same type of technologies
  - Example: On-Premise PostgreSQL => RDS PostgreSQL
  - The DB engine is still PostgreSQL (RDS is the platform)

## DMS - Multi-AZ Deployments
- when multi-AZ enabled, DMS provisions and maintains a synchronously stand replica in a different AZ
- Advantages:
  - provides data redundanc
  - eliminates I/O freezes
  - minimizes latency spikes

## RDS & Aurora MySQL Migrations
- RDS MySQL to Aurora MySQL
  - Option 1: DB snapshots from RDS MySQL restored as MySQL aurora DB
  - Option 2: Create an Aurora Read Replica your RDS mysql and when the replication is lag 0, promote it as its own DB cluster (can take time and cost $)
- External MySQL to Aurora MySQL
  - Option 1: 
    - Use Percona XtraBackup to create a file backup in amazon s3
    - create an aurora MySQl DB from amazon s3
  - Option 2L
    - create an aurora MySQL DB
    - use mysqldump utility to migrate data MySQL into aurora (slow than s3 method)
- Use DMS if both databases are up and runnin

## RDS & Aurora PostgreSQL migrations
- RDS postgreSQL to aurora postgresql
  - option 1: DB snapshots from RDS postgresql restored as PostgreSQL aurora DB
  - option 2: create an aurora read replica of your RDS postgresql and when the replication lag is 0, promote it as its own DB cluster (can take time and cost $)
- External PostgreSQL to Aurora PostgreSQL
  - create a backup and put it in Amazon S3
  - Import it using the aws_s3 aurora extension
- use dms if both databases are up and running

## On-Premise strategy with AWS
- Ability to download Amazon linux 2 ami as VM .iso format
  - VMWare, KVM, VirtualBox (Oracle VM), Microsoft Hyper-V
- VM import / export
  - migrate existing applications into EC2
  - create a DR repository strategy for your on-premise VMs
  - can export back the VMs from EC2 to on-premise
- AWS application discovery service
  - Gather information about your on-premise servers to plan a migration
  - Server utilization and dependency mappings
  - Track with AWS migration hub
- AWS database migration service (DMS)
  - replicate on-premise => AWS, AWS => AWS, AWS => on-premise
  - Works with various database technologies (Oracle, MySQL, DynamoD, etc...)
- AWS server migration service (SMS)
  - Incremental replication of on-premise live servers to AWS

## AWS Backup
- fully managed service 
- centrally manage and automate backups across AWS services
- no need to create custom scripts and manual processes
- Supported Services:
  - Amazon EC2/EBS
  - Amazon S3
  - Amazon RDS (all engines)
  - Amazon DynamoDB
  - Amazon EFS
  - Amazon FSx (Lustre & Windows File Server)
  - AWS Storage Gateway (Volume Gateway)
- Support cross-region and cross-account backups
- On-demand and scheduled backups
- Tag-based backup policies
- You create backup policies known as Backup Plans
  - Backup frequency (every 12 hours, daily, weekly, monthly, cron expression)
  - Backup window
  - Transition to cold storage (infrequent access)
  - Retention period (always, days, weeks, months, years, custom)

## AWS Backup Vault Lock
- enforce a work (write once read many) state for all the backups that you store in you AWS backup vault
- Additional layer of defense to protect your backup against:
  - inadvertent or malicious delete operations
  - updates that shorten or alter retention periods
- Even root user cannot delete backups when enabled

## AWS Application Discovery Service
- Plan migration projects by gathering information about on premises data centers
- Server utilization and dependency mapping are important for migrations
- Agentless Discovery (AWS Agentless Discovery Connector)
  - VM inventory, configuration, and performance history such as CPU, memory, and disk usage
- Agent-based Discovery (AWS Application Discovery Agent)
  - System configuration, system performance, running processes, and details of the network connections between systems
- Resulting data can be viewed within AWS Migration Hub

## AWS Application migration Service (MGN)
- The "AWS evolution" of cloudendure migration, replacing aws server migration server sms
- Lift-and-shift (rehost) solution which simplify migrating applications to AWS
- Convert your physical, virtual, and cloud-based servers to run natively on AWS
- Minimal downtime, reduced costs

## Transferring large amount of data into AWS
- Example: transfer 200tb of data in the cloud. We have 100mbps internet connection
- Over the internet / Site-to-Site VPN: 
  - Immediate to setup
  - will take 200(TB) * 1000(GB) * 8(Gb) / 100(Mbps) = 16,000,000s (185 days)
- Over direct connect 1gbps: 
  - Long for the one time setup (over a month)
  - will take 200(TB) * 1000(GB) * 8(Gb) / 1000(Mbps) = 1,600,000s (18.5 days)
- Over Snowball:
  - Takes about 1 week for the end to end transfer
  - can be combined with DMS
- For on-going replication/transfers: site-to-site vpn or DX with DMS or Datasyn

## VMware cloud on AWS
- Some customers use VMware cloud to manage their on-premises data center
- They want to extend the data center capacity to AWS, but keep using the VMware cloud software
- VMware Cloud On AWS
- Use cases:
  - migrate your VMware vSphere based workloads to AWS
  - Run your production workloads across VMware vSphere based private, public and hybrid cloud environments
  - have a disaster recovery strategy

## fan out pattern: deliver to multi sqs
- Process a message from SNS and send the message to multiple SQS queues, one per "subscriber"
- Fully decoupled, no data loss
- SQS allows for: data persistence, delayed processing and retries of work
- Ability to add more SQS subscribers over time
- Make sure your SQS queue access policy allows for SNS to write

## S3 event notifications
- S3:objectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication...
- Object name filtering possible (*.jpg)
- Use case: generate thumbnails of images uploaded to S3
- Can create as many "S3 events" as desired
- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer

## S3 event notifications with amazon event bridge
- Advanced filtering options with JSON rules (metadata, object size, name...)
- Multiple destinations - ex step functions, Kinesis streams
- EventBridge capabilities - archive, replay events, rely on advanced filtering

## Caching Strategies
- DB cache
- Cache at the application level
- CDN caching
- it's all about choosing where do we want to cache content
- how do we want to cache content,
- how long do we want to cache content,
- and then, are we okay with some latency,
- and which content actually do we want to be cached?

## Blocking IP Address
- NACLs can be used to block specific IP addresses at the subnet level
- AWS WAF can be used to block IPs at the ALB level
- ALB, cloudfront & WAF

## High Performance Computing (HPC)
- The cloud is the perfect place to perform HPC
- You can create a very high number of resources in no time
- You can speed up time to results by adding more resources
- You can pay only for the systems you've actually used
- Perform genomics, computational chesmitry, financial risk modeling, weather prediction, machine learning, deep learning, autonomous driving

## Data Management & Transfer
- AWS direct connect
  - move gb/s of data to the cloud over a private secure network
- Snowball and Snowmobile
  - move pb of data to the cloud
- AWS Datasync
  - move large amount of data between onpremise and s3, efs, fsx for windows

## Compute and networking
- EC2 instances:
  - CPU optimized, gpu optimized
  - spot instances / spot fleets for cost saving + auto scaling
- EC2 Placement groups: cluster for good network performance
- EC2 enhaced networking (SR-IOV)
  - higher bandwidth, higher pps (packet per second), lower latency
  - Option 1: elastic network adapter (ENA) up to 100Gbps
  - Option 2: intel 82599 VF up to 10Gbps - LEGACY
- Elastic Fabric Adapter (EFA)
  - improved ENA for HPC, only works for linux
  - great for inter-node communications, tightly coupled workloads
  - leverages message passing interface (MPI) standard
  - bypasses the underlying linux OS to provide low-latency, reliable transport

## Storage
- Instance attached storage:
  - EBS: scale up to 256000 IOPS with io1/io2
  - Instance store: scale to millions of IOPS, linked to ec2 instance, low latency
- network storage:
  - S3: large blog not a file system
  - EFS: scale IOPS based on total size or use provisioned IOP
  - FSx for Lustre: HPC optimized distributed file system, millions of IOPS

## Automation and orchestration
- AWS Batch
  - AWS batch supports multi-node parallel jobs, which enables you to run single jobs that span multiple EC2 instances
  - easily schedule jobs and start ec2 instances accordingly
- AWS ParallelCluster
  - open source cluster management tool to deploy HPC on aws
  - configure with text files
  - automate creation of vpc, subnet, cluster type and instance type
  - ability to enable EFA on the cluster (improves network performanc
  
## Creating a highly available EC2 Instance
- Auto scaling group
- Multi AZ deployment
- Elastic load balancer
- ASG + EBS

## Cloudformation
- Is a declarative way of outlining your AWS infrastructure, for any resources (most of them are supported)
- Cloudformation is free, but you pay for the resources it creates
- for example: within a cloudformation template you say:
  - I want a security group
  - I want two EC2 instances using this security group
  - I want two elastic IPs for these EC2 instances
  - I want an S3 bucket
  - I want a load balancer (ELB) in front of these EC2 instances
- Cloudformation creates those for you, in the right order, with the exact configuration that you specify

## benefits of AWS cloudformation
- Infrastracture as a code
  - no resources are manually created which is excellent for control
  - changes to the infrastructure are reviewed through code
- Cost
  - each resources within the stack is tagged with an identifier so you can easily see how much a stack costs you
  - you can estimate the costs of your resources using the cloudformation template
  - savings strategy: in dev, you could automate deletion of templates at 5 PM and recreated at 8 AM, safely
- Productivity
  - Ability to destroy and re-create an infrastructure on the cloud on the fly
  - Automated generation of Diagram for your templates!
  - Declarative programming (no need to figure out ordering and orchestration)
- Don't re-invent the wheel
  - leverage existing templates on the web!
  - leverage the documentation
- Supports (almost) all AWS resources:
  - evertythin we'll see in this course is supported
  - you can use "custom resources" for resources that are not supported

## Cloudformation + Infrastracture Composer
- Example: wordpress cloudformation stack
- we can see all the resources
- we can see all the relationships between the components
