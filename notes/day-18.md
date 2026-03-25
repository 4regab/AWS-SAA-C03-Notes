## day 18 - March 25


Amazon SQS – Standard Queue
• Oldest offering (over 10 years old)
• Fully managed service, used to decouple applications
• Attributes:
• Unlimited throughput, unlimited number of messages in queue
• Default retention of messages: 4 days, maximum of 14 days
• Low latency (<10 ms on publish and receive)
• Limitation of 1,024 KB per message sent
• Can have duplicate messages (at least once delivery, occasionally)
• Can have out of order messages (best effort ordering)

SQS – Producing Messages
• Produced to SQS using the SDK (SendMessage API)
• The message is persisted in SQS until a consumer deletes it
• Message retention: default 4 days, up to 14 days
• Example: send an order to be processed
• Order id
• Customer id
• Any attributes you want
• SQS standard: unlimited throughput
SQS – Consuming Messages
• Consumers (running on EC2 instances, servers, or AWS Lambda)…
• Poll SQS for messages (receive up to 10 messages at a time)
• Process the messages (example: insert the message into an RDS database)
• Delete the messages using the DeleteMessage API
Poll / Receive
messages
insert
Consumer
DeleteMessage

SQS – Multiple EC2 Instances Consumers
SQS Queue
poll
• Consumers receive and process
messages in parallel
• At least once delivery
• Best-effort message ordering
• Consumers delete messages
after processing them
• We can scale consumers
horizontally to improve
throughput of processing

SQS with Auto Scaling Group (ASG)
Poll for messages
EC2 Instances
SQS Queue
Auto Scaling Group
scale
Alarm for breach
CloudWatch Metric – Queue Length
ApproximateNumberOfMessages
CloudWatch Alarm

SQS to decouple between application tiers
Front-end web app
Back-end processing
application
requests SendMessage
ReceiveMessages
SQS Queue
(infinitely scalable)
Auto-Scaling
Auto-Scaling

Amazon SQS - Security
• Encryption:
• In-flight encryption using HTTPS API
• At-rest encryption using KMS keys
• Client-side encryption if the client wants to perform encryption/decryption itself
• Access Controls: IAM policies to regulate access to the SQS API
• SQS Access Policies (similar to S3 bucket policies)
• Useful for cross-account access to SQS queues
• Useful for allowing other services (SNS, S3…) to write to an SQS queue

SQS – Message Visibility Timeout
• After a message is polled by a consumer, it becomes invisible to other consumers
• By default, the “message visibility timeout” is 30 seconds
• That means the message has 30 seconds to be processed
• After the message visibility timeout is over, the message is “visible” in SQS
• If a message is not processed within the visibility timeout, it will be processed twice
• A consumer could call the ChangeMessageVisibility API to get more time
• If visibility timeout is high (hours), and consumer crashes, re-processing will take time
• If visibility timeout is too low (seconds), we may get duplicates
