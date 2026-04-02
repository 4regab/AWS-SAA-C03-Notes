# Day 26 - April 2

## Amazon Rekognition
- Find objects, people, text, scenes in images and videos using ML
- facial analysis and facial search to do user verification, people counting
- create a database of "familiar faces" or compare against celebrities
- Use Cases:
  - Labelling
  - content moderation
  - text detection
  - face search and verification
  - celebrity recognition
  - Pathing
## Amazon rekognition - content moderation
- Detect content that is inappropriate, unwanted, or offensive (images and videos)
- used in social media, broadcast media, advertising and e-commerce situations to create a safer user experience
- set a minimum confidence threshold for items that will be flagged
- flag sensitive content for manual review in augmented AI (A2I)
- help comply with regulations
## Amazon Transcribe
- Automatically convert speech to text
- Uses deep learning process called automatic speech recognition (asr) to convert speech to text quickly and accurately
- automatically remove personally identifiable information (PII) using redaction supports automatic language identification for multi lingual audio
- use cases: transcribe customer service calls
- automate closed captioning and subtitling
- generate metadata for media assets to create a fully searchable archive
## Amazon Polly
- Turn text into lifelike speech
- Uses deep learning to synthesize natural-sounding speech from text
- Supports multiple languages, voices, and speech styles
- use cases: create interactive voice applications, automated phone systems, audiobooks, and accessibility solutions
- enables developers to add speech output to applications without requiring audio recording
## Amazon Polly - lexicon and ssml
- customize the pronounciation of words with pronounciation lexicons
  - stylized words: st3ph4ane => "Stephane"
  - acronyms: AWS => "amazon web services"
- Upload the lexicons and use them in the synthesizespeech operation
- generate a speech from plain text or from documents marked up with speech synthesis markup language (ssml) - enables more customization
  - emphasizing speecific words or phrases
  - using phonetic pronounciation
  - including breathing sounds, whispering
  - using the newscaster speaking style
## Amazon Translate
- Natural and accurate language translation
- amazon translate allows you to localize content - such as websites and applications for international users and to easily translate large volumes of text efficiently
## Amazon Lex & Connect
- amazon lex: (same tech that powers alexa)
  - auto speech recognition (asr)to convert speech to text
  - natural language understanding to recognize the intent of text, callers
  - helps build chatbots, call center bots
- amazon connect: 
- receive calls, create contact flows, cloud baed virtual contact center
- can integrate with other CRM systems over AWS
- No upfront payments, 80% cheaper than traditional contact center solutions
## Amazon comprehend
- for natural language processing (NLP)
- fully managed and serverless service
- uses machine learning to find insights and relationships in text
  - language of the text
  - extracts key phrases, places, people, brands, or events
  - understand how positive or negative the text is
  - analyzes text using tokenization and parts of speech
  - automatically organizes a collection of tet files by topic
  - sample use cases:
    - analyze customer interactions(emails): to find what leads to a positive or negative experience
    - create and groups articile by topics that comprehend will uncover
## Amazon Comprehend Medical
- Amazon comprehend medical detects and returns useful information in unstructured clinical text:
  - physician's notes
  - discharge summaries
  - test results
  - case notes
- Uses NLP to detect protected health information (PHI) - DetectPHI API
- Store your documents in Amazon s3, analyze realtime data with kinesis data firehose or use amazon transcribe to transcribe patients narratives into text that can be analyzed by amazon comprehend medical
## Amazon Sagemaker AI
- fully managed service for developers/data scientists to build ML modeles
- typically difficult to do all the processes in one place + provision servers
- machine learning process (simplified): predicting your exam score
## Amazon kendra
- fully managed document search service powered by machine learning
- extract answers from within a document (text, pdf, html,powerpoint, ms word, faqs)
- natural language search capabilities
- learn from user interactions/feedback to promote preferred results (incremental learning)
- Ability to manually fine-tune results (importance of data, frensess, custom,...)
## Amazon Personalize
- Fully managed ML-service to build apps with real-time personalized recommendations
- Same technology used by amazon.com
- integrates into existing websties, applications, sms, email marketing systems, ...
- implement in dsys not months ( you dont need to build, train and deploy ml solutions)
- use cases: retail stores, media and entertainment...
## Amazon Extract
- automatically extracts texts, handwriting, and data from any scanned documents using ai and ml
- extract data from forms and tables
- read and process any types of documents (pdfs, images..)
- use cases:
  - financial services
  - healthcare
  - public sector
## AWS machine learning summary
- Rekognition: face detection, labeling, celebrity recognition
- transcribe: audio to text
- polly: text to audio
- translate: translations
- lex: build conversational bots - chatbots
- connect: cloud contact center
- comprehend: natural language processing
- sagemaker: machine learning for every developer and data scientist
- kendra: ML powered search engine
- personalize: real-time personalized recommendations
- textract: detect text and data in documents
## AWS Monitoring - Cloudwatch
- Continually stream cloudwatch metrics to a destination of your choice, with near realtime delivery and low latency.
  - amazon kinesis data firehose (and then its destinations)
  - 3rd party service prvoder: datadog, dynatrace, new relic, splunk, sumo logic...
- option to filter metrics to only stream a subset of them
## Cloudwatch logs
- log groups: arbitrary name, usually representing an application
- log stream: instances within application/log files/ containers
- can define log expiration policies (never expire , 1 day to 10 years)
- cloudwatch logs can send logs to:
  - amazon s3 (exports)
  - kinesis data streams
  - kinesis data firehose
  - aws lambda
  - opensearch
- Logs are encrypted by default
- can setup kms-based encryption with your own keys
## Cloudwatch logs sources
- SDK, Cloudwatch logs agent, cloudwatch unified agent
- elastic beanstalk: collections of logs from application
- ecs: collection from containers
- aws lambda: collection from funtion logs
- vpc flow logs: vpc specific logs
- api gateway
- cloudtrail based on filter
## CloudWatch logs insights
- search and analyze log data stored in cloudwatch logs
- example: find a specific IP inside a log, count occurences of "ERROR" in your logs....
- provides a purpose built query language
  - automatically discovers fields from aws services and josn log events
  - fetch desired event fields, filter based on conditions, calculate aggregate statistics, sort events, limit number of events..
  - can save queries and add them to cloudwatch dashboards
- Can query multiple log groups in differen aws accounts
- it's a query engine not real time engine
## CloudWatch logs subscriptions
- get arealtime log events from cloudwatch logs for processing and analysis
- send to kinesis data streams, kinesis data fire hose or lambda
- subscription filter - filter which logs are events delivered to your destination
## Cloudwatch Alarms
- alarms are used to trigger notifications for any metric
- various options (sampling, %, max, min, etc..)
- alarms states:
  - ok
  - insufficient_data
- Period:
  - length of time in seconds to evaluate the metric
  - high resolution custom metrics: 10sec, 30 sec or multiple of 60 sec
## CloudWatch Alarm Targets
- stop, terminate, reboot or recover an ec2 instance 
- trigger auto scaling action
- send notification to sns (from which you can do pretty much anything)
## CloudWatch Alarms - composite Alarms
- CloudWatch alarms are on a single metric
- composite alarms are monitoring the states of multiple other alarms
- AND and OR conditions
- helpful to reduce "alarm noise" by creating complex composite alarms
## EC2 instance recovery
- status check:
  - instance status = check the ec2 vm
  - system status = check the underlying hardware
  - attached ebs status = check the attached ebs volumes
- recovery: same private, public, elastic ip, metadata, placement group
## CLoudWatach alarm
- alarms can be created basedd on cloudwatch logs metrics filters
- to test alarms and notifications aset the alarm state toa alarm using cli `aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM --state-reason "testing"`
## CloudWatch Network Synthethic Monitor
- monitor and detects network issues between your apps hosted on AWS and your premises data center
- identify any network performacne degradation (eg.g packets loss, latency, jitter)
- no agents requred to be installed
- tests icmp, or tcp to ipv4/ipv4 on premises destinations through direct connect or s2s vpn connections
- publishes data to cloudwatch metrics
## Amazon EventBridge
- schedule cronjobs (scheduled scripts)
- event pattern: event rules to react to a service doing something
- trigger lambda funcitons, send sqs/sns messages
- event busses can be accessed by other aws accounts using resource based policies
- you can archive events (all/filter) sent to an event bus (indefinitely or set period)
- ability to replay archived events
## Amazon Eventbridge - schema registry
- eventbridge can analyze the events in your bus and infer the schema
- the schema registry allows you to generated code for your application that will know in advance how data is structured in the event bus
- schema can be versioned
## Amazon eventbridge resourece based policy
- managed permissions for a specific event bus
- example: allow/deny events  from another aws account or aws region
- use case: aggregate all events from your aws organization in a single aws account or aws region
## cloudwatch container insights
- collect, aggregate, summarize metrics and logs from containers
- available for containers on 
  - amazon elastic container service (Amazon ECS)
  - amazon elastic kubernetes services (amazon eks)
  - kubernetes platforms on ec2
  - fargate (both for ecs and eks)
- In amazon eks and kubernetes, cloudwatch insights is using a containerized version of the cloudwatch agent to discover containers
## Cloudwatch lambda insights
- monitoring andtrouble shooting solution for servelss applications running on aws lambda
- collect, aggregates and summarizes system-level metrics including cpu time, memory disk, and network
- collects, aggregates, and summarizes diagnostic information such as cold starts and lambda worker shutdowns
- lambda insights is provided as lambda layer
## Cloudwatch Contributor insights
- analyze log data and create time series that display contributor data
  - see metrics about the top-n contributors
  - the total number of unique contributors and their usage
