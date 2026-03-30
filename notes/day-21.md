# Day 21 - March 28

## Serverless in AWS
- AWS Lambda
- DynamoDB
- AWS Cognito
- AWS API Gateway
- Amazon S3
- Amazon SNS & SQS
- AWS Kinesis Data Firehose
- Aurora Serverless
- Step Functions
- Fargate

## Lambda Overview
- Virtual functions - no servers to manage
- Limited by time - short execution duration (max 15 minutes)
- Run on demand
- Scaling is automated

## Benefits of AWS Lambda
- Easy pricing
  - Pay per request and compute time
  - Free tier of 1M AWS Lambda requests and 400k GBs of compute time
- Integrated with the whole AWS suite of services
- Integrated with many programming languages
- Easy monitoring through AWS CloudWatch
- Easy to get more resources per function (up to 10 GB of RAM)
- Increasing RAM will also improve CPU and network performance

## AWS Lambda Integrations: Main Ones
- API Gateway
- AWS S3
- AWS DynamoDB
- AWS SNS
- AWS SQS
- CloudWatch Events / EventBridge
- CloudWatch Logs
- Cognito
- CloudFront

## AWS Lambda Limits - Per Region
- Execution time: 15 minutes
- Max memory: 10 GB
- Environment variables: 4 KB
- Disk capacity: 512 MB to 10 GB
- Concurrent executions: 1000 (soft limit, can be increased)
- Deployment package size: 50 MB (zipped), 250 MB (unzipped)
- Size of uncompressed code and dependencies: 250 MB
- Can use the /tmp directory to load other files at startup

## Concurrency and Async Invocations
- Concurrency: number of executions that can happen at the same time
- Async invocations: the caller does not wait for the function to complete, and Lambda retries
- If the function doesn't have enough concurrency, it will be throttled and the caller will get a throttling error (429)
- For throttling errors (429) and system errors (5xx), Lambda will return the event to the queue and attempt to run the function again for up to 6 hours

## Cold Starts and Provisioned Concurrency
- Cold start
  - New instance => code is loaded and code outside the handler runs (init)
  - If the init is large, this process takes time
  - The first request served by new instances has higher latency than the rest
- Provisioned concurrency
  - Concurrency is allocated before the function is invoked (in advance)
  - So the cold start never happens and all invocations have low latency
  - Application Auto Scaling can manage concurrency (schedule or target utilization)

## Lambda SnapStart
- Lambda SnapStart is a feature that reduces the cold start time of Java functions by initializing the execution environment and caching the initialized state
- Improves your Lambda function performance up to 10x at no extra cost
- When enabled, the function is invoked from a pre-initialized state, so the cold start is reduced by up to 10x
- When you publish a new version of your function, Lambda initializes the execution environment and caches the initialized state. When a function is invoked, Lambda creates a new execution environment from the cached state, which reduces the cold start time.

## Customization at the Edge
- Many modern applications execute some form of logic at the edge
- Edge function
  - Code that you write and attach to CloudFront distributions
  - Runs close to your users to minimize latency
- CloudFront provides two types: CloudFront Functions and Lambda@Edge
- You don't have to manage any servers; it is deployed globally
- Use case: customize CDN content
- Pay only for what you use
- Fully serverless

## CloudFront Functions & Lambda@Edge Use Cases
- Website security and privacy
- Dynamic web application at the edge
- Search engine optimization (SEO)
- Intelligently route across origins and data centers
- Bot mitigation at the edge
- Real-time image transformation
- A/B testing
- User authentication and authorization
- User prioritization
- User tracking and analytics

## Lambda in VPC
- By default, your function is launched outside your own VPC (in AWS-owned VPC)
- Therefore, it cannot access resources in your VPC (RDS, ElastiCache, EC2)
- You must define the VPC ID, the subnets, and the security groups
- Lambda will create an ENI (Elastic Network Interface) in your subnets

## Lambda with RDS Proxy
- If Lambda functions directly access your database, they may open too many connections under high load, which can overwhelm the database
- RDS Proxy
  - Improve scalability of your database by pooling and sharing database connections
  - Improve availability by reducing failover time by 66% and preserving connections
  - Improve security by enforcing IAM authentication and storing credentials in AWS Secrets Manager
- The Lambda function must be deployed in your VPC because RDS Proxy is never publicly accessible

## Invoking Lambda from RDS Aurora
- Invoke Lambda functions directly from your DB instance
- Allows you to process data events within a database
- Supported for RDS for PostgreSQL and Aurora MySQL
- Must allow outbound traffic to your Lambda function from within your DB instance (public, NAT, GW, VPC endpoints)
- DB instance must have the required permissions to invoke the Lambda function (Lambda resource-based policy and IAM policy)

