# Day 19 \- March 26

## SNS + SQS Fanout
- push once in SNSs, receive in all sqs queues that are subscribers
- fully decooupeld no datal oss
- SQS allows for: data persistence, delayed processing and retries of work.
- ability to add more SQS subscribers over time
- Make sure your SQS queue acess poilbicy allows for SNS to write
- cross region delivery: works with SQS Queues in other regions
## S3 events to multiple queues
- for the same combination of: event type (e.g. object created), prefix (e.g. images/), suffix (e.g. .jpg) you can only have one s3 event rule
- if you want to send the same s3 event to many sqs queues use fan-out
## Amazon Kinises Data Streams
- collec and store streaming data in real-time
- retention up to 365 days
- ability to reprocess replay data by consumers
- data cant be deleted from kinises until it expires
- data up to 10MiB (typical use case is a lot of "small" real-time data)
- data ordering guarantee for data with same "partition ID"
- at-rest KMS encryption, in flight HTTPS encryption
- Kinises Producer library kpl to write an optoimized producer application
## kineses data stream -capacity modes
- provisioned: specify the number of "shards" (units of capacity) you want to use, each shard can handle up to 1MiB/s of data input and 2MiB/s of data output, and up to 1000 records/s. scale manually or decrease the number of shards, pay per shard provisioned per hour
- on-demand: no need to specify the number of shards, automatically scales up and down based on the data volume, pay per data volume in/out per GB
## Amazon data firehose
- use top be called as kineses data firehose
- fully managed service
  - amazon redshift, amazon s3, amazon opensearch service
  - 3rd party: splunk/ mongo db/  datadog / newrelic
  - custopm HTTP endpoint
- Automatic scaling serverless pay for what you use
- near realtime service with buffering capability based on size or time
- supports csv, json, parquet, avro formats
- custom data transformation
## Kinesis Data Streams vs Amazon data firehose
- Kinises Data Streams
  - streaming data collection
  - producer & consumber code
  - realtime
  - provised/ondemand
  - 365 days storage
  - replay capability
- Amazon data firehose
  -  Load streaming data into S3, redshift, opensearch, splunk, mongo db, datadog, newrelic or custom HTTP endpoint
  - fully managed
  - near realtime with buffering
  - automatic scaling
  - no data storage or replay capability
  - doesnt support replay capability
## Amazon MQ
- SQS, SNS are cloud native serivices: proprietary protocols from AWS 
- traditional applications running from on presmises may use open protocols such as: MQTT, AMQP, STOMP, Openwire, WSS
- Amazon MQ is a managed message broker service for Apache ActiveMQ and RabbitMQ that supports these open protocols
- amazon mqdoesnt scale
- amazon mq runs on servers  can run in multi az with failover
- amazon mq has both queue and topic feautres