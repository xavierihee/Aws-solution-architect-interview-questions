### 1. What is AWS Lambda?
AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. It automatically scales and manages the infrastructure required to run your code in response to events.

### 2. How does AWS Lambda work?
You can upload your code to Lambda and define event sources that trigger the execution of your code. Lambda automatically manages the execution environment, scales it as needed, and provides monitoring and logging.

### 3. What are the key benefits of using AWS Lambda?
The benefits of AWS Lambda include automatic scaling, reduced operational overhead, cost efficiency (as you pay only for the compute time used), and the ability to build event-driven architectures.

### 4. What types of events can trigger AWS Lambda functions?
AWS Lambda functions can be triggered by various event sources, such as changes in Amazon S3 objects, updates to Amazon DynamoDB tables, HTTP requests through Amazon API Gateway, and more.

### 5. How is concurrency managed in AWS Lambda?
Lambda automatically handles concurrency by scaling out instances of your function in response to incoming requests. You can set a concurrency limit to control how many concurrent executions are allowed.

### 6. What is the maximum execution duration for a single AWS Lambda invocation?
The maximum execution duration for a single Lambda invocation is 15 minutes.

### 7. How do you pass data to and from AWS Lambda functions?
You can pass data to Lambda functions through event objects, which contain information about the triggering event. You can also return data by using the return statement or creating a response object.

### 8. Can AWS Lambda functions communicate with external resources?
Yes, Lambda functions can communicate with external resources such as databases, APIs, and other AWS services by using appropriate SDKs and APIs provided by AWS.

### 9. What are AWS Lambda layers?
AWS Lambda layers are a way to manage and share code that is common across multiple functions. Layers can include libraries, custom runtimes, and other function dependencies.

### 10. How can you handle errors in AWS Lambda functions?
You can handle errors by using try-catch blocks in your code. Lambda also provides CloudWatch Logs for monitoring, and you can set up error handling and retries for asynchronous invocations.

### 11. Can AWS Lambda functions access the internet?
Yes, Lambda functions can access the internet through the Virtual Private Cloud (VPC) or through public endpoints if your function is not configured within a VPC.

### 12. What are the execution environments available for AWS Lambda functions?
Lambda supports several runtimes, including Node.js, Python, Java, Go, Ruby, .NET Core, and custom runtimes using the Runtime API.

### 13. How can you configure environment variables for AWS Lambda functions?
You can set environment variables for Lambda functions when creating or updating the function. These variables can be accessed within your code.

### 14. What is the difference between synchronous and asynchronous invocation of Lambda functions?
Synchronous invocations wait for the function to complete and return a response, while asynchronous invocations return immediately, and the response is sent to a specified destination.

### 15. What is the AWS Lambda Event Source Mapping?
Event Source Mapping allows you to connect event sources like Amazon DynamoDB streams or Amazon Kinesis streams to Lambda functions. This enables the function to process events as they occur.

### 16. How can you manage the permissions and execution roles for AWS Lambda functions?
You can use AWS Identity and Access Management (IAM) roles to grant permissions to your Lambda functions. Execution roles define what AWS resources the function can access.

### 17. What is AWS Step Functions?
AWS Step Functions is a serverless orchestration service that lets you coordinate multiple AWS services into serverless workflows using visual workflows called state machines.

### 18. How can you automate the deployment of AWS Lambda functions?
You can use AWS Serverless Application Model (SAM) templates, AWS CloudFormation, or CI/CD tools like AWS CodePipeline to automate the deployment of Lambda functions.

### 19. Can AWS Lambda functions connect to on-premises resources?
Yes, Lambda functions can connect to on-premises resources by placing the function inside a VPC and using a VPN or Direct Connect connection to establish connectivity.

### 20. What is the Cold Start issue in AWS Lambda?
The Cold Start issue occurs when a Lambda function is invoked for the first time or after it has been idle for a while. The function needs to be initialized, causing a slight delay in response time.

·



1. Lambda has 2 side permissions
When we look at access permissions for Lambda, there are 2 sides to consider: Permission to trigger (or invoke) the function, and permissions of the Lambda function itself to take actions with other services. These permissions are handled through AWS IAM, primarily through a resource policy that controls which principles have permissions to invoke the function and an execution role that controls what the function is permitted to do. A principal may be a user, role, service, or account. However, in this course, we’ll focus on the use case of another AWS service triggering a Lambda function.


