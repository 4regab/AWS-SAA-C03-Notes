# Day 22 \- March 29

## Mobile Application Architecture
- Serverless restapi:HTTPs, api gateway, lambda, dynamodb
- using cognito to generate temporary credentials to access s3 bucket with restricted policy. APp users can directly access aws resources this way. pattern can be applied to dynamodb, lambda
- caching he reads on dynamodb using DAX
- caching the rest requests at the api gateway level
- security  for authentication and authorzaiton with  cognito
## AWS Hosted website summary
- weve seen static content being distributed using cloudfront with s3
- therest api was serverless, didnt need cognito because public
- we leveraged a global dynamodb table to serve the data global
- (we couldve userd aurora global database)
- we enabled dyanodb streams to trigger a lambda function 
- the lambda function had an IAM role which coudl use SESS
- s3 can trigger sqs/sns/lambda to notify events
## Micro services architecture
- you are free todesign each micro srevce the way you want 
- synchronous aptterns (api gateway, load balancers)
- asynchronous patterns: sqs, kinesis, sns, lambda triggers (s3)
- challenges with micro-services
  - repated overhead for creating each new microservice
  - issues with optimizeing server density/utilization
  - complexity of running multi ersions of multiple microservices
  - proliferation of clinet side code requirements with many sepearate services
- some of the challenges are solved by serverless patterns:
  - api gateay, lambda csale autoamticall pay per usage
  - you can easily clone api, reproduce environments
  - generated client sdk throug hswagger integration for the api gateway