- AWS Serverless and Application Services
    - 

- Event Driven Architecture
    - no constant running or waiting for things
    - producers generate events when something happens
    - events are delivered to consumers
    - action are taken and the system returns to waiting
    - mature event driven architecture only consumes resources while handling events (serverless environment)

- Lambda
    - Function as a Service (FaaS), short running and focussed
    - Lambda function is a piece of code that lambda runs
    - functions use a runtime (python 3.8)
    - Functions are loaded and run in a runtime environment
    - the environment has a direct memory (indirect CPU) allocation
    - you are billed for the duration that a function runs
    - lambda layers lets you use custom runtimes such as Rust
    - Docker is NOT supported by lambda
    - 900s (15 minutes) is the function timeout, anything longer than 15 minutes of run time can't use Lambda functions
    - Lambda is used for serverless applications, file processing, database triggers, serverless CRON, reatime stream data processing
    - by default, lambda function are on the public network, and can't access private/vpc services unless security controls allow
        - private lambda would be inside the VPC, and obey the same rules as anything else in the VPC
        - this means the Lambda functions can't access anything public unless allowed
    - Lambda runtime needs an execution role (just like an EC2 instance role)
        - this is an IAM role attached to a function that controls the permissions the Lambda function receives
    - Lambda resource policy controls what services and accounts can invoke lambda functions
    - Lambda uses Cloudwatch, Cloudwatch Logs and X-Ray
        - logs from Lambda executions are stored in CloudWatchLogs
        - metrics for success/failure, retries, latency etc are stored in CloudWatch
        - Lambda can be integrated with X-Ray for distributed tracing
        - CloudWatch Logs requires permissions via execution role

    - Synchronous Invocation
        - CLI/API invoke a lambda function, passing in data and waiting for a response
        - lambda function responds to the CLI/API with data or fail
        - errors or tried have to be handled with the client
    - Asynchronous Invocation
        - Typically used when AWS services invoke lambda functions
        - Example, S3 isn't waiting for any kind of response, the event is generated and S3 stops tracking
        - Lambda is responsible for handling any retries during an event failure
        - lambda function needs to be idempotent
            - Example, if someone has $10 and needs $20 in their bank, you can give them +$10 or set their bank to be $20
            - no matter where the process fails, if the function runs multiple times with an end result of $20, the result is the same
        - Events can be sent to dead letter queues after repeated failed processing
        - Lambda supports destinations like SQS, SNS, EventBridge where successful or failed events can be sent
    - Event Sourc Mapping
        - Used on streams or queues which dont support event generation to invoke lambda functions, like Kinesis
        - source batches are sent from Kinesis streams, picked up by event source mapping, and event batches are sent to Lambda
        - batch size has to complete faster than 15 minutes
        - Event Source Mapping gets permissions from the lambda execution role to interact with the event source (Kinesis in this example)
        - DLQ (dead letter queue) can be used to send failed event batches to
    
    - Lambda functions have version, v1 v2 v3
    - Version is the code + the configuration of the lambda function
    - its immutable, never changes once published and has it's own ARN
    - $Latest point to the latest version
    - Aliases (DEV, STAGE, PROD) point at a version and can be changed

    - Lambda startup time
        - Lambda code runs inside a runtime environment (execution context)
        - When a lambda function is invokved, execution context is created
        - deployment package is created which contains the function code
        - a cold start is a full creation and configuration including function code download (100 ms)
        - if there isn't too much of a gap, the next invocation can use the same execution context
            - a new event is passed in but the execution context cration can be skipped (1-2ms)
        - provisioned concurrency can be used. AWS will create and keep X contexts warm and ready to go
        - you can also use the /tmp space to predownload the execution context

- CloudWatch Events and EventBridge
    - EventBridge is replacing CloudWatch Events
    - EventBridge can handle everything CloudWatch Events can + more, including events from 3rd party sources
    - if X happens, or at Y times, do Z
    - a default event bus for the account
        - bus = stream of events that occur from any supported service in that AWS account
        - in CloudWatch Events, there is the only one bus (implicit)
        - in EventBridge you can have additional event busses for third party sources
    - Rules match incoming events or schedules 
    - Route the events to 1 or more targets, such as an event invoking a Lambda function
        - Bob changes EC2 state from stopped to start
        - the state change goes through the event bus and gets passed to EventBridge
        - EventBridge passes the events through the event pattern rule, and then is passed to the target (Lambda)


- Serverless
    - serverless isn't one single thing
    - you manage few, if any server, low overhead
    - applications are a collection of small and specialized functions
    - stateless and ephemeral environment, billed for duration
    - environment is event driven, consumption only when being used
    - FaaS is used where possible for compute functionality
    - managed services are used where possible, like S3, DynamoDB

- Simple Notification Service (SNS)
    - Public AWS Service, network connectivity with public endpoint
    - coordinates the sending and delivery of messages
    - messages are 256KB or less
    - SNS topics are the base entity of SNS, permissions and configuration
    - a Publisher send messages to a topic
    - topics have subscribers which receive messages
    - delivery status, delivery retries, HA and Scalable (Regional service), server side encryption, cross account via topic policy

