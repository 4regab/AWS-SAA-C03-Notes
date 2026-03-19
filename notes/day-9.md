# Day 9 \- March 16

**DNS - Domain Name System**

- Domain Name System translates human-friendly hostnames into machine IP addresses

**DNS Terminologies**

- Domain Registrar: Amazon Route 53, GoDaddy  
- DNS Records: A, AAAA, CNAME, NS, ...  
- Zone File: contains DNS records  
- Name Server: resolves DNS queries (authoritative or not)  
- Top-Level Domain (TLD): .com, .us, .in, .gov, .org   
- Second-Level Domain (SLD): Amazon.com, google.com

**Route 53**

- A highly available, scalable, fully managed, authoritative DNS service  
  - Authoritative = the customer can update the DNS records  
- Route 53 is also a domain registrar  
- Health checks are the only service that provides a 100% availability SLA  
- 53 is a reference to the traditional DNS port

**Route 53 Records**

- How you want to route traffic for a domain  
- **Domain/subdomain name**: e.g. example.com  
- **Record types**: A or AAAA, CNAME, NS  
- **Value**: e.g. 12.34.56.67  
- **Routing policy**: how Route 53 responds to queries  
- **TTL** (time to live): amount of time the record is cached at DNS resolvers

**Route 53 Record Types**

- **A**: maps a hostname to IPv4  
- **AAAA**: maps a hostname to IPv6  
- **CNAME**: maps a hostname to another hostname. You cannot create it for the top node of a DNS name space or the zone apex, e.g. you cannot create it for example.com, but you can for www.example.com  
- **NS (Name servers)** for the hosted zone: control how traffic is routed for a domain

**Route 53: Hosted Zones**

- Container for records that define how to route traffic to a domain and its subdomains  
- **Public hosted zones**: route records for traffic on the internet (public domain names)  
- **Private hosted zones**: records that specify route traffic within one or more VPCs (private domain names), e.g. Application1.company.internal  
- You pay $0.50 per month per hosted zone

**Route 53 Records TTL (Time to Live)**

- **High TTL**: e.g. 24 hrs  
  - Less traffic on Route 53  
  - Possibly outdated records  
- **Low TTL**: e.g. 60 sec  
  - More traffic on Route 53 (costs more)  
  - Records are outdated for less time  
  - Easy to change records  
- TTL is mandatory for every record except for alias records

**CNAME vs. Alias**

- AWS resources (load balancers, CloudFront, etc.) expose an AWS hostname:
  - 12-213.us-east-2.elb.amazonaws.com and you want myapp.mydomain.com  
- **CNAME:**  
  - Points a hostname to any other hostname (`app.mydomain.com` => `bla.mydomain.com`)  
  - Only for non-root domains (`app.mydomain.com`)  
- **Alias:**  
  - Points a hostname to an AWS resource (`app.mydomain.com` => `bla.mydomain.com`)  
  - Works for root domain and non-root domain (`mydomain.com`)  
  - Free of charge with native health check

**Route 53 Alias Records**

- Maps a hostname to an AWS resource  
- An extension to DNS functionality  
- Automatically recognizes changes in the resource's IP address  
- Can be used for the top node of a DNS name space (zone apex)  
- Alias records are always of type A/AAAA for AWS resources (IPv4/IPv6)  
- You cannot set TTL; Route 53 sets it for you

**Route 53 Alias Record Targets**

- Elastic Load Balancers  
- CloudFront distributions  
- API Gateway  
- Elastic Beanstalk environments  
- S3 websites  
- VPC interface endpoints  
- Global Accelerator   
- Route 53 record in the same hosted zone  
- You cannot set an alias record for an EC2 DNS name

**Route 53 Routing Policies**

- Define how Route 53 responds to DNS queries  
- DNS does not route any traffic; it only responds to DNS queries

**Routing Policies - Simple**

- Typically route traffic to a single resource  
- Can specify multiple values in the same record  
- If multiple values are returned, a random one is chosen by the client  
- When alias is enabled, specify only one AWS resource  
- Can't be associated with health checks

**Routing Policies - Weighted**

- Control the percentage of requests that go to each specific resource  
- DNS records must have the same name and type  
- Can be associated with health checks  
- Use cases: load balancing between Regions, testing, new application version  
- Assign a weight of 0 to a record to stop sending traffic to a resource

**Routing Policies - Latency**

- Redirect resources that have the least latency close to users  
- Super helpful when latency for the user is a priority  
- Latency is based on traffic between users and AWS Regions  
- Can be associated with health checks (has failover capability)

**Route 53 Health Checks**

- HTTP health checks are only for public resources  
- Health check => automated DNS failover:  
  - 1. Health checks that monitor an endpoint (application, server, other AWS resource)  
  - 2. Health checks that monitor other health checks (calculated health checks)  
  - 3. Health checks that monitor CloudWatch alarms (full control)  
- HTTP health checks are integrated with CloudWatch metrics

**Route 53 Calculated Health Checks**

- Combine the results of multiple health checks into a single health check  
- You can use OR, AND, and NOT

**Private Hosted Zones**

- Route 53 health checkers are outside the VPC   
- They can't access private endpoints  
- You can create a CloudWatch metric and associate a CloudWatch alarm, then create a health check that checks the alarm itself

**Routing Policies - Failover**

- If the health check becomes unhealthy, then Route 53 is going to automatically fail over to the second EC2 instance and start sending that result back instead.

**Routing Policies - Geolocation**

- This routing is based on user location  
- Specify location by continent, country, or by US state  
- You should create a default record   
- Use cases: website localization, restrictions, content distribution, load balancing  
- Can be associated with health checks

**Routing Policies - Geoproximity**

- Route traffic to your resources based on geographic location of users and resources  
- Ability to shift more traffic to resources based on defined bias  
- To change the size of the geographic region, specify bias values:  
  - To expand (1-99), more traffic to the resource  
  - To shrink (-1 to -99), less traffic to the resource  
- You must use the advanced Route 53 Traffic Flow feature

**Routing Policies - IP-based routing**

- Routing is based on clients' IP addresses  
- You provide a list of CIDRs for your clients and the corresponding endpoints/locations (user-IP-to-endpoint mappings)  
- Use cases: optimize performance, reduce network costs. Example: route end users from a particular ISP to a specific endpoint

**Routing Policies - Multi Value**

- Use when routing traffic to multiple resources  
- Route 53 returns multiple values/resources  
- Can be associated with health checks  
- Up to 8 healthy records  
- Multi value is not a substitute for having an ELB

**Domain Registrar vs. DNS Service**

- You buy or register your domain name with a domain registrar, typically by paying annual charges (e.g. GoDaddy, Amazon Registrar, Namecheap)  
- A domain registrar usually provides you with a DNS service to manage your DNS records, but you can use another DNS service to manage your DNS records like Route 53 or Cloudflare

**Route 53 Hybrid DNS**

- **Hybrid DNS** resolves your DNS queries between VPCs (Route 53 Resolver) and your networks (other DNS resolvers)  
- Networks can be:  
  - VPC itself / Peered VPC  
  - On-premises network (connected through Direct Connect or AWS VPN)

**Route 53 Resolver Endpoints**

- **Inbound Endpoint:** allows your DNS resolvers to resolve domain names for AWS resources (e.g. EC2 instances) and records in private hosted zones  
- **Outbound Endpoint:** Route 53 Resolver forwards DNS queries to your DNS resolvers  
- Use case: if you want to connect your on-premises data center and AWS and make sure DNS queries are resolved both ways, you must use the resolver inbound and outbound endpoints.
