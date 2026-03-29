# Day 21 \- March 28

## server less in AWS
- AWS Lambda
- DynamoDB
- AWS Cognito
- AWS API Gateway
- Amazon s3
- Amazon SNS & SQS
- AWS Kinesis Data Firehose
- Aurora Serverless
- Step Functions
- Fargate
## lambda Overview
- Virtual functions  - no servers to maange
- Limited by time - short execution duration (max 15 minutes)
- Run on demand
- Scaling is automated
## Benefits of AWS Lamnda
- Easy pricing
  - pay per request and compute time
  - free tier of 1M aws lambda requests and 400k GBs compute time
- Integrated with the whole AWS suite of services
- integrated with many progamming lanugases 
- easy monitoring thorugh aws cloudwtch
- easy to get more resources per functions (up to 10gb of ram)
- Increasing ram will also improve CPU and network!
## AWS Lambda Integrations Main ones
- APi Gateway
- AWS S3
- AWS DynamoDB
- AWS SNS
- AWS SQS
- CloudWatch Events Event Bridge
- CloudWatch Logs
- Cognito
- Cloudfront
## AWS Lambda limits - per region
- Execution time: 15 minutes
- Max memory: 10 GB
- Evironment vrialbles: 4 KB
- Disk Capacity: 512 MB to 10gb
- Concurrent executions: 1000 (soft limit, can be increased).
- Deployment package size: 50 MB (zipped), 250 MB (unzipped)
- Size of uncompressed code and dependencies: 250 MB
- Can use the /tmp directory to load other files at startup
- Size of environment variables: 4 KB
## Concurrency and Async Invocations
- Concurrency: number of executions that can happen at the same time
- Async invocations: the caller does not wait for the function to complete, and Lambda retries
- If the function doesnt have enough concurrency, it will be throttled and the caller will get a throttling error (429)
- For throttling errors (429) and system errors (5xx), Lambda will return the event to the queue and atttempts to run the function again for up to 6 hours
## Cold starts and provisioned concurrency
- Cold start 
  - new instance => code is loaded and ccode outside the handler run (init)
  - if the init is large this process take time
  - first request served by new instances has higher latency than the rest
- Provisioned concurrency
  - COncurrency is allocated before the function is invoked (in advanced)
  - so the cold start never happens and all invocations have low latency
  - application auto scaling can manage concurrency (schedule or target utilization)
## Lambda Snapstart
- Lambda Snapstart is a feature that reduces the cold start time of Java functions by initializing the execution environment and caching the initialized state
- improves your lambda funtion performance up to 10x at no extra cost
- when enabled, function is invoked from pre initialized state, so the cold start is reduced by up to 10x
- when you publish a new version of your function, Lambda initializes the execution environment and caches the initialized state. When a function is invoked, Lambda creates a new execution environment from the cached state, which reduces the cold start time.
## Customization at the EDge
- many modern execute some form of the logic at the edge
- Edge function
  - a code that you write and attach to cloudfront distributions
  - runs close to your users to minimize latency
- Cloudfront provides two types: cloudfront functions and lambda@edge
- you dont have to manage any servers, deployed globally
- Usecase: customize the CDN content
- pay only for what you use
- fully serverless
## Cloudfront functions & Lambda@Edge usecases
- website security and privacy
- dybamic web application at the edge
- search engine optimization seo
- intelligently route accross origins and data centers
- bot mitigation at the edge
- real time image rtansformation
- a/b testing 
- User authentication and authorization
- user prioritizaion
- user tracking and analytics
## Lambda in VPC
- By default your funciton is launched outside  your own vpc (in aws ownd vpc)
- therefore it cannot access resoureces in your vpc (rds, elasticache, ec2)
- you must define the vpc id the subnets and the security groups
- lambda will create an ENI (elastic network interface) in your subnets
## Lambda with rds proxy
= if lambda functions directly access your database they may open too many connections under high load, which can overwhelm the database
- RDS Proxy
  - Improve scalability of your database by pooling and sharing database connections
  - improve availability by reducing 66% the failover time and preserving connections
  - improve security by enforcing IAM authentication and storing credentials in AWS Secrets Manager