2. What is Lambda Execution Role
Execution role gives permission to interact with services

Example : IAM Role

Selected or created when you create a Lambda function
IAM policy includes actions that can be taken with the resource
Trust policy that allows Lambda to AssumeRole
Creator must have permission for iam:PassRole

Example execution role policy definitions

IAM policy : This IAM policy allows the function to perform the DynamoDB PutItem action against a table named Test in the us-west-2 region.
{
  "Effect": "Allow",
  "Action": [
    "dynamodb:PutItem"
  ],
  "Resource": "arn:aws:dynamodb:us-west-2:00000000:table/test"
}
Trust policy: And the trust policy shows that this role gives Lambda the permission to assume the role and invoke the function on your behalf.
{
  "Effect": "Allow",
  "Pricipal": {
    "Service": "lambda.amazonaws.com"
  },
  "Action": "sts:AssumeRole"
}

3. How to Lambda function access to resources in a VPC

To enable your Lambda function to access resources inside your private virtual private cloud (VPC), you must provide additional VPC-specific configuration information, which includes VPC subnet IDs and security group IDs. You also need to include an execution role with permissions to create, describe, and delete elastic network interfaces. Lambda provides a permissions policy named AWSLambdaVPCAccessExecutionRole for this purpose.


4. How many kind of event sources (used to trigger lambda function )
Lots of services can be event sources. An event source is the entity that publishes events, and a Lambda function is the custom code that processes the events. Configuration of services as event triggers is referred to as Event Source Mapping.


event sources
Data Stores: Tracking change from data ( New/update record, delete record action ) we can trigger an event to call the lambda function
Endpoints: HTTP/ Websocket from API Gateway, Alexa command can trigger a lambda function
Repositores: We can trigger a lambda function when new code commit to github use CodeCommit to execute CI/CD action build.
Message Services: A lambda function can triggered when a new message added to SQS, or a SNS notification ( Even we can use Cloudwatch with corn event run some lambda function every 2 weeks )
5. How many model for invoking lambda function
Each of the event sources will invoke Lambda in one of these execution models:

Synchronous push
Asynchronous push
Stream-based polling
Non-stream polling
Synchronous Push

To invoke Lambda synchronously via API, use RequestResponse.
Synchronous events expect an immediate response from the function invocation.

With this execution model, there is no built-in retry in Lambda. You must manage your retry strategy within your application code.

AWS services that invoke Lambda synchronously

Amazon API Gateway
Amazon Cognito
AWS CloudFormation
Amazon Alexa
Amazon Lex
Amazon CloudFront
2. Asynchronous Push


To invoke functions asynchronously via API, use Event.
Asynchronous events are queued, and the requestor doesn’t wait for the function to complete.

This model makes sense for batch processes. With an async event, Lambda automatically retries the invoke twice more on your behalf. You also have the option to enable a dead-letter queue on your Lambda function.

In November, 2019, two new error handling options were added to give you more control over failed records from asynchronous event sources: Maximum Event Age and Maximum Retry Attempts.

Notes:

When invoking a Lambda function programmatically, you must specify the invocation type.
When AWS services are sources, the invocation type is predetermined.
For your convenience, the Lambda console shows all event sources for a given function.
When you select Test from the Lambda console, it always invokes your Lambda function synchronously.
Examples of AWS services that invoke Lambda asynchronously

Amazon Simple Storage Service (Amazon S3)
Amazon Simple Notification Service (Amazon SNS)
Amazon Simple Email Service (Amazon SES)
Amazon CloudWatch Logs
Amazon CloudWatch Events
AWS CodeCommit
AWS Config
AWS Internet of Things (IoT) button
3. Polling events Stream-based polling


Three services use the polling model. Two of them, DynamoDB and Kinesis Data Streams, are stream-based. In the polling model, events put information into the stream AWS Lambda polls the steam and if it finds records, it will deliver the payload and invoke the Lambda function. the Lambda service itself is pulling data from the stream for processing by the Lambda function

