# Day 27 - April 3

## AWS Organizations
- Global service
- allows to manage multiple aws accounts
- the main account is the management account
- other accounts are member accounts
- member accounts can only be part of one organization
- consolidated billing across all accounts - single payment method
- pricing benefits from aggregated usage (volume discount for ec2, s3...)
- shared reserved instances and savings plans discounts across accounts
- API is available to automate aws account creation
- Advantages
  - multi account vs one account multi vpc
  - use tagging standards for billing purposes
  - enable cloudt trail on all accounts, send logs to central s3 account
  - send cloudwatch logs to central logging account
  - establish account roles for admin purposes
- Security: Service control (SCP)
  - IAM Policies applied to OU or accounts to restrict users and roles
  - must have an explicit allow from the root through each OU in the direct path to the target account (does not allow anything by default - like IAM)
## AWS Organization Tag policies
- Helps you standardize tags across resources in an aws organization
- ensure consistent tags, autdit tagged resources maintain proper resoureces categorization,..
- you dfine tag keys and their allowed values
- helps with AWS Cost Allocation tags and attribute-based access control
- Generate a report that list all tagged/non compliant resources
- Use eventbridge to monitor compliant tags
## Resource policies & aws:PrincipalOrgID
- aws:PrincipalOrgID can be used in any resource policies to restrict access to accounts that are member of an AWS Organization
## IAM Roles Vs Resource Based Policies
- When you assume a role (user, application or service), you give up your permissions and take the permissions assigned to the role
- When using a resource based policy, the principal doesnt have to give up his permissions
- example: user in account A needs to scan a dynamodb table in account A and dumpt it in an s3 bucket in account b
- supported by: amazon s3 buckets, sns topics, sqs queues, etc
## Amazon Event bridge - security
- when a rule runs, it needs permission on the target
- resource based policy: lambda, sns, sqs, s3 buckets, api gateway...
- IAM Role: Kinesis stream, ec2 auto scaling, systems manager run command, ecs task
## IAM Permission boundaries
- IAM permission boundaries are supported for users and roles (not groups)
- Advanced feature to use a managed policy to set the maximum permissions an IAM entity can get
- An IAM permission boundary does not grant permissions. It only sets the maximum permissions an entity can get
- If you set a permission boundary policy on a user or role, then the entity can perform only the actions that are allowed by both the identity-based
## AWS IAM Identity Center (successor to aws single sign on)
- one login (single sign-on) for all your
  - aws accounts in aws organizations
  - business cloud applications (e.g. salesforce, box, m365)
  - saml2.0-enabled applications
  - ec2 windows instnaces
- identity providers
  - built in identity store in IAM identity center
  - 3rd party: active directory (AD), onelogin, OKTA..
## AWS IAM Identity Center Fine-Grained permissions and Assignments
- Multi account permissions
  - manage access across aws accounts in your AWS Organization
  - permission sets - a collection of one or more IAM Policies
- Application Assignments
  - SSO Access to many SAML 2.0 business applications (salesforce, box, microsoft 365)
- Attirbute-based access control (ABAC)
  - fine-grained permissions based on users' attributes stored in IAM Identity Center Identity Store
  - example: cost center, title, locale,...
- Use Case: Define permissions once then modify aws access by changing the attributes
## What is Microsoft Active Directory (AD)?
- Found on any windows server with AD Domain Services
- Database of objects: User Accounts, Computers, Printers, File Shares, Security Groups
- Centralized security management, create accounts, assign permissions
- Objects are organized in trees
- A group of trees is a forest
## AWS Directory Services
- AWS Managed Microsoft AD
  - create your own AD in AWS, manage users locally, supports MFA
  - establish "trust" connections with your on-premise AD
- AD Connector
  - directory gateway (proxy) to redirect to on-premise AD, supports MFA
  - users are managed on the on-premise AD
- Simple AD
  - AD-compatible managed directory on AWS
  - cannot be joined with on-premise A
## IAM Identity Center - Active directoryr setup
- connect to an aws managed microsoft ad (directory service)
  - integration out of the box
- connect to a self managed directory
  - create a two way trust relationship using aws managed microsoft ad
  - create an AD connector
## AWS Control Tower
- Easy way to set up and govern a secure and compliant multi-account aws environment based on best practices
- AWS control tower uses aws organizations to create accounts
- Benefits: 
  - automate the set up of your environment in a few clicks
  - automate ongoing policy management using guardrails
  - detect policy violations and remediate them
  - monitor compliance through an interactive dashboard
