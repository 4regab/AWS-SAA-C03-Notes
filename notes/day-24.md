# Day 24 - March 31

## Amazon Athena
- Serverless query service to analyze data stored in amazon s3
- uses standard sql language to query the files (built on presto)
- supports CSV, JSON, ORC, Avro, Parquet
- pricing: $5 per tb of data scanned
- commonly used with amazon quicksight for reporting/dashboards
- usecases: business/intelligence/analytics/reporting/analyze & query VPC flow logs, ELB logs, cloudtrails trails etc.
- analyze data in s3 using serverless sql use athena
## Amazon Athena - performance improvement
- use columnar data for cost savings (less scan)
  - apache parquet or ORC is recommended
  - huge performance improvement
  - use glue to convert your data to parquet or ORC
- Compress data foro smaller retrievals (bzip,gzip,lz4, snappy,  zlip, zstd)
- Partition datasets in s3 for easy querying on virtual columns
## Amazon Athena - federated query
- allows you to rrun sql queries across data stored in relation non relational object custom data sources (aws or on premises)
- use data source connecters that run on aws lambda function to run queries (e.g. cloudwatch logs, dynamodb,rds)
## Amazon EMR (Elastic MapReduce) 
- is a managed cloud big data platform that simplifies running large-scale distributed data processing, analytics, and machine learning jobs using open-source frameworks like Apache Spark, Hive, and Hadoop on AWS.