With stream-based polling, Lambda gets a batch of records from within a shard, and attempts to invoke the Lambda function. If there’s an error processing the batch of records, Lambda will retry until the data expires, which can be up to seven days. The key thing to note is that a failure in this model blocks Lambda from reading any new records from the stream until the failed batch of records either expires or is processed successfully. This is important because the events in each shard from the stream need to be processed in order

With streams, errors in a shard block further processing A failure in this model blocks Lambda from reading any new records from the stream until the failed batch of records either expires or is processed successfully. This is important because the events in each shard from the stream need to be processed in order.

4. Polling events non-stream polling


Polling events non-stream polling
Amazon SQS, is not stream-based. This is the most recently added event source for Lambda. In the polling model, events put information into the queue respectively. AWS Lambda polls the queue, and if it finds records, it will deliver the payload and invoke the Lambda function. In this model, the Lambda service itself is pulling data from the queue for processing by the Lambda function

With queues, errors in a batch are returned to queue Lambda will keep retrying a failed message until it is processed successfully, or the retries or retention period are exceeded. If the message fails all retries, it will either go to the DLQ if configured, or it will be discarded.

An error doesn’t stop processing of the batch, but there may be a change to the order in which messages are processed.

So when sequence matters, this is what you want. In contrast, with Amazon SQS, AWS Lambda will poll a batch of records. If an invocation fails or times out, the failing message is returned to the queue. It will be available for invocation again once the visibility timeout period expires. Processing continues on other messages in the queue. Lambda will keep retrying the failed message until the message is processed successfully, or the message retention period expires. If the message expires, it will either go to the DLQ if you have one configured, or it will be discarded. The key distinction with Amazon SQS is that an error doesn’t stop processing of the batch, although there may be a change to the order in which messages are processed. If your application doesn’t care about the order in which events are processed, you have a good reason to use this method: An additional consideration is that in stream-based polling, there is no cost to make the polling calls, but with SQS polling, standard SQS rates apply for each request.

6. Lifecycle of a Lambda function
1. When a function is first invoked, an execution environment is launched and bootstrapped. Once the environment is bootstrapped, your function code executes. Then, Lambda freezes the execution environment, expecting additional invocations.
2. If another invocation request for the function is made while the environment is in this state, that request goes through a warm start. With a warm start, the available frozen container is thawed and immediately begins code execution without going through the bootstrap process.
3. This thaw and freeze cycle continues as long as requests continue to come in consistently. But if the environment becomes idle for too long, the execution environment is recycled.
4. A subsequent request starts the lifecycle over, requiring the environment to be launched and bootstrapped. This is a cold start.
7. Different between warm start and cold start
Warm start : With a warm start, the available frozen container is “thawed” and immediately begins code execution without going through the bootstrap process. This thaw and freeze cycle continues as long as requests continue to come in consistently

warm start
Cold Start : But if the environment becomes idle for too long, the execution environment is recycled. A subsequent request will start the lifecycle over, requiring the environment to be launched and bootstrapped. This is a cold start.

For many application access patterns, the cold start latency is not a problem. But, if you have a workload where a cold start is never acceptable, you can use provisioned concurrency to keep a certain number of execution environments “warm”, even when the function hasn’t been invoked in a while, or in cases where Lambda has to create new environments to keep up with requests.

We’ll talk more about concurrency in the Configuring your Lambda functions module, and you’ll find a link to more information about provisioned concurrency in the section titled Lifecycle of a Lambda function in the material that follows the video.

In 2019, AWS introduced provisioned concurrency to give you the ability to keep a number of environments “warm” to prevent cold starts from impacting your function’s performance.

8. How to write functions to take advantage of a warm start
1) Store and reference dependencies locally.
2) Limit re-initialization of variables.
3) Add code to check for and reuse existing connections.
4) Use tmp space as transient cache.
5) Check that background processes have completed.
6) Minimize cold start times
Plan for real-world conditions and traffic patterns — cold starts may not have much impact on your application and you could use provisioned concurrency to keep environments warm. But it’s always a good idea to minimize the impact of a cold start.


Plan for real-world conditions and traffic patterns — cold starts may not have much impact on your application and you could use provisioned concurrency to keep environments warm. But it’s always a good idea to minimize the impact of a cold start.

