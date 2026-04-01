# Day 25 - April 1

## AWS QuickSight
- serverless machine learning powered business intelligence service to create interactive dashboards
- fast automatically scalable, embeddable with perssssion pricing
- usecases: business analytics, building visualiziations, perform ad-hox analysis, get business insights using data
- integrated with rds, aurora, athena, redshift, s3
- in memory computation using spice engine if data is imported int o quicksight
- enterprise editionL possibly to setupp column level security

## AWS Glue
- managed extract, transform, and load (ETL) service
- fully serverless data integration platform
- discover, catalog, clean, enrich, and move data between data stores
- supports both code-based and visual ETL development
- automatically generates and runs Apache Spark code
- Glue Job Bookmarks: prevent reprocessing old data
- Glue Databrew: clean and normalize data using pre-built transformation
- glue studio: new GUI to create, run and monitor etl jobs in glue
- Glue Streaming ETL: (builto n apache spark strucutredstreaming): compatible with kinesis data streaming, kafka, MSK (managed kafka)

## Amazon Lake formation
- Data lake central place to have all your data for analytics purposes
- Fully managed service that makes it easy to setup a data lake in days
- discover, cleans, transform, and ingest data into your data lake
- it automates many complex manual stesps (collecting, cleansing, moving, cataloging data,...) and deduplication (using ml transforms)
- combine structured and unstructured data in the data lake
- out for hte box source blueprints: s3, rds, relational and no sqldb
- fine grained access control for your applications (row and column level)
- built on top of AWS glue
## Amazon Managed Service for Apache Flink
- Previously named: Kinesis Data Analytics
- Fully managed service for running Apache Flink applications
- FLink (java, scala,  or SQL) is a framework for processing data streams
- Run any apache flink application on a managed cluster on AWS
  - provisioned compute resources, parallel computation, automatic scaling
  - application backups (implemented as checkpoints and snapshots)
  - use any apache flink programming features to transform data
  - important: flink does not read from amazon data firehose
## Amazon Managed Service for Apache Kafka (Amazon MSK)
- alternative to amazon kinesis
-  fully managed apache kafka on AWS
  - allows you to CRUD clusters
  - msk creates and manages kafka brokers nodes & zookeeper nodes for you
  - deploy the msk cluster in your vpc, multi-AZ (up to 3 for HA)
  - auto recovery from common apache kafka failures
  - data is stored on ebs volumes for as long as you want
  - msk serverless
    - run apache kafka on MSK without managing the capacity
    - msk automatically provisions resources and scales compute and storage
## Kinesis data streams vs amazon msk
- Kinesis data streams
  - 1mb message size limit
  - data streams with shards
  - shard splitting and merging
  - tls in flight encryption
  - kms at rest encryption
- Amazon msk
  - 1mb default configure for higher ex10mb
  - kafka topics with partitions
  - can only add partitions to a topic
  - plaintext or tls in flight encryption
  - kms at rest encryption
## Big Data Ingestion Pipeline Discussion
- IoT core allows you to harvest data from IoT devices
- kinesis is great for realtime data collection
- firehose helps with data delivery to s3 in near realtime (1min)
- lambda can help firehose with data transformations amazon s3 can trigger notifications to sqs
- lambda can subscribe to sqs (we could have connector s3 to lambda)
- athena is a serverless sql device and results are stored in s3
- the reporting bucket contains analyzed data and can be used by reporting tool such as aws quicksight, redshift, etc..
