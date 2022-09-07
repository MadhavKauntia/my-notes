# [AWS Certified Developer - Associate](https://aws.amazon.com/certification/certified-developer-associate/)

- [Compute](#compute)
  - [Elastic Compute Cloud (EC2)](#elastic-compute-cloud-ec2)

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
