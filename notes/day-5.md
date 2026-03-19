# Day 5 \- March 12

**Scalability and High Availability**

- Scalability means that an application system can handle greater loads by adapting.  
- There are two kinds: vertical and horizontal.

**Vertical Scalability**

- Means increasing the size of the instance  
- For example, if an application runs on a t2.micro, scaling that application vertically means running it on a t2.large  
- Vertical scalability is very common for non-distributed systems, such as databases  
- RDS and ElastiCache are services that can scale vertically  
- There is usually a limit to how much you can scale (hardware limit)

**Horizontal Scaling**

- Means increasing the number of instances/systems for your application  
- Horizontal scaling implies a distributed system. This is very common for web applications or modern applications. But remember, not every application can be horizontally scaled. It is easy to horizontally scale thanks to cloud offerings such as Amazon EC2.

**High Availability** 

- It usually goes hand in hand with horizontal scaling  
- Means running your application/system in at least 2 data centers (2 Availability Zones)  
- The goal of high availability is to survive a data center loss. It can be passive (for RDS Multi-AZ, for example)

**Load Balancer**

- Load balancers are servers that forward traffic to multiple downstream servers (e.g. EC2 instances)  
- Spread load across multiple downstream instances  
- Expose a single point of access DNS to the application  
- Seamlessly handle failures of downstream instances  
- High availability across zones  
- Separate public traffic and private traffic

**Elastic Load Balancer** 

- Is a managed load balancer  
- AWS guarantees it will be working; AWS takes care of upgrades, maintenance, and high availability  
- It is integrated with many AWS services

**Health Checks** 

- Are crucial for load balancers  
- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests  
- The health check is done on a port and route /health  
- If the response is not 200 (OK), then the instance is unhealthy

**Types of load balancer on AWS** 

* Classic Load Balancer V-1 old generation 2009   
* Application Load Balancer v2 new gen 2016   
* Network Load Balancer v2 new gen 2017  
* Gateway Load Balancer 2020. GWLB operates at layer 3 (network layer), IP protocol

Overall, it is recommended to use the newer-generation load balancers as they provide more features. Some load balancers can be set up as internal/private or external/public ELBs.

**Application Load Balancer v2**

- ALB is layer 7 (HTTP)  
- The following cookie names are reserved by the ELB (AWSALB, AWSALBAPP, AWSALBTG).  
- Load balancing to multiple HTTP applications across machines (target groups)  
- Load balancing to multiple applications on the same machine (e.g. containers)  
- Support for HTTP/2 and WebSocket  
- Support redirects (from HTTP to HTTPS, for example)  
- ALBs are a great fit for microservices and container-based applications, e.g. Docker and Amazon ECS  
- The application servers do not see the IP of the client directly