## AWS control tower - guardrails
- provides ongoing governance for your control tower environment (aws accounts)
- preventive guardrails - using SCs (e,g., restrict regions across all your accounts)
- detective guardrails - using AWS Config (e.g., identity untagged resources)
## AWS Security - Encryption
- Encryption in flight (SSL)
  - data is encrypted before sending and decrypted after receiving
  - SSL certificates help with encryption (HTTPS)
  - Encryption in flight ensures no MITM
- Server side encryption at rest
  - data is encrypted after being received by the server
  - data is decrypted before being sent
  - it is stored in an encrypted form thanks to a key (usually a data key)
  - the encryption / decryption keys must be managed somewhere and the server must have access to it
- Client side encryption
  - data is encrypted by the client and the server doesn't participate in encryption
  - the server should not be able to decrypt the data
  - could leverage envelope encryption
## AWS KMS (key management service)
- anytime you hear "encryption" for an AWS service, it's most likely KMS
- easy way to control access to your data, AWS manages keys for us
- capable of generating, rotating, disabling, deleting encryption keys
- integrated with most AWS services (EBS, S3, RDS, SSM...)
- seamless integration with IAM for authorization
- never ever store your secrets in plaintext, especially in your code!
- Encrypted secrets can be stored in the code / environment variables
## KMS Keys Types
- KMS keys is the new name for customer master key
- Symmetric (AES-256 keys)
  - single encryption key that is used to Encrypt and Decrypt
  - AWS services that are integrated with KMS use Symmetric CMKs
  - you never get access to the KMS key unencrypted (must call KMS API to use)
- Asymmetric (RSA & ECC key pairs)
  - public (Encrypt) and private key (Decrypt) pair
  - used for Encrypt/Decrypt or Sign/Verify operations
  - the public key is downloadable, but you can't access the Private Key unencrypted
  - use case: encryption outside of AWS by users who can't call the KMS API
## AWS KMS (Key management service)
- Types of KMS Keys: 
  -AWS Owned Keys (free): SSE-S3, SSE-SQS, SSE-DDB (default key)
  - AWS Managed Key: free (aws/service-name, ex: aws/rds, aws/ebs)
  - Customer managed keys created in KMS: $1/month
  - Customer managed keys imported (must be 256-bit symmetric key): $1/month
  - + pay for API call to KMS ($0.03/10000 calls)
- Automatic Key Rotation
  - aws managed kms keys: automatic key rotation every 1 year
  - customer managed kms keys: (must be enabled) automatic key rotation every 1 year
  - imported kms keys: only manual rotation possible using alias
## KMS Key Policies
- control access to KMS keys, "similar" to S3 bucket policies
- difference: you cannot control access without them
- Default KMS Key Policy:
  - created if you don't provide a specific KMS key policy
  - completes access to the key to the root user = entire AWS account
  - Gives you admin permissions
## Copying Snapshots across accounts
1. create a snapshot, encrypted with your own KMS Key (customer managed key)
2. attach a KMS Key Policy to authorize cross-account access
3. share the encrypted snapshot
4. (in target) create a copy of the snapshot, encrypt it with your own KMS Key (customer managed key)
5. create a volume from the snapshot
## KMS multi-region keys
- identical KMS key in different AWS regions that can be used interchangeably
- multi-region keys have the same key ID, key material, automatic rotation
- encrypt in one region and decrypt in other regions
- no need to re-encrypt or making cross-region API calls
- KMS multi-region are NOT global (primary+replicas)
- each multi-region key is managed independently
- use cases: global client-side encryption, encryption on global dynamodb, global auror
## DynaboDM Global Tables and KMS multi region keys client-side encryption
- we can encrypt specific attributes client-side in our dynamodb table using the amazon dynamodb encryption client
- combine with global tables, the client-side encrypted data is replicated to other regions
- if we use a multi region key, replicated in the same region as dynamodb global table, then clients in these region can use low latency API calls to KMS in their region to decrypt the data client side
## Global Aurora and KMS multi region keys client-side encryption
- we can encrypt specific attributes client-side in our aurora using the AWS Encryption SDK
- combine with  aurora global tables, the client-side encrypted data is replicated to other regions
- if we use a multi region key, replicated in the same region as global aurora, then clients in these region can use low latency API calls to KMS in their region to decrypt the data client side
- using client side encryption we can protect specific fields and gurantee only decryption if the client has access to an API key, we can protect specific fields even from data base admin
## S3 Replication Encryption Considerations
- Unencrypted objects and objects encrypted with SSE-S3 are replicated by default
- Objects encrypted with SSE-KMS you need to enable the option
  - specify which KMS key to encrypt the objects within the target bucket
  - adapt the KMS key Pollicy for the tare key
  - an IAM Role with kms:decrypt for the source KMS key and kms:Encrypt for the target KMS key
  - you might ge throttling errors, in which case you can ask for a service qoutas increase
