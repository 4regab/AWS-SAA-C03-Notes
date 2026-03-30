# Day 22 - March 29

## Mobile Application Architecture
- Serverless REST API: HTTPS, API Gateway, Lambda, DynamoDB
- Using Cognito to generate temporary credentials to access an S3 bucket with a restricted policy. App users can directly access AWS resources this way. This pattern can be applied to DynamoDB and Lambda.
- Caching reads on DynamoDB using DAX
- Caching REST requests at the API Gateway level
- Security for authentication and authorization with Cognito

## AWS Hosted Website Summary
- We've seen static content being distributed using CloudFront with S3
- The REST API was serverless and didn't need Cognito because it was public
- We leveraged a global DynamoDB table to serve the data globally
- (We could have used Aurora Global Database)
- We enabled DynamoDB Streams to trigger a Lambda function
- The Lambda function had an IAM role which could use SES
- S3 can trigger SQS/SNS/Lambda to notify events

## Microservices Architecture
- You are free to design each microservice the way you want
- Synchronous patterns (API Gateway, load balancers)
- Asynchronous patterns: SQS, Kinesis, SNS, Lambda triggers (S3)
- Challenges with microservices
  - Repeated overhead for creating each new microservice
  - Issues with optimizing server density/utilization
  - Complexity of running multiple versions of multiple microservices
  - Proliferation of client-side code requirements with many separate services
- Some of the challenges are solved by serverless patterns:
  - API Gateway and Lambda scale automatically and pay per usage
  - You can easily clone APIs and reproduce environments
  - Generated client SDK through Swagger integration for the API Gateway