- this helps you find top talkers and understand who or what is impacting system performance
- works for any aws-generated logs (vpc, dns, etc..)
- for example, you can find bad hosts, identify heaviest network users, or find the urls that generate most errors
- you can build your rules from scratch or you can do also use sample rules that aws has created - leverages your cloudwatch logs
- cloudwatch also provides built-in rules that you can use to analyze metrics from other aws services
## Cloudwatch application insights
- provides automated dashboards that show potential problems with monitored applications to help isolate ongoing issues
- your application run on amazon ec2 instances with select technologies only (java, .net, microsoft IIS webserver, databases...)
- and you can use other aws resources such as amazon ebs, rds, elb, asg, lambda, sqs, dynamodb,s3 bucket, ecs, eks, sns, api gateway
- powered by sage maker
- enhanced visibility into your application health to reduce the time it will take you to trouble shoot and repair your applications
- findings and alerts are sent to amazon eventebridge and ssm opscenter
## Cloudwatch insights and operational visibility
- cloudwatch container insights
  - evs, eks, kubernetes on ec2, fargate, needs agent for kubernetes
  - metric and logs
- cloudwatch lambda insights
  - detailed metrics to troubleshoot serverless applications
- cloudwatch contributor insights
  - find "top-n" contributor through cloudwatch logs
  - cloudwatch application insights
    - automatic dashboard to throubleshoot your application and related aws services
## AWS CloudTrail
- provides governance, compliance and audit for your aws account
- cloudtrail is enabled by default
- get an history of events/api calls made within your aws account by:
  - console, sdk, cli, aws services
- can put logs from cloudtrailt into cloudwatch logs or s3
- a trail can be applied to all regions (default) or a single region.
- if a resource is deleted in aws, investigate cloudtrail first!
## Cloudtrail events
- Management events: 
  - operations that are performed on resources in your AWS account
  - Examples: Configuring security (IAM AttachRolePolicy)
  - configuring rules for routing data (amazon ec2 createsubnet)
  - setting up logging (aws cloudtrail createtrail)
  - By default, trails are configured to log management events.
  - can separate read events (that dont modify resources from write events (that may modify resources)
- Data Events:
  - By Default,data events are not logged (because high volume operations)
  - amazon s3 object-level activity (ex.getObject,deleteobject,putobject)can seperate read and write events
## cloudtrail insights
- enable cloudtrail insights to detect unusual activity in  oyur account
  - inaccurate resource provisioning
  - hitting service limits
  - bursys of aws iam actions
  - gaps in periodic maintenance activity
- cloudtrail insights analyzes normal management events to create a baseline
- and then continuously analyzes write events to detect unusual patterns
  - anomalies appear in the cloudtrail console
  - event is sent to amazon s3
  - an eventbridge event is generated (for automation needs)
## cloudtrail events retention
- events are stored for 90days in cloudtrail
- to keep events beyond this period log them to s3 and use athena
## Amazon Config
- helps with auditing and recording compliance of your aws resources
- helps record configuration and changes overtime
- questions that can be solved by aws config:
  - is there unrestricted ssh access to my security group?
  - do my bucket have any public access?
  - how has my alb configuration changed overtime?
- you can receive alerts (sns notifications) for any changes
- AWs config is per region service
- can be aggregated across regions and accounts
- possibly of storing the configuration data into s3 (analyzed by athena)
## Config Rules
- Can use aws managed config rules (over 75)
- can make custom config rules (must be defined in aws lambda)
  - ex. evaluate if each ebs disk is of type gp2
  - ex. evaluate if each ec2 instance is t2.micro
- rules can be evaluated/triggereed
  - for each config change
  - and / or : at regular time intervals
- AWS config rules does not prevent actions from happening (no deny)
- pricing: no free tier, 0.003$ per configuration item recorded per region, 0.001 per config rule evaluation per region
## Config Rules Remediations
- automate remediation of non-compliant resources using ssm automation documents
- use aws manaed automation documents or create custom automation documents
  - tip: you can create custom automation documents that invokes lambda function
- you can set remediation retries if the resource is still non-compliant after auto remediation
## Config Rules - Notifications
- use event bridge to trigger notifications when aws resources are non-compliant
- ability to send configuration changes and compliance state notifications to sns (all events -use sns filtering or filter at client side)
## Cloudtrail vs cloudwatch vs config
- Cloudwatch
  - performance monitoring (metrics, cpu, network, etc..) & dashboards
  - events & alerting
  - log aggregation and analysis
- CloudTrail
  - record api calls made within your account by everyone
  - can define trails for specific resources
  - global service
- config
  - record configuration changes
  - evaluate resources against compliance rules
  - get timeline of changes and compliance
## Example for an Elastic loadbalancer
- Cloudwatch
  - monitoring incoming connections metric
  - visualize error codes as % over time
  - make a dashboard to get an idea of your load balancer performance
- config:
  - track securuty group rules for the load balancer
  - track configuration changes for the load balancer
  - ensure an ssl vertificate is always assigned to the load balancer (compliance)
- cloudtrail
  - track who made any changes to the load balancer with api calls