- you can use multi region AWS KMS Keys, but they are currently treated as independent keys by amazon s3 (The object will still be decrypted and then encrypted)
## AMI Sharing Process Encrypted via KMS
 1. AMI in source account encrypted with KMS key from source account
 2. Must modify the image attribute command to add a launch permission which corresponds to the target account ID
 3. Must share the KMS key used to encrypted the snapshot the AMI references with the target account / IAM Role
 4. The IAM role/User in the target account must have the permissions to DescribeKey, ReEncrypt*, CreateGrant, Decrypt
 5. When launching the EC2 instance from the AMI optionally the target account can specify a new KMS key in its own account to re-encrypt the volume
## SSM Parameter Store
- Secure storage for configuration and secrets
- Optional Seamless Encryption using KMS
- Serverless, scalable, durable, easy SDK
- Version tracking of configurations / secrets
- Security through IAM
- Notifications with Amazon EventBridge
- Integration with Amazon CloudFormation
## Parameters Policies (for advanced parameters)
- Allow to assign a TTL to a parameter (expiration date) to force updating or deleting sensitive data such as passwords
- Can assign multiple policies at a time
## AWS Secret Manager
- Newer service, meant for storing secrets
- Capability to force rotation of secrets every X days
- Automate generation of secrets on rotation (uses Lambda)
- Integration with Amazon RDS (MySQL, PostgreSQL, Aurora)
- Secrets are encrypted using KMS
- Mostly meant for RDS integration
## AWS Secret Manager - Multi Region Secrets
- Replicate Secrets across multiple AWS Regions
- Secrets Manager keeps read replicas in sync with the primary Secret
- Ability to promote a read replica secret to a standalone secret
- Use cases: multi-region apps, disaster recovery strategies, multi-region DB...
- No extra charges for using multi-region secrets
## AWS Certificate Manager (ACM)
- easily provision, manage, and deploy SSL/TLS certificates
- Provide in-flight encryption for websites (HTTPS)
- Supports both public and private TLS certificates
- Free of charge for public TLS certificates
- Automatic TLS certificate renewal
- Integrations with (load balancers, CloudFront, APIs on API Gateway)
- Cannot use ACM with EC2 (must use IAM instead for EC2 instances
## ACM - Requesting public certificates
1. List domain names to be included in the certificate
  - Fully qualified domain name (FQDN) example: example.com or *.example.com
2. Select validation method (DNS or Email)
  - DNS validation is preferred for automation
  - Email validation will send emails to contact addresses in the WHOIS database
  - DNS validation will leverage a CNAME record to DNS config (ex: Route 53)
3. It will take few hours to get verified.
4. The public certificate will be enrolled for automatic renewal
  - ACM automatically renews ACM-generated certificates 60 days before expiry
## ACM - Importing public certificates
- Option to generate the certificate outside of ACM and then import it
- No automatic renewal, must import a new certificate to replace an expiring one
- ACM sends daily expiration events starting 45 days prior to expiration
  - the # of days can be configured
  - events are appearing in EventBridge
- AWS Config has a managed rule named acm-certificate-expiration-check to check for expiring/expired certificates (configurable number of days)
## ACM - Integration with ALB
- Need to specify HTTPS listener rules to redirect HTTP to HTTPS
- Need to specify a default certificate and can add an optional list of certs to support multiple domains
- Clients can use SNI (Server Name Indication) to specify the hostname they reach
## API Gateway - Endpoint Types
- edge optimized (default): For global clients
  - Requests are routed through the CloudFront Edge locations (improves latency)
  - The API endpoint will be assigned from the CloudFront edge location
- Regional:
  - For clients within the same region
  - Could manually combine with CloudFront (more control over the caching strategies and the distribution)
- Private:
  - Can only be accessed from your VPC using an interface VPC endpoint (ENI)
  - Use a resource policy to define access
## ACM - Integration with API gateway
- create a custom domain name in API Gateway
- Edge-optimized (default): for global clients
  - requests are routed through the CloudFront Edge locations (improves latency)
  - The API gateway still lives in only one region
  - the TLS Certificate must be in the same region as cloudfront in us-east-1
  - the setup cname or (better) a-alias record in route 53
- Regional:
  - for clients within the same region
  - TLS certificate must be in the API Gateway region
  - setup a CNAME or (better) A-alias record in Route 53
## CloudHSM
- KMS => AWS manages the software for encryption
- CloudHSM => AWS provisions encryption hardware
- Dedicated Hardware (HSM = Hardware Security Module)
- You manage your own encryption keys entirely (not AWS)
- HSM device is tamper resistant, FIPS 140-2 Level 3 compliance
- Supports both symmetric and asymmetric encryption (ssl/tsl keys)
- No free tier available
- must use the cloudhsm client software
- redshift support cloudhsm for database encryption and key management
- good optoin to use with sse-c encryption
- CloudHSM software: manage the keys and users
## CloudHSM - high availability
- CloudHSM clusters are spread across multi AZs
- great for availability and durability
## CloudHSM - integration with AWS Services
- through integration with AWS KMS
- configure KMS custom key store with cloudhsm
- example: EBS, s3, rds,..
## CLoudhsm vs KMS 
- CloudHSM: AWS provisions encryption hardware, customer managed CMK
- KMS: AWS manages the software for encryption , AWS managed , AWS Owned CMK, Customer managed CMK
- CloudHSM must be used within a VPC
- CloudHSM is FIPS 140-2 level 3 compliance
- CloudHSM is tamper resistant
- CloudHSM is can be shared across VPCs
- CloudHSM is more expensive than KMS
- KMS  is integrated with most AWS services
- KMS is available in multi regions 
- KMS is cheaper than CloudHSM 
## AWS WAF - Web Application Firewall
- Protects your web applications from common web exploits (layer 7)
- Layer 7 is HTTP (vs layer 4 is TCP)
- Deploy on ALB, API Gateway, CloudFront, appsync graphql api, cognito user pool
- Define Web ACL (Web Access Control List) Rules:
  - IP set: up to 10,000 IP addresses
  - HTTP headers, HTTP body, or URI strings protect from common attack - SQL injection and cross-site scripting (XSS)
  - Size constraints, geo-match (block countries)
  - Rate based rules (to count occurrences of events) - for DDoS protection
- Web ACL are Regional except for cloudfront 
- A rule group is a reusable set of rules that you can add to a web ACL
## WAF - fixed IP while using WAF with a load balancer
- WAF does not support network load balancer (layer 4)
- we can use global accelerator for fixed IP on the ALB
## AWS Shield: protect from ddos attack
- DDoS: distributed denial of service - many requests at the same time
- AWS Shield Standard: 
  - free service that is activated for every AWS Customer provides protection from attacks such as SYN/UDP floods, reflection attacks and other layer3/4 attacks
- AWS shield advance
  - optional DDoS mitigation service ($3000 per month per organization)
  - protect against more sophisticated attack on Amazon EC2, Elastic Load Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Route 53
  - 24/7 access to AWS DDoS response team (DRP)
  - Protect against higher fees during usage spikes due to DDoS
  - Shield Advanced automatic application layer DDoS mitigation automatically creates, evaluates, and deploys AWS WAF rules to mitigate layer 7 attacks
## AWS Firewall Manager
- manage rules in all accounts of an AWS Organization
- security policy: common set of security rules
  - WAF rules (application load balancer, API gateways, cloudfront)
  - AWS Shield Advanced (ALB, CLB, NLB, Elastic IP, CloudFront)
  - Security Groups for EC2 and ENI resources in VPC
  - AWS Network Firewall (VPC Level)
  - Amazon Route 53 Resolver DNS Firewall
  - Policies are created at the organization level
- Rules are applied to new resources as they are created across all and future accounts in your organization (good for compliance)
## WAF vs Firewall manager vs Shield
- WAF, shield, and firewall manager are used together for comprehensive protection
- define your web acl rules in waf
- for granular protection of your resources, waf alone is the correct choice
- if you want to use aws waf accross accounts, accelerate waf configuration, automate the protection of new resources, use firewall manager with AWS WAF
- shield advanced adds additional feature on top of AWS WAF, such as dedicated support from the shield response team (SRT) and advanced reporting
- if you're prone to frequent DDoS attacks, consider purchasing shield advance
## AWS Best Practices for DDoS resiliency edge location mitigation (BPI, BP3)
- BP1 - cloudfrount
  - web application delivery at the ede
  - protect from ddos common attacks (syn floods, udp reflection,....)
- BP1 - Global Accelerator
  - access your application from the edge
  - integration with shield for DDoS protection
  - helpful if your backend is not compatible with cloudfront
- BP3 - Route 53
  - domain name resolution at the edge
  - DDoS protection mechanis
## AWS Best Practices for DDoS resiliency best practices for DDos Mitigation
- Infrastracture layer defense (BP1 cloudfront, BP3 route53, BP6 elastic load balancing)
  - protect amazon ec2 against high traffic
  - that includes using global accelerator, route 53, cloudfront, elastic load balancing
- Amazon EC2 with auto scaling (BP7 auto scaling)
  - Helps scale in case of sudden traffic surges including a flash crowd or DDoS attack
- Elastic Load Balancing (BP6 elastic load balancing)
  - Elastic load balancing scales with the traffic increases and will distribute the traffic to many ec2 instances
## AWS Best Practices for DDoS resiliency Application Layer Defense
- detect and filter malicious web requests (BP1 cloudfront, BP2 waf)
  - cloudfront cache static content and serve it from edge locations, protecting your backend
  - aws waf is used on top of cloudfront and application load balancer to filter and block request based on requests signature
  - WAF Rate-based rules can automatically block the IPs of bad actors
  - use managed rules on WAF to block attacks based on IP reputation, or block anonymous IPS
  - cloudfront can block specific geographies
- shield advanced (BP1, BP2, BP6) 
  - shield advanced automatic application layer ddos mitigation and deploy aws waf rules to mitigate layer 7 attacks 
## AWS Best Practices for DDoS resiliency Attack surface reduction
- Obfuscating AWS resources (bp1, bp4, bp6)
  - using cloudfront, API gateway, elastic load balancing to hide your backend resources (lambda functions, ec2 instances)
- Security GRoups and network ACLs (bp5 VPC)
 - use security groups and network NACLs to filter traffic based on specific IP at the subnet or ENI-level
 - elastic IP are protected by aws shield advanced
- Protecting API endpoints (bp4 amazon api gateway)
  - hide ec2, lambda, elsewhere
  - edge optimized mode or cloudfront + regional mode (more control for ddos)
  - waf + api gateway: burst limits, headers filtering use api keys
## Amazon GuardDuty
- intelligent Threat discovery to protect your AWS account
- uses machine learning algorithms, anomaly detection 3rd party data
- one click to enable (30 days trial), no need to install software
- input data includes 
  - cloudtrail event logs - unusual api calls, unauthorized deployments
    - Cloudtrail management events - create VPC subnet, create trail,..
  - VPC Flow Logs - unusual internal traffic, unusual IP Address
  - DNS logs - compromised EC2 instances sending encoded data within DNS queries
  - optional features - EKS audit logs, RDS & aurora, ebs, lambda, s3 data events...
- can setup eventbridge rules to be notified in case of findings
- eventbridge rules can target AWS lambda or SNS
- can protect against crypto currency attacks (has a dedicated "finding" for it)
## Amazon Inspector
- automated security assessments
- for EC2 instances
  - leveraging the AWS system manager (SSM) agent
  - analyze against unintended network accessibility
  - analyze the running OS against known vulnerabilities
- for container images push to Amazon ECR
  - assessment of container images as they are pushed
- for lambda functions
  - identifies software vulnerabilities in function code and package dependencies
  - assessment of functions as they are deployed
- reporting and integration with AWS security hub
- send findings to amazon eventbridge
## What does Amazon inspector evaluate
- Remember only for EC2 instances, container images and lambda functions
- Continuous scanning of the infrastructure, only when needed
- Package vulnerabilities (EC2, ECR & Lambda) - database of CVE
- Network reachability (EC2)
- A risk score is associated with all vulnerabilities for prioritization
## Amazon Macie
- fully managed data security and data privacy service that uses machine learning and pattern matching to discover and protect your sensitive data in AWS
- Macie helps identify and alert you to sensitive data, such as personally identifiable information (PII)
