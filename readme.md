# Intended Learning Strategy

This repo is a concise summary and replacement of the [AWS Lambda](https://acloud.guru/learn/aws-lambda) tutorial by "A Cloud Guru".

Read this repo linearly from top to bottom. You do not need to access the above course to learn AWS Lambda. However, the course link is provided just in case you need more information about a topic, or if you need video examples (such as how to hook up AWS Kinesis to Lambda)

# Section 1

### What is Serverless?

- __What other components will be needed for a full web app? (according to this video)__ - (3:12)
  - Lambda - Functions as a Service (compute)
  - API Gateway - HTTP endpoints as a Service (connectivity)
  - DynamoDB - Managed NoSQL (storage)
  - S3 - Managed object store (storage)
  - Cognito - User management & authentication (security)
  - Certificate Manager - free, automatic SSL certs (security)

### Why Lambda

- __What are some use cases?__
  - [ETL](https://www.webopedia.com/TERM/E/ETL.html) jobs - when you extract data, you have to transform it, and load it into another data source. So you can take data that's being put into S3 and loading it into a Redshift database. Or, taking data that's coming in via HTTP calls and putting them into a SQL database.
  - APIs - Creating Rest APIs with API gateway.
  - Mobile backends - Lambda will do the "compute" part (either AWS mobile services, or API Gateway, or directly invoking from the SDK)
  - Infrastructure automation - Reacting to events. If an instance is going to be retired, your Lambda can react and remediate that automatically
  - Data validation - Since Lambda can listen to events from DynamoDB and Aurora, Lambda can do data validation. You can let data be written to a database, and Lambda can pick that up (via a trigger) and can validate an address is correctly formatted.
  - Security remediation - Some AWS Services are built around Lambda functions. AWS Config lets you supply custom Lambda functions to remediate security and configuration issues that AWS Config finds.

# Section 2

### Create the Function

- __How increase memory/CPU/network bandwidth in Lambda?__ - Increasing memory also increases CPU and network bandwidth (along width cost). If you need more memory to finish something, increasing memory can actually save you money by having the Lambda finish faster.
- __How get more Cores for your Lambda?__ - go above 1500MB of memory and it's automatically 2 Cores. This helps in parallel processing.

### Testing and Logging
- __Where are Lambda logs saved?__ - Cloudwatch.
- __Are logs saved automatically?__ - Yes.


# Section 3

### Updating Lambda Functions with the AWS CLI

- __What is `context` variable in function parameters?__ - lets us communicate with the Lambda runtime.

### Function Versions and Qualifiers

- __What is a Lambda alias?__ - a pointer to a specific version of a Lambda function.
- __How can you do A/B testing with Lambda?__ - when creating an alias, you can choose 2 different versions of your Lambda to invoke.

### Function Outputs and Timeouts

- __Where do your Lambda function's returned value(s) get stored?__ - (~0:30) they're not stored, you have to store them yourself.
- __What are 2 things that can cause your Lambda to time out__ - Not enough memory, out of time.
- __What can you do if your job (in Lambda) is too big?__
  1. Break your job into smaller bits, spinning off multiple jobs from 1 job.
  1. Use another service
    1. Fargate - a container runtime that's easier to run than classic ECS since you're not running the container instances yourself.
    1. AWS Batch - aimed at data processing workloads and big data.
    1. S3 Select - pulling values out of structured s3 objects.


# Section 4

### Introduction to Kinesis

- __What is Kinesis?__ - (0:30) an event streaming service. A gigantic log where all events come in as ordered streams by time.
- __What's a Kinesis stream?__ - (1:00) a group of shards that's configurable. You can add/remove shards as needs change.

### Create a Stream and Function Trigger

- __What do shards do?__ - (0:40) They determine stream capacity. Each shard gives you approximately 1 MB/s throughput
- __How are Kinesis payloads encoded?__ (0:50) base-64. You must decode it in your Lambda function.
- __How handle "streaming" data?__ - (2:10) '"Batch size" is how many records we receive at a time. We set batch size to something like 100. In the Lambda, we can loop through these records (looping shown in next video ("Test the Function") at 0:45)