- The lamdbda function must be deployed in your VPC because rds proxy is never publicly accessible
## Invovking Lambda from rds aurora
- invoke lambda functions directly from your DB instance
- allows oyou to process data events within a database
- supported for RDS for postgre sql and aurora mysql
- must allow outbound traffic to your lambda function from within your db instance (public, NAT, GW,VPC endpoints)
- DB instance must have the required permissions to invoke the lambda function (lambda resource based policy and IAM policy)
## RDS Event notifications
- Notifications that tells information about the DB instance it self (created, stopped, start)
- you dont have any information about the data itself
- subscribe to the following event categories : DB instance, db snapshot, db parameter group, db security group, RDS proxy, custome engine version
- near realtime events (upto 5minutes)
- send notification to sns or subscribe to events using event bridge
## AWS DynamoDB 
- Fully managed, highly available with replication across multiple azs
- nosql database not a relational database with transaction support 
- scales to massive workloads, disrtubuted database
- millions of requests per seconds trillion of row, 100s tb of storage 
- fast and consistent in performanc (single digit ms)
- integrarted with IAM for security
- lowcost and auto scalning
- no maintenance or patching always available
- standard and infreqeuent access(IA) table class
- Dynamo is made of tables
- each table has a primary key (must be decided at creation time)
- each table can have an infinite number of items (rows)
- each item has attributes
- maximum size of an item is 400kb
- data types spported are: scalar types, document types, set types
- therefore in dynamodb you can rapidly evolve schemas
## Dynamodb read/write capacity modes
- Provisioned mode (default)
  - you pay for specific umber of read/writes per sec
  - you need to plan the capacity nbefore hand
  - posssibility of atuo scalining mode for RCU and WCU
- on demand mode read/writes automatically scale
- no capacity planning needed
- pay for what you use expensivee
- great for unpredicatble workloads
## DynamoDB - stream processing
- ordered stream of item level modifcaions (CRUD) in a table
- use cases: react o changes in realtime (welcome mail), realtime usage analytics, insert into derivatible tables, cross region replicaitom, invoke aws lambda
- 24hrs retention
- limited of # consumers
- processing aws lambda triggers or dynamo db stream kinises adapter
## DynamoDB global tables
- make adynamo db table accessible with low latency in multi regions 
- active-active replication
- applications can read and write to the table in any region
- must enable dynamodb streams as prerequisite
## DynamoDB - time to live (TTL)
- automatically delete items after an expiry timestamp
- usecases: reduce stored data by keeping only current items, adhere to regulatory obligations, web session handlng
## DynamoDB - Backups for disaster recovery
- Continuous backups using point-in-time recovery (PITR)
  - optionally enabled for the last 35 days
  - point in time recovery to any time within the backup window
  - the recovery process creates a new table
- on demand backups
  - full backups for long term retention until explicitely deleted
  - doesnt affect performance or latency
  - can be configured and managed in AWS backup (enable cross region copy)
## Dynamodb integration wit AWS S3
- Export to s3 must enable pitr
  - Works for any point of time in the last 35 day
  - doesnt afect the read capacity of your table
  - perform data analysis on top of dynamoDB
  - retain snapshots for auditing
  - ETL on TOP of s3 data before importing back into dynamodb
  - export in dynamodb json or ION format
- Import from s3
  - import csv, dynamodb json, or ION format
  - doesnt consume any write capacity
## AWS API Gateway
- AWs lambda + api gateway: no infra to manage
- support for the websocket protocol
- handle diff evnironments (dev, test, prod)
- Handle security (authentication and authorization)
- create api keys, hadnle request throttling
- swagger/open api import to quickly define apis
- transform and validate requests and responses
- generate sdk and api specifications
- cache api responses
## Api gateway integrations high level
- lambda function
- http
- aws service
## Api gateway - endpoint types
- edge optimized (default): for global clients
  - requests are routed the cloudfrount edge locations (improves latency)
  - the api gateway still lives in only one region
- Regional:
  - for client within the same region
  - could manually combine with cloudfrount(mroe control over the caching strategies and the distribution)
- Private 
  - can only be accessed from your VPC using an interface VPC endpoint (eni)
## Api gateway Security
- User authetnitcaioon thorugh
  - iam roles 
  - cognito
  - custom authorizer
- custom domain name https security through integration with aws certificate manager (ACm)
## AWS step functions
- build serverless visual workflow to orchestrate your lambda functions
- features: sequence, parallel, conditions, timeouts, error handling...
- can integrate with EC2, ecs, on-premises servers, api gateway, sqs queues, etc
- possibility of implementing human approval feature
- use cases: order fulfillment, data processing, web applications, any workflow
## Amazon cognito
- give users an identity to intergact with our web or mob app
  - cognito user pools;sign in functility for app usrs, integrate with api gateway and application load balancer
- cognito identity pools (federation identity)
  - provide aws credentials to users so they can access aws resources directly
  - integrate with cognito user pools as an identity provider
- cognito vs IAM: "hundreds of users"
## Cognito User Pools - User features
- create a serverless database of user for your web and mobile apps
- simple login:username(or email)/password combination
- password reset
- email and phone number verification
- multifactor authentication (MFA)