- AWS Step Functions
    - State Machines
        - Usually used for backend processing
        - Serverless workflow Start > States > End
        - States are THINGS which occur inside the workflow
        - Maximum duration of 1 year
        - Standard Workflow and Express Workflow
        - IAM Role is used for permissions
    
    - States
        - Succeed and Fail
        - Wait
        - Choice
        - Parallel
        - Map
        - Task

- API Gateway
    - Overview
        - service that lets you create and manage APIs
        - its how applications can communicate with eachother
        - acts as an endpoint or entry point for applications
        - sits between applications and integrations
        - highly available and scalable, hansles authorization, throttling, caching, CORS, transformations, OpenAPI spec, direct integrations
        - can connect to services/endpoints in AWS or on-prem
        - HTTP, REST and WebSocket APIs
    
    - Authentication
        - Can use cognito user pools for authentication
        - can also use Lambda Authorization
    
    - Endpoint Types
        - Edge optimized
            - Routed to the nearest CloudFront point of precence 
        - Regional
            - Clients in the same region
        - Private
            - Endpoint accessible only within a VPC via interface endpoint

    - Errors
        - 4XX client error, invalid request on client side
        - 5XX server error, valid request but backend issue
        - 400 bad request, generic client error
        - 403 access denied, authorizer denies
        - 429 API Gateway can throttle, this means youve exceeded the throttled amount
        - 502 bad gateway exception, bad output returned by lambda
        - 503 service unavailable, backing endpoint offline, or major service issue
        - 504 integration failure or timeout, 29s timeout limit

    - Caching
        - Caching is configured per stage
        - caches things by default for 300 seconds, 500mb to 237gb cache size
        - can be encrypted
        - caching reduces cost, increases performance

- Simple Queue Service (SQS)
    - public, fully managed, highly available queues with standard or FIFO modes
    - messages up to 265KB, link to large data
    - received messages are hidden (VisibilityTimeout)
        - if the message isn't processed within the timeout period, it'll reappear (retry) or explicity deleted
    - Dead-Letter queues can be used for problem messages
    - ASGs can scale and Lambda function can be invoked from the queue
    
    - Standard SQS
        - gaurantees atleast one delivery
    - FIFO SQS
        - exactly once delivery, first in first out and keeps track of the order of the messages in queue
        - 3000 messages per second with batching, or up to 300 per second without
        - billed based on requests
        - short polling is immediate and is billed even if returning 0 messages
        - long polling (waitTimeSeconds) should be used as it can wait inbetween requests
        - ecnryption at rest (KMS) and in-transit
        - queue policy, same as a resource policy for an S3 bucket
        
- Kineses
    - scalable streaming service used to injest lots of data
    - producers send data into kinesis stream
    - streams can scale from low to near infinite data rates
    - public service and HA in region by design
    - streams store a 24-hour moving window of data
    - multiple consumers can access the data from that moving window
    - great for analytics and dashboards
    - each shard in the kinesis stream has 1MB ingestion, and 2MB consumption
    - the data window determines the price, 24 hour window by default. can be increased to 7 days for additional cost
- Kineses Data Firehose
    - if you need to store data for longer than the moving window, you can use firehose to put that data in to S3
    - fully managed service to load data into data lakes, data stores, and analytics services
    - automatic scaling, fully serverless and resilient
    - near real time delivery (60 seconds)
    - supports transformation of data on the fly (lambda)
    - pay as you go billing, billed by the volume through firehose
    - destinations are http endpoints, splunk, redshift, elasticsearch, S3 destination bucket
    - producers can send data directly to firehose if you dont need the features of kinesis data streams

- Kinesis Data Analytics
    - real time data processing product
    - uses Structured Query Language (SQL)
    - ingests from kinses data streams or firehose
    - Destinations support Firehose (s3, redshift, elsastic search, splunk), AWS Lambda, data streams
    - you define source stream and destination stream which is ingested by the kineses analytics application (inputs and outputs)
    - S3 bucket contains reference data in a reference table
        - static data used to enrich the streaming input
    - Application code written in SQL processes the input and produces output
    - you only pay for the data processed by the analytics application but its not cheap
    - scenarios where you would use analytics
        - streaming data needing real time SQL based processing
            - time-series analytics, elections/esports
            - real time dashboards, leaderboards for games
            - real time metrics for security and response teams

- Amazon Cognito
    - provides authentications, authorization and user management for web/mobile apps
    - user pools - sign in and get a JSON Web Token (JWT)
        - user directory management and profiles, sign up and sign in
    - Identity Pools - allow you to offer access to temporary AWS credentials
        - unauthenticated identities like guest users
        - federated Identities like google, facebook, twitter, SAML2.0 and user pool for short term AWS credentials
        - Cognito assumed an IAM role defined in Identity Pool and returns temporay AWS credentials

    - User Pools used for allowing self managed user accounts
    - Identity Pools to swap external Identities for temporary AWS credentials
    - 
    
