# Day 3 \- March 10

**Private vs. Public IP (IPv4)**  
**Two types of IP: IPv4 and IPv6**

- IPv4 is the most common format and allows 3.7B different addresses, e.g. 1..160.10.240  
- IPv6 is newer and solves problems for IoT, e.g. 3ffe:1900:4545:3;200:f8ff:fe21:67cf

**Public IP** \- a machine can be identified on the internet; it is unique across the whole web and can be geolocated easily.  
**Private IP** \- a machine can only be identified on a private network; it is unique within that private network. Two different private networks can have the same IPs. It connects using an internet gateway, and only specified ranges of IPs can be used as private IPs.  

**Elastic IPs**

- When you start and then stop an EC2 instance, its public IP can change.  
- If you need a fixed public IP for an instance, you need an Elastic IP.  
- An Elastic IP is a public IPv4 IP you own as long as you do not delete it.  
- You can attach Elastic IPs one instance at a time. Using an Elastic IP, you can mask the failure of an instance by remapping the address to another instance. You can only have 5 Elastic IPs in your account (you can ask AWS to increase it).  
- Avoid Elastic IPs; they reflect poor architectural decisions. Use a public IP and register a DNS name to it instead, or use a load balancer and do not use a public IP.  
- By default, EC2 comes with a private IP for the internal AWS network.  
- A public IP is for the web. When doing SSH into EC2 machines, we cannot use a private IP because we are not in the same network.  
- If a machine is stopped and then started, the public IP can change.

**Placement Groups** \- To control how EC2 instances are placed within AWS infrastructure, the placement strategy can be defined using placement groups.  
**Three strategies for placement groups:**

- **Cluster**: instances into a low-latency group available in an AZ. Pros: great network performance (10 Gbps bandwidth). Cons: if an AZ fails, all instances fail. Good for big data jobs that need to complete fast, applications that need extremely low latency, and high network throughput.  
- **Spread**: instances across underlying hardware (max 7 instances per group per AZ) for critical applications. Pros: can span across availability zones, reduced risk of failure, each instance is on different physical hardware. Good for apps that need to maximize high availability or are critical applications.  
- **Partition**: instances across many different partitions within an AZ, scaling to 100s of EC2 instances per group. (Use cases: HDFS, HBase, Cassandra, Kafka.) Instances in one partition do not share racks with the instances in other partitions.

**Elastic Network Interfaces (ENI)**

- Logical component in a VPC that represents a virtual network card  
- You can create ENIs independently and attach them on the fly (move them) to EC2 instances for failover  
- Bound to a specific AZ

**ENI Attributes:**

- ENI can have a primary private IPv4, one or more secondary IPv4s  
- One Elastic IP (IPv4) per private IPv4  
- One public IPv4  
- One or more security groups  
- A MAC address

**EC2 Hibernate**

- The in-memory (RAM) state is preserved, the instance boots much faster, the OS is not stopped/restarted, and under the hood the RAM state is written to a file in the root EBS volume. The EBS volume must be encrypted  
- Use cases: long-running processing, saving the RAM state, services that take time to initialize  
- Supported instance families: c3, c4, c5, i3, m3, m4, r3, r4, t2, t3, ...  
- Instance RAM size must be less than 150 GB  
- Instance size is not supported for bare-metal instances  
- Supports many AMIs  
- Available for On-Demand, Reserved, and Spot instances  
- An instance can NOT be hibernated for more than 60 days
