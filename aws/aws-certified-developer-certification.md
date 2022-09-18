# [AWS Certified Developer - Associate](https://aws.amazon.com/certification/certified-developer-associate/)

- [Compute](#compute)
  - [Elastic Compute Cloud (EC2)](#elastic-compute-cloud-ec2)
  - [Elastic Beanstalk](#elastic-beanstalk)
  - [AWS Lambda](#aws-lambda)

## Compute

- Compute resources can be considered the brains and processing power required by applications and systems to carry out computational tasks via a series of instructions.
- Compute is closely related to common server components such as CPUs and RAM.
- A physical server within a data center would be considered a _compute_ resource as it may have multiple CPUs and many GBs of RAM.

### Elastic Compute Cloud (EC2)

EC2 allows you to deploy virtual servers within your AWS environment. Most people will require an EC2 instance within their environment as a part of at least one of their solutions.

Components of EC2:

- **Amazon Machine Images (AMIs)**: An AMI is an image baseline with an operating system and applications along with any custom configuration.

- **Instance Types**: An instance type defines the size of the instance based on a number of different parameters - vCPUs, Memory, Instance Storage and Network Performance.

- **Instance Purchasing Options**: This includes different plans based on which you can purchase EC2 instances.

- **Tenancy**: This related to what underlying host your EC2 instance will reside on, so essentially the physical server within an AWS data center.

- **User Data**: User data allows you to enter commands that will run during the first boot cycle of that instance.

- **Storage Options**: Selecting storage for your EC2 instance will depend on the instance selected, what you intend to use the instance for and how critical the data.

- **Security**: During the creation of your EC2 instance, you will be asked to select a Security Group for your instance.

### Elastic Beanstalk

- Allows you to upload code of your web application along with the environment configurations.

- Automatically provision and deploy the appropriate and necessary resources required within AWS to make the web application operational.

- These resources can include other AWS services and features, such as EC2, Auto Scaling, application health-monitoring and Elastic Load Balancing in addition to capacity provisioning.

- **Environment Tiers** - The environment tier reflects on how Elastic Beanstalk provisions resources based on what the application is designed to do. For example, if the application manages HTTP requests, it will be run in a web server environment. The two environment tiers are _web server tier_ and _worker tier_.

- **Deployment Options** -
  1. _All at once_ - default choice. Rolls the application out to your resources at the same time.
  2. _Rolling_ - application is deployed in batches. this minimises the amount of disruption that is caused.
  3. _Rolling with additional batch_ - adds another batch of instances within your environment to your resource pool to ensure application availability is not impacted.
  4. Immutable - creates an entirely new set of instances. this new set is served through a temporary autoscaling group behind your ELB. the old environment would be removed and the autocaling group updated once the deployment is complete.

### AWS Lambda

- It is a serverless compute service that allows you to run application code without having to manage EC2 instances.

Components for AWS Lambda:

1. A **lambda function** is compiled of your own code that you want Lambda to invoke.
2. **Event sources** are AWS services that can be used tot trigger your Lambda functions.
3. **Downstream resources** are resources that are required during the execution of your lambda function.
4. **Log streams** help to identify issues and troubleshoot issues with your Lambda function.

**Event Source Mapping**: An event source mapping is the configuration that links your event source to your Lambda function. It's what links the events generated from your event source to invoke your function.

| Push-Based Services                                                                                                         | Poll-Based Services                                                                       |
| --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| The mapping is maintained within the Event Source                                                                           | The configuration of the mappings is held within your Lambda function                     |
| By using the appropriate API calls for the event source service, you are able to create and configure the relevant mappings | With the `CreateEventSourceMapping` API, you can set up the relevant event source mapping |
| This will require specific access to allow the event source to invoke the function                                          | The permission is required in the Execution role policy                                   |

- When a function is invoked synchronously, it enables you to assess the result before moving on to the next operation required
- If you want to control the flow of your functions, then synchronous invocations can help you maintain an order
- Asynchronous invocations can be used when there is no need to maintain an order of function execution
