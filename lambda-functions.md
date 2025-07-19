### AWS Lambda

<details><summary>1. What is AWS Lambda?</summary>
<code>AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. It scales automatically and executes in response to events.</code>
</details>

<details><summary>2. How does AWS Lambda work?</summary>
<code>You upload your code and configure event sources. Lambda manages the environment, scales execution, and provides monitoring via CloudWatch.</code>
</details>

<details><summary>3. What are the key benefits of using AWS Lambda?</summary>
<code>Auto-scaling, reduced ops overhead, cost-efficient (pay-per-use), ideal for event-driven architectures.</code>
</details>

<details><summary>4. What types of events can trigger Lambda?</summary>
<code>Amazon S3 object changes, DynamoDB table updates, API Gateway requests, CloudWatch events, and custom triggers.</code>
</details>

<details><summary>5. How is concurrency managed in Lambda?</summary>
<code>Lambda handles concurrency by scaling function instances. You can set reserved and maximum concurrency limits.</code>
</details>

<details><summary>6. What’s the max execution duration?</summary>
<code>15 minutes per invocation.</code>
</details>

<details><summary>7. How do you pass data to/from Lambda?</summary>
<code>Use event objects as input and return responses with output data, formatted as JSON.</code>
</details>

<details><summary>8. Can Lambda talk to external resources?</summary>
<code>Yes. It can call APIs, connect to databases, and integrate with other services using SDKs and VPC configurations.</code>
</details>

<details><summary>9. What are Lambda layers?</summary>
<code>Reusable packages for code, libraries, or runtimes shared across functions for modularization and cleaner deployment.</code>
</details>

<details><summary>10. How to handle errors in Lambda?</summary>
<code>Use try-catch blocks in code. Monitor via CloudWatch Logs. Configure retry strategies for async invocations.</code>
</details>

<details><summary>11. Can Lambda access the internet?</summary>
<code>Yes, either via public endpoints or by configuring the function inside a VPC with NAT access.</code>
</details>

<details><summary>12. Supported Lambda execution environments?</summary>
<code>Node.js, Python, Java, Go, Ruby, .NET Core, and custom runtimes via Runtime API.</code>
</details>

<details><summary>13. Configuring environment variables?</summary>
<code>Set variables when creating/updating the function via Console, CLI, or Infrastructure as Code templates.</code>
</details>

<details><summary>14. Synchronous vs. Asynchronous invocations?</summary>
<code>Sync = wait for result; Async = immediate return with output sent to a destination like SQS or SNS.</code>
</details>

<details><summary>15. What is Event Source Mapping?</summary>
<code>Binds event sources (e.g., Kinesis, DynamoDB streams) to a Lambda function for automatic invocation when new events arrive.</code>
</details>

<details><summary>16. Managing permissions for Lambda?</summary>
<code>Attach IAM execution roles defining access to AWS resources. Lambda assumes the role at runtime.</code>
</details>

<details><summary>17. What are AWS Step Functions?</summary>
<code>A serverless orchestration tool to coordinate multi-step workflows using state machines across AWS services, including Lambda.</code>
</details>

<details><summary>18. How to automate Lambda deployment?</summary>
<code>Use AWS SAM, CloudFormation, CodePipeline, or third-party CI/CD tools for scripted deployments.</code>
</details>

<details><summary>19. Can Lambda connect to on-prem resources?</summary>
<code>Yes. Deploy within a VPC with VPN or AWS Direct Connect access to your on-prem network.</code>
</details>

<details><summary>20. What is the Cold Start issue?</summary>
<code>Delay caused when a Lambda function is invoked after being idle — the runtime environment must initialize before execution.</code>
</details>

---

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