Article Refer : https://aws.amazon.com/lambda/resources/workshops-and-tutorials/ (AWS Lambda Foundations)

A useful article benchmark cold start, warm start performance between languages : https://filia-aleks.medium.com/aws-lambda-battle-2021-performance-comparison-for-all-languages-c1b441005fd1

9. Which types of Lambda runtime are supported by AWS?
For the latest list of supported runtimes, see AWS Lambda Runtimes in the AWS Lambda Developer Guide.

Node.js runtimes
Python runtimes
Ruby runtimes
Java runtimes
Go runtimes
.NET runtimes
You can also use custom runtimes.

10. Describe the handler method in the lambda function?
The handler method is the entry point that AWS Lambda calls to start executing your Lambda function. The handler method always takes two objects: the event object and the context object.

DynamoDB Event

KinesisFirehose Event

Event: The event object provides information about the event that triggered the Lambda function (DynamoDBEvent, SQS Event, Kinesis Event, etc). This could be a pre-defined object that an AWS service generates, or it could be a custom user-defined object in the form of a serializable string. For example, it could be a pojo or a json stream.

Context: The context object is generated by AWS, and provides metadata about the execution. You can use it to interact with Lambda. For example, it includes getAwsRequestId(), getLogGroupName(), getRemainingTimeInMillis(); and the getInvokedFunctionArn(); function.

getInvokedFunctionArn : Gets the function Arn of the resource being invoked.

Note: with NodeJS the third argument is callback used for non-async purpose

https://docs.aws.amazon.com/lambda/latest/dg/nodejs-handler.html

11. Could you please tell me how to design a lambda function
Separate business logic
Write modular functions
Treat functions as stateless
Only include what you need
Use environment variables
Avoid recursive code

Separate your core business logic from the handler method.
This not only makes your code more portable but also allows you to target unit tests at your code without worrying about the configuration of the function.
Service: External integration & service abstractions

Note: Handler function configuration, some specific lambda code, log something, and No business logic here

Write modular functions Create single purpose functions.
Follow the same principles you would apply to develop microservices.


Write modular functions
Treat functions as stateless: No information about state should be saved within the context of the function itself. Because your functions only exist when there is work to be done, it’s particularly important for serverless applications to treat each function as stateless.
DynamoDB is serverless and scales horizontally to handle your Lambda invocations. It also has single-millisecond latency, which makes it a great choice for storing state information.

Consider Amazon ElastiCache if you have to put your Lambda function in a VPC. It may be less expensive than DynamoDB.

Amazon S3 can be used as an inexpensive way to store state data, if throughput isn’t critical and the type of state data you are saving won’t change rapidly.

Only include what you need : Minimize both your deployment package’s dependencies and its size.
This can have a significant impact on the startup time for your function. For example, only choose the modules that you need — don’t include an entire AWS SDK.
Reduce the time it takes Lambda to unpack deployment packages authored in Java. Put your dependency.jar files in a separate /lib directory.
Opt for simpler Java dependency injection (IoC) frameworks.
For example choose Dagger or Guice, over more complex ones like Spring Framework. (https://medium.com/bigeye/dependency-injection-103-guice-vs-dagger-6ab7afa68d3d_ https://www.baeldung.com/guice-spring-dependency-injection )
Use environment variables : Take advantage of environment variables for operational parameters. They allow you to pass updated configuration settings without changes to the code itself. You can also use environment variables to store sensitive information required by the function. https://docs.aws.amazon.com/lambda/latest/dg/env_variables.html
Avoid recursive code : Avoid a situation where a function calls itself. This could continue to spawn new invocations that would make you lose control of your concurrency. You can quickly set the concurrent execution limit to zero by using the console or command line to immediately throttle requests if you accidentally deploy recursive code.
11. How many ways to deploy the lambda function
AWS Management Console
External tool (IDE) SAM CLI, Serverless Framwork
Link to S3 Bucket
12. Three core components to the configuration
Memory
Timeout
Concurrency ( It’s a long story and challenge )
You are billed based on memory + duration

13


AWS
AWS Lambda
Interview Questions
13



