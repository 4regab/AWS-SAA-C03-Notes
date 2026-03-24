# Day 17 \- March 24

## AWS Control Tower 
- is a high-level service offering a straightforward way to set up and govern an AWS multi-account environment, following prescriptive best practices. AWS Control Tower orchestrates the capabilities of several other AWS services, including AWS Organizations, AWS Service Catalog, and AWS IAM Identity Center, to build a landing zone in less than an hour.
## AWS Lambda 
- is a compute service that lets you run code without provisioning or managing servers.
- Lambda runs your code on a high-availability compute infrastructure and performs all of the administration of the compute resources, including server and operating system maintenance, capacity provisioning and automatic scaling and logging. With Lambda, all you need to do is supply your code in one of the language runtimes that Lambda supports.
- With Lambda SnapStart for Java, Lambda initializes functions as new versions are published. Lambda then takes a Firecracker microVM snapshot of the memory and disk state of the initialized execution environment, encrypts the snapshot, and caches it for low-latency access.
## Amazon Elastic Kubernetes Service (Amazon EKS) 
- is a managed service that eliminates the need to install, operate, and maintain your own Kubernetes control plane on Amazon Web Services (AWS).
## An Amazon EKS add-on 
- is software that provides supporting operational capabilities to Kubernetes applications but is not specific to the application. This includes software like observability agents or Kubernetes drivers that allow the cluster to interact with underlying AWS resources for networking, compute, and storage. For instance, the community-initiated 
## AWS Load Balancer Controller 
- simplifies the managing and provisioning of load balancing resources on AWS, initially conceived for Ingress-related load balancing and later expanded to include Service-related load balancing, i.e., L4 and NLB. 
## AWS Elastic Load Balancing  
- automatically distributes your incoming traffic across multiple targets, such as EC2 instances, containers, and IP addresses, in one or more Availability Zones. It monitors the health of its registered targets, and routes traffic only to the healthy targets. Elastic Load Balancing scales your load balancer as your incoming traffic changes over time. It can automatically scale to the vast majority of workloads.
## Amazon Simple Queue Service (Amazon SQS) 
- offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components. Amazon SQS offers common constructs such as dead-letter queues and cost allocation tags.
- Amazon SQS provides a generic web services API that you can access using any programming language supported by AWS SDK. A single subscriber typically processes messages in the queue. This does not necessarily mean a single consumer consuming the whole queue serially. For example, there could be concurrent executions of an AWS Lambda function, each consuming a different queue item. The important thing is that queue items are usually consumed only once. In some use cases, several subscribers need to act on the same item; Amazon SQS and Amazon SNS are often used together to create a fanout messaging application in such scenarios.
## Amazon SNS 
- is a publish-subscribe service that provides message delivery from publishers (also known as producers) to multiple subscriber endpoints(also known as consumers). Publishers communicate asynchronously with subscribers by sending messages to a topic, which is a logical access point and communication channel. Subscribers can subscribe to an Amazon SNS topic and receive published messages using a supported endpoint type, such as Amazon Data Firehose, Amazon SQS, Lambda, HTTP, email, mobile push notifications, and mobile text messages (SMS). Amazon SNS acts as a message router and delivers messages to subscribers in real-time. If a subscriber is not available at the time of message publication, the message is not stored for later retrieval.
## Amazon Aurora 
- is a relational database service that combines the speed and availability of high-end commercial databases with the simplicity and cost-effectiveness of open-source databases. Aurora is fully compatible with MySQL and PostgreSQL, allowing existing applications and tools to run without requiring modification.
## Amazon Aurora Serverless v2
- With Aurora Serverless V2, you can mix and match provisioned capacity writer/reader instances with serverless writer/reader that match your workloads. Taking an example of a read-heavy but consistent workload with erratic writes, the cluster can be configured to include provisioned reader instance/s and a serverless writer.