## RDS Event Notifications
- Notifications that provide information about the DB instance itself (created, stopped, started)
- You don't have any information about the data itself
- Subscribe to the following event categories: DB instance, DB snapshot, DB parameter group, DB security group, RDS Proxy, custom engine version
- Near real-time events (up to 5 minutes)
- Send notifications to SNS or subscribe to events using EventBridge

## AWS DynamoDB
- Fully managed, highly available with replication across multiple AZs
- NoSQL database, not a relational database, with transaction support
- Scales to massive workloads; distributed database
- Millions of requests per second, trillions of rows, 100s of TB of storage
- Fast and consistent in performance (single-digit ms)
- Integrated with IAM for security
- Low cost and auto scaling
- No maintenance or patching; always available
- Standard and infrequent access (IA) table class
- DynamoDB is made of tables
- Each table has a primary key (must be decided at creation time)
- Each table can have an infinite number of items (rows)
- Each item has attributes
- Maximum size of an item is 400 KB
- Data types supported are scalar types, document types, and set types
- Therefore, in DynamoDB, you can rapidly evolve schemas

## DynamoDB Read/Write Capacity Modes
- Provisioned mode (default)
  - You pay for a specific number of read/write operations per second
  - You need to plan the capacity beforehand
  - Possibility of auto scaling mode for RCU and WCU
- On-demand mode read/write operations automatically scale
- No capacity planning needed
- Pay for what you use; expensive
- Great for unpredictable workloads

## DynamoDB - Stream Processing
- Ordered stream of item-level modifications (CRUD) in a table
- Use cases: react to changes in real time (welcome mail), real-time usage analytics, insert into derivable tables, cross-region replication, invoke AWS Lambda
- 24-hour retention
- Limited number of consumers
- Processing AWS Lambda triggers or DynamoDB Stream Kinesis adapter

## DynamoDB Global Tables
- Make a DynamoDB table accessible with low latency in multiple regions
- Active-active replication
- Applications can read and write to the table in any region
- Must enable DynamoDB Streams as a prerequisite

## DynamoDB - Time to Live (TTL)
- Automatically delete items after an expiry timestamp
- Use cases: reduce stored data by keeping only current items, adhere to regulatory obligations, web session handling

## DynamoDB - Backups for Disaster Recovery
- Continuous backups using point-in-time recovery (PITR)
  - Optionally enabled for the last 35 days
  - Point-in-time recovery to any time within the backup window
  - The recovery process creates a new table
- On-demand backups
  - Full backups for long-term retention until explicitly deleted
  - Doesn't affect performance or latency
  - Can be configured and managed in AWS Backup (enable cross-Region copy)

## DynamoDB Integration with AWS S3
- Export to S3 must enable PITR
  - Works for any point in time in the last 35 days
  - Doesn't affect the read capacity of your table
  - Perform data analysis on top of DynamoDB
  - Retain snapshots for auditing
  - ETL on top of S3 data before importing back into DynamoDB
  - Export in DynamoDB JSON or Ion format
- Import from S3
  - Import CSV, DynamoDB JSON, or Ion format
  - Doesn't consume any write capacity

## AWS API Gateway
- AWS Lambda + API Gateway: no infra to manage
- Support for the WebSocket protocol
- Handle different environments (dev, test, prod)
- Handle security (authentication and authorization)
- Create API keys, handle request throttling
- Swagger/OpenAPI import to quickly define APIs
- Transform and validate requests and responses
- Generate SDKs and API specifications
- Cache API responses

## API Gateway Integrations: High Level
- Lambda function
- HTTP
- AWS service

## API Gateway - Endpoint Types
- Edge optimized (default): for global clients
  - Requests are routed to the CloudFront edge locations (improves latency)
  - The API Gateway still lives in only one region
- Regional
  - For clients within the same region
  - Could manually combine with CloudFront for more control over caching strategies and distribution
- Private
  - Can only be accessed from your VPC using an interface VPC endpoint (ENI)

## API Gateway Security
- User authentication through
  - IAM roles
  - Cognito
  - Custom authorizer
- Custom domain name HTTPS security through integration with AWS Certificate Manager (ACM)

## AWS Step Functions
- Build serverless visual workflows to orchestrate your Lambda functions
- Features: sequence, parallel, conditions, timeouts, error handling...
- Can integrate with EC2, ECS, on-premises servers, API Gateway, SQS queues, etc.
- Possibility of implementing a human approval feature
- Use cases: order fulfillment, data processing, web applications, any workflow

## Amazon Cognito
- Give users an identity to interact with our web or mobile app
  - Cognito user pools: sign-in functionality for app users, integrate with API Gateway and Application Load Balancer
- Cognito identity pools (federation identity)
  - Provide AWS credentials to users so they can access AWS resources directly
  - Integrate with Cognito user pools as an identity provider
- Cognito vs IAM: "hundreds of users"

## Cognito User Pools - User Features
- Create a serverless database of users for your web and mobile apps
- Simple login: username (or email) and password combination
- Password reset
- Email and phone number verification
- Multi-factor authentication (MFA)
