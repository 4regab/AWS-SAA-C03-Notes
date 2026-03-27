# Day 19 \- March 26

## SNS + SQS Fanout
- Push once in SNS, receive in all SQS queues that are subscribers.
- Fully decoupled, no data loss.
- SQS allows for data persistence, delayed processing, and retries of work.
- Ability to add more SQS subscribers over time
- Make sure your SQS queue access policy allows SNS to write.
- Cross-Region delivery: works with SQS queues in other Regions
## S3 Events to Multiple Queues
- For the same combination of event type (e.g. object created), prefix (e.g. images/), and suffix (e.g. .jpg), you can only have one S3 event rule.
- If you want to send the same S3 event to many SQS queues, use fan-out.
## Amazon Kinesis Data Streams
- Collect and store streaming data in real time
- Retention up to 365 days
- Ability to reprocess replay data by consumers
- Data cannot be deleted from Kinesis until it expires
- Data up to 10 MiB (typical use case is a lot of "small" real-time data)
- Data ordering guarantee for data with the same partition ID
- At-rest KMS encryption, in-flight HTTPS encryption
- Kinesis Producer Library (KPL) to write an optimized producer application
## Kinesis Data Stream - Capacity Modes
- Provisioned: specify the number of shards (units of capacity) you want to use. Each shard can handle up to 1 MiB/s of data input, 2 MiB/s of data output, and up to 1,000 records/s. Scale manually by increasing or decreasing the number of shards. Pay per shard provisioned per hour.
- On-demand: no need to specify the number of shards; it automatically scales up and down based on the data volume. Pay per data volume in/out per GB.
## Amazon Data Firehose
- Used to be called Kinesis Data Firehose
- Fully managed service
  - Amazon Redshift, Amazon S3, Amazon OpenSearch Service
  - Third party: Splunk, MongoDB, Datadog, New Relic
  - Custom HTTP endpoint
- Automatic scaling, serverless, pay for what you use
- Near real-time service with buffering capability based on size or time
- Supports CSV, JSON, Parquet, and Avro formats
- Custom data transformation
## Kinesis Data Streams vs Amazon Data Firehose
- Kinesis Data Streams
  - Streaming data collection
  - Producer and consumer code
  - Real time
  - Provisioned/on-demand
  - 365 days storage
  - Replay capability
- Amazon Data Firehose
  - Load streaming data into S3, Redshift, OpenSearch, Splunk, MongoDB, Datadog, New Relic, or a custom HTTP endpoint
  - Fully managed
  - Near real time with buffering
  - Automatic scaling
  - No data storage or replay capability
## Amazon MQ
- SQS and SNS are cloud-native services with proprietary protocols from AWS.
- Traditional applications running from on premises may use open protocols such as MQTT, AMQP, STOMP, OpenWire, and WSS.
- Amazon MQ is a managed message broker service for Apache ActiveMQ and RabbitMQ that supports these open protocols.
- Amazon MQ does not scale.
- Amazon MQ runs on servers and can run in Multi-AZ with failover.
- Amazon MQ has both queue and topic features.
