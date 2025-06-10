### Deployment modes of the cloud:
- Private Cloud: Single org, complete control, good security.
- Public Cloud: Owned and operated by third party cloud service. 
- Hybrid Cloud: some servers on premises, some extensions to public cloud. Control over private infrastructure. Flexible 
### Characteristics of Cloud Computing
- On-demand self service: use resources without human interaction
- Broad Network Access: accessed by diverse client platforms
- Multi-tenancy and resource pooling: same resources shared b/w multiple tenants. 
- Rapid elasticity and scalability: duh
- Measured service: pay for what you use.
### Six Advantages of Cloud Computing
- Trade capital expense (CAPEX) for operational expense (OPEX).
- Benefit from massive economies of scale.
- Stop guessing capacity.
- Increase speed and agility.
- No money spend running and maintaining data centers.
- Go global in minutes.
### Types of Cloud Computing
- Infrastructure as a Service (IaaS):
	- Building blocks for Cloud IT.
	- Example: Amazon Web Services (AWS), Google Cloud, Microsoft Azure.
- Platform as a Service (PaaS):
	- Includes everything that developers need to build, run, and manage applications - From servers and OS to all the networking, storage, middleware tools etc.
	- Example: AWS Elastic Beanstalk
- Software as a Service (SaaS):
	- Completed Product run and managed by service provider. 
	- Example: Gmail, Microsoft Office 365.
	![[ManagementWithAWSServices.png|550]]

#### AWS pricing (Pay as you go pricing model)
- **Compute** - pay for compute time.
- **Storage** - data stored in the cloud.
- **Data Transfer** OUT of the cloud - IN is free.

### AWS Global Infrastructure
- AWS Regions: Cluster of Data Centres. E.g.- us-east-1, eu-west-3 etc.
- AWS Availability Zones (AZ): 
	- Physically separate and isolated location within a region. This means if one AZ fails, the others remain operational.
	- Regions have many availability zones (min 3, max 6). E.g. - ap-southeast-2a, ap-southeast-2b, ap-southeast-2c.
- Points of Presence: Global network locations to improve performance and security of AWS. Over 400.
	- Consists of Edge Locations, Regional Edge Caches, AWS Global network hubs.
	- **Edge Locations** are **data centres located in various cities worldwide** that serve as entry points to AWS’s network. 

- AWS has both **Global services** (Route 53, IAM, Cloudfront) as well as **Regional Services** (Amazon EC2, Elastic Beanstalk, Lambda)
### Shared Responsibility Diagram
![[SharedResponsibilityModel.png]]
- **Customer**: Security IN the cloud.
- **AWS**: Security OF the cloud.
---
### Identity and Access Management
- **Users** are people within your organization can be grouped.
- **Groups** only contain users, not other groups.
- Users don’t have to belong to a group, and user can belong to multiple groups.
- **Policies** are JSON documents that can be assigned to groups and/or users. They define the **permissions** of the users. ^d154b6
- **Least Privilege Principle**: don't give more permissions than needed.
- Policies also have **inheritance** which means users part of two groups will inherit the policies from both groups. 
- Example of a policy:
	```
	{ 
	//Version is the version of the policy language
	//One or more individual statements (required)
	//id is an identifier for the policy (optional)
	
		"Version": "2012-10-17", 
		"Statement": [ 
			{ 
				"Effect": "Allow", 
				"Action": "ec2:Describe*", 
				"Resource": "*" 
			}, 
			{ 
				"Effect": "Allow", 
				"Action": "elasticloadbalancing:Describe*" , 
				"Resource": "*" 
			}, 
			{ 
				"Effect": "Allow", 
				"Action": [ 
					"cloudwatch:ListMetrics", 
					"cloudwatch:GetMetricStatistics", 
					"cloudwatch:Describe*" 
				], 
				"Resource": "*" 
			} 
		] 
	}
	```
- Statements consists of:
	- Sid: Statement id (optional).
	- Effect: allow or deny access.
	- Principal: account/user/role the policy is applied to.
	- Action: list of actions that the policy allows or denies.
	- Resource: list of resources to which the action applies to
	![[ExampleOfPolicyAWS.png|500]]
- IAM also allows you to set up password policies.
- Multi Factor Authentication (MFA): to protect root accounts and IAM Users. if a password is stolen, the account is not compromised.
- When AWS services need to perform actions on the user's behalf, they have to be assigned permissions to do so, using IAM Roles.
___
### Accessing AWS
- AWS Management Console: Online on the website. protected by password + MFA.
- AWS Command Line Interface (CLI): Command Line shell interaction. protected by access keys.
- AWS Software Development Kit (SDK): set of libraries and tools for development (API calls, multiple language support). protected by access keys.
	- Access Key ID synonymous to username.
	- Secret Access key synonymous to password.
	
---
### Amazon Elastic Compute Cloud (EC2)
- Main capability:
	- Renting virtual machines (VMs).
	- Storing data on virtual drives (EBS).
	- Distributing load across machines (ELB).
	- Scaling services using Auto Scaling Groups (ASG).
- EC2 Configurations:
	- OS (Linux, Windows etc)
	- CPU (Compute power)
	- RAM
	- Storage Space:
		- Network Attached (EBS and EFS)
		- Hardware (EC2 Instance Store)
	- Network Card (Internet access)
	- Firewall (Protection)
	- Bootstrap script (starting OS: EC2 User Data): Only run once at the instance first start. 

#### EC2 Instance Types
- Different instance types optimized for different use cases.
- Naming convention: m5.2xlarge
	- m: Instance class
	- 5: generation
	- 2xlarge: size within instance class
##### General Purpose
- Great for diversity of workloads such as web servers or code repos.
- Balance between:
	- Compute
	- Memory
	- Networking 
- Example: t2.micro

##### Computer Optimized
- Great for compute-intensive tasks that require high performance processors:
	- Examples: High performance computing, ML, gaming servers etc.
- Usually start with C (just an observation).

##### Memory Optimized
- Fast performance for processing large data sets in memory.
	- Examples: High performance databases, real time processing of big unstructured data.
- Usually starts with X or M 

##### Storage Optimized
- Great for high sequential read and write access to large data sets on local storage.
	- Example: Relational and Nosql database, data warehousing applications.

#### Security Groups
- Control the traffic in and out of EC2 instance.
- Only contain **allow** rules. (no deny)
- Acting "firewall" for EC2 instances.
- Regulating access to ports, authorised IP ranges, control of inbound and outbound network.
- All inbound traffic **blocked** by default.
- All outbound traffic is **authorised** by default.
#### EC2 Instance Connect
- Connect to EC2 instance in your browser without using SSH on Command line.
- Temporary key uploaded onto EC2 by AWS.
- Still uses SSH behind the back.
#### EC2 Instances Purchasing Options (Important)

##### EC2 On Demand
- Pay for what you use. **Billing per second**. 
- **Highest Cost** but no upfront payment.
- No long-term commitment.
- Good for **short-term** and **uninterrupted** workloads, where you can't predict how the application will behave.
##### EC2 Reserved Instances (RI)
- Reserve a specific instance attributes (Instance Type, Region, Tenancy, OS).
- **Reservation period** - **1 year** or **3 years**. Higher discount the longer reservation period.
- **Payment options** - **No upfront**, **Partial upfront**, **All upfront**. Higher payment upfront higher discount.
- **Reserved Instance's Scope**: **Regional** (reserve capacity across a region) or **Zonal** (reserve capacity across a specific Availability Zone).
- Recommended for steady-state usage applications (like database).
- Buy and sell in the Reserved Instance marketplace.
- Two types of RIs:
	- Convertible RIs: exchanged with different instance types. More flexible.
	- Standard RIs: steady instance types. better with predictable workload. Less flexible

##### EC2 Savings Plan
- Get a discount based on long-term usage.
- Commit to a certain type of usage ($10/hour for 1 or 3 years).
- Usage beyond the savings plan is billed at **on demand price**.

##### Difference between RI and Savings Plan

| **Feature**     | **Reserved Instances**         | **Savings Plan**                         |
| --------------- | ------------------------------ | ---------------------------------------- |
| **Commitment**  | Instance type, size, AZ/region | Amount spending per hour across services |
| **Flexibility** | Less Flexible                  | More Flexible                            |
| **Scope**       | Regional or AZ                 | Flexible across EC2, Lambda, and Fargate |
##### EC2 Spot Instances
- **Most cost-efficient** instances in AWS.
- Instances that you can **lose** at any point if your max price is less than the current spot price.
- Good for workloads **resilient to failure** (like Batch jobs, Image processing, data analysis, distributed workload, flexible workloads).
- Bad for **critical** **jobs** or **databases**.
##### EC2 Dedicated Hosts
- Physical server with EC2 fully dedicated to your use.
- Addresses **compliance requirements** and use existing **server-bound software licenses**.
- Purchasing options:
	- **On-demand**: pay per second for dedicated host
	- **Reserved**: 1 or 3 years (**No upfront**, **Partial upfront**, **All upfront**)
- **Most expensive option**.
- Whenever you think about **licensing** or **regulatory or compliance**, or **detailed control** think Dedicated hosts.
##### EC2 Dedicated Instances
- Instances run on hardware that's dedicated to you.
- Isolated at the hardware level. No sharing physical server with other customers. Physical Host can still be shared with other customers.
- AWS controls placement.
- Whenever you think **compliance requirements** **but simpler** than dedicated hosts. 
##### EC2 Capacity Reservations
- Reserve **on-demand** instances in specific AZ for any duration.
- Always have access to EC2 when needed.
- **No time commitment**, no discount.
- Charged at On-demand rate.
#### Shared Responsibility Model for EC2

| **AWS**                                  | **Customer**                                   |
| ---------------------------------------- | ---------------------------------------------- |
| Infrastructure (global network security) | Security group rules                           |
| Isolation on physical hosts              | OS patches and updates                         |
| Replacing faulty hardware                | Software installed on EC2                      |
| Compliance Validation                    | IAM Access Management and Data security on EC2 |
#### EC2 Instance Storage
##### Elastic Block Store (EBS) Volume
- Network drive (not a physical drive, so has latency) attached to instances.
- Mounted to only on instance at a time.
- Bound (locked) to specific AZ.
- Root EBS volume gets deleted on termination of EC2 instance by default (not other volumes).
###### EBS Snapshots
- Backup (Snapshot) of EBS volumes
- Can be done at any point in time.
- Snapshots can be copied and then restored in another AZ/Region.
- **EBS Snapshot Archive Tier**: Cheaper than normal tier to archive snapshot. Takes 24 to 72 hours to restore archive.
- **Recycle bin for EBS**: rules to delete EBS Snapshots so you can recover them after accidental deletion.
##### EC2 Instance Store
- High performance hardware disk (unlike EBS network drives).
- Lose their storage if stopped.
- Risk of data loss if hardware fails, better I/O performance.
##### Elastic File System (EFS)
- A managed Network File System (NFS) allows shared access to a file system across EC2 instances and services.
- Can be mounted on **100s** of EC2 instances.
- Highly available, scalable, pay per use.
	- **EFS-IA (infrequent access)**: Storage class optimized for files not accessed everyday.
		- Quite cheaper than EFS standard class.
		- EFS automatically moves files to EFS-IA if **lifecycle policies** are enabled.
##### Amazon Machine Image
- Customization of EC2 Instance like software, configs, OS etc.
- Faster boot/config time as all software is pre-packaged.
- Built for specific **region**.
- **AWS Marketplace AMI**: Selling and buying AMIs.
###### EC2 Image Builder
- Service automating the creation of Virtual Machines (VMs) or container images.
- Automate creation, maintain, validate, test **EC2 AMIs**.
- Can be run on a schedule.
- Free service, only pay for underlying resources.

![[EC2ImageBuilderService.png| center |600]]
![EC2 Image Builder](./AWS%20Notes%20Screenshots/EC2ImageBuilderService.png)
Diagram of EC2 Image Builder Service

##### Amazon FSX
- 3rd Party High-performance file systems on AWS
- Fully managed service
- **FSx for Windows File Server** or **for Lustre**

| **AWS**                                                                                         | **Customer**                                                                                                  |
|--------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| Infrastructure                                                                                   | Setting up backup/snapshot procedures                                                                          |
| Responsible for replication of data for EBS and EFS drives (**not** the EBS and EFS drives themselves) | Setting up data encryption                                                                                     |
| Replacing faulty hardware                                                                        | Responsible for any data on the drives                                                                         |
| Ensuring that AWS employees cannot access your data                                               | Understanding the risk of using EC2 Instance Store                                     |

#### Important ports to know

| Ports                                | **Port Number** |
| ------------------------------------ | --------------- |
| SSH (Secure Shell)                   | 22              |
| FTP (File Transfer Protocol)         | 21              |
| SFTP (Secure File Transfer Protocol) | 22              |
| HTTP (HyperText Transfer Protocol)   | 80              |
| HTTPS (HTTP Secure)                  | 443             |
| RDP (Remote Desktop Protocol)        | 3389            |

---
### Elastic Load Balancing and Auto Scaling Groups
#### Scalability 
- **Definition**: Ability to accommodate larger load by making the hardware stronger (scale up) or by adding nodes (scale out).
- Linked to **High Availability**.
- **Vertical Scalability** (scale up/down):
	- Increasing the size of the instance
	- Usually hardware limit to how much you can scale.
	- Common for non-distributed systems like **databases**.
- **Horizontal Scalability** (scale out/in):
	- Increasing the number of instances/systems.
	- Implies distributed systems
#### High Availability
- Goes hand in hand with horizontal scaling.
- Means running your application in **at least** 2 Availability Zones.
- Goal: Survive a data centre loss.
#### Elasticity
- **Definition**: Ability to scale systems depending on the load. 
- Only happens after scalability is established.
- Optimizes costs.
#### Agility
- **Definition**: Reducing the time to make resources available for developers from weeks to just minutes.
- **Not** related to scalability.
#### Load Balancing
- **Definition**: Process of distributing network/application traffic across multiple servers so that no single server is overwhelmed with traffic.
- Load Balancers are servers that forward traffic to multiple servers (EC2 Instances) downstream.
- Exposes a single point of access (DNS) to application.
- Provide SSL (HTTPS) to applications.
- High Availability across zones.
##### Elastic Load Balancer
- Managed load balancer.
- 4 types:
	- **Application Load Balancer** (HTTP/HTTPS only) - Layer 7
	- **Network Load Balancer** (ultra high performance, allows for TCP) - Layer 4
	- **Gateway Load Balancer** - Layer 3
	- **Classic Load Balancer** - Layer 4 and 7
	
![[ElasticLoadBalancerDiagram.png|center| 600]]
<p style="text-align:center">Load Balancer</p>
#### Auto Scaling Group (ASG)
- Scale servers based on website/application traffic.
- Scale out (add EC2 instances) to match increased load.
- Scale in (remove EC2 instances) to match decreased load.
- Ensure min and max number of instances running.
- Replace unhealthy instances.
- Great at cost savings.
##### Scaling Strategies
- **Manual Scaling**: manually update ASG.
- **Dynamic Scaling**: Respond to changing demand.
	- **Simple/Step Scaling**: depending on **CloudWatch** alarm triggers.
	- **Target Tracking Scaling** like I want the average ASG CPU to stay at around 40%.
	- **Scheduled Scaling** anticipate scaling based on usage patterns
	- **Predictive Scaling**: Uses Machine Learning to predict future traffic ahead of time.

![[AutoScalingGroupExample.png| center | 600]]
<p style="text-align:center">ASG with Load Balancer</p>
___
### Amazon S3
- Building blocks of AWS
#### Use Cases
- Backup and Storage
- Disaster Recovery
- Media Hosting
- Website hosting
#### Buckets
- Allows people to store files (objects) into directories (buckets).
- **Must have** globally unique name.
- Regional service although looks global.
#### Objects
- Files are objects.
- All objects have keys which is the full path of the object.
	- The key is composed of **prefix** + *object name*
	- E.g.- **s3://my-bucket/my_folder1/another_folder/***my_file.txt*
- Illusion of directories but all objects just have keys.
- Object values are the content of the body.
- Contains Metadata: list of key-value pairs
- Tags: Unicode key-value pairs, useful for security.
- Version ID if versioning enabled.
	- Easy rollback to previous version.
	- Protect against unintended deletes.
#### Security
- **User-Based**
	- **IAM policies** 
- **Resource-Based**
	- **Bucket Policies**: JSON based policies very similar to [[Amazon Web Services (AWS) CCP Notes#^d154b6|IAM Policies]].
	- **Object Access Control List (ACLs)**: finer grain 
	- **Bucket ACLs**: less common
- IAM Principal can access an S3 object if
	- The user IAM permissions ALLOW **OR** resource policy ALLOW 
	- **AND** no explicit DENY.
- **Encryption**: encrypt objects in S3 using encryption keys.
	- **Server Side Encryption**: Handled by AWS (default).
	- **Client Side Encryption**: Handled by User and then AWS does NOT encrypt.
#### Replication
- Must enable versioning for this
- **Cross Region Replication (CRR)**: compliance, lower latency access, replication across accounts (use cases).
- **Same Region Replication (SRR)**: log aggregation, live replication between product and test accounts (use cases).
#### Storage Classes
- **Durability**: how reliably data is stored and protected from loss over time. S3 is highly durable 99.999999999% (11 nines)
- **Availability**: how accessible is the data at any given time. 
##### S3 Standard - General Purpose
- Used for frequently accessed data.
- Low Latency and high throughput.
- Use cases: Big data analytics, mobile and gaming applications.
##### Infrequent Access
- Less frequently accessed data but rapidly available when needed. 
- Lower cost than Standard.
- **Amazon S3 Standard-Infrequent Access (S3 Standard-IA)**:
	- Use cases: Disaster recovery, backups.
- **Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)**:
	- Use cases: Storing secondary backup copies of on premises data.
##### Amazon S3 Glacier
- Low-Cost object storage meant for archiving/backup.
- Price for storage + object retrieval cost.
###### Amazon S3 Glacier Instant Retrieval
- Min storage for 90 days.
- Millisecond retrieval, great for data used once every quarter.
###### Amazon S3 Glacier Flexible Retrieval
- Expedited (1 to 5 minutes), Standard (3 to 5 hours), Bulk (5 to 12 hours) - free
- Min storage for 90 days.
###### Amazon S3 Glacier Deep Archive - for Long term storage
- Standard (12 hours), Bulk (48 hours)
- Min storage for 180 days.
##### S3 Intelligent Tiering
- Small monthly monitoring and auto tiering fee.
- Moves objects automatically between Access tiers based on usage.
- No retrieval charges in S3 Intelligent Tiering.
#### Shared Responsibility Model
| **AWS**                                                                                                       | **Customer**                                                     |
| ------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Infrastructure (global security, availability, durability, sustain concurrent loss of data in two facilities) | S3 Versioning                                                    |
| Configuration and vulnerability analysis                                                                      | S3 Bucket Policies                                               |
| Compliance Validation                                                                                         | S3 Replication Setup, Logging and Monitoring, S3 Storage Classes |
| Ensuring that AWS employees cannot access your data                                                           | Data encryption at rest and in transit                           |

---
### AWS Snowball
- Portable offline devices to collect and process data at the **edge** and **migrate** data in and out of AWS.
- Helps migrate up to Petabytes of Data (HUUUGE DATA)
- Solves the issue of low connectivity, low bandwidth, high network cost as one can transfer data offline into snowball device and the data gets transferred to AWS bucket of choice when it reaches the facility.
#### Snowball Edge
- Used for Edge Computing
	- Edge computing is processing data while it is being created in an edge location (limited internet and no access to computing power).
	- Use cases: preprocess data, machine learning.

### AWS Storage Gateway
- Bridge between on-premise data and cloud data in S3.
- Use cases: disaster recovery, backup and restore.

---
### Databases
#### Amazon Relational Database Service (RDS)
- Managed DB service to use SQL as query language.
- Allows to create databases in the cloud managed by AWS
	- E.g.- Postgres, MySQL, MariaDB etc.
- Automated provisioning, OS patching 
- Continuous backups
- Multi AZ for disaster recovery
- Read Replicas for improved read performance
- **Cannot** SSH into instances

![[ASGwithAmazonRDS.png| center | 600]]
<p style="text-align:center">Usage of Amazon RDS with ELB and EC2 Instances</p>
#### Amazon Aurora
- AWS technology
- PostgreSQL and MySQL are both supported as Aurora DB.
- Cloud optimized much faster than Amazon RDS. Costs more but is cost effective as it is faster.
- **Amazon Aurora Serverless**:
	- Automated database instantiation and autoscaling based on usage.
	- Good for infrequent, intermittent or unpredictable workloads.
#### RDS Deployments
- **Read Replicas**: Scale the workload of your DB.
	- Data only written to main DB.
- **Multi-AZ**: **Failover** in case of AZ outage (high availability).
	- Data is only read/written to the main database (1 other AZ as failover).
- **Multi-Region**: using read replicas in multiple regions
	- **Disaster recovery** in case of region issue.
	- **Local Performance** for global reads
#### Amazon ElastiCache
- Manage Redis or Memcached (in-memory databases).
- Helps reduce load off database for read intensive workloads.
- AWS takes care of OS maintenance/patching, optimizations, setup, config.

![[ElastiCacheArchitectureExample.png| center | 600]]
<p style="text-align:center">ElastiCache Architecture</p>
#### DynamoDB
- Fully Managed Highly available with replication across 3 AZ.
- **NoSQL Database**. Key/value database.
- **Serverless Database**
- Very low latency
- **Global Tables**: read/write to any AWS region.
#### DynamoDB Accelerator - DAX
- In memory cache for DynamoDB.
- ElastiCache for DynamoDB.
#### Redshift
- Petabyte-scale Warehouse service in AWS.
- **Redshift Serverless**: provisions and scales data warehouse.
#### Amazon Elastic MapReduce (EMR)
- Helps creating Hadoop clusters to analyze and process vast amount of data.
- For data processing, machine learning.
#### Amazon Athena
- **Serverless** query service to analyze data stored in **Amazon S3**.
- Uses SQL language to query the files.
#### Amazon QuickSight
- **Serverless** machine learning-powered **buisness intelligence** to create **interactive dashboards**.
#### DocumentDB
- AWS-implementation for MongoDB (which is a NoSQL database).
- Store, query, and index JSON data.
#### Amazon Neptune
- Fully managed **graph** database.
- Graph dataset would be social network.
#### Amazon Timestream
- **Serverless** **time series database**.
#### Amazon QLDB 
- Quantum Ledger Database.
- For recording **financial transactions**.
#### Amazon Managed Blockchain
- Think about cryptocurrencies.
#### AWS Glue
- **Serverless ETL** service: extract, transform, and load.
- Perform and transform data for analytics.
#### Database Migration Service (DMS)
- Migrate databases to AWS, resilient, self healing.
- Source database remains available during migration.

---
### Other Compute Section
#### Docker
- Software development platform to deploy applications
- Apps packaged in containers that can be run on any OS.
- Stored in Docker repositories.

| Docker                                                   | Virtual Machines                                                            |
| -------------------------------------------------------- | --------------------------------------------------------------------------- |
| Not isolated                                             | The operating system is isolated                                            |
| Really fast startup time                                 | Really slow as the whole OS needs to be booted up                           |
| Low resource usage as it doesn't need a full OS          | High resource usage as each VM has a full OS                                |
| High compatibility                                       | Low compatibility as it requires a compatible hypervisor                    |
| Used for microservices, cloud apps, CI/CD, scalable apps | Used for Running multiple OSs, legacy applications, full system emulation   |
#### Elastic Container Service (ECS)
- Launch Docker containers on AWS.
- Provision and maintain the infrastructure (EC2 instances).
- AWS takes care of starting/stopping containers.
#### Fargate
- **Serverless**, Launch Docker containers on AWS.
- No provisioning infrastructure - Simpler!
- AWS runs containers based on the CPU/RAM you need.
#### Elastic Container Registry (ECR)
- Storing your docker images on AWS
- The stored docker images can then be used by Fargate or ECS.
#### Elastic Kubernetes Service (EKS)
- **Kubernetes**: Open-source traffic manager for containerized applications. Helps run, organize, and manage multiple containers efficiently across multiple machines.
- EKS allows you to launch Kubernetes clusters on AWS.
- Containers can be hosted on:
	- EC2 Instances
	- Fargate (Serverless)
	
>[!info] What is Serverless?
>Serverless is when AWS is managing/provisoning the servers for you. Basically you don't have to worry about the servers. You just worry about running the code/application.
#### AWS Lambda
- Pricing: pay per request and computing time.
- **Event Driven**: functions get invoked by AWS when needed.
- Integrated with a lot of programming languages.

| **AWS Lambda**                         | **Amazon EC2**                 |
| -------------------------------------- | ------------------------------ |
| Serverless                             | You have to manage the servers |
| Virtual functions                      | Virtual servers                |
| Limited by time- short executions only | Limited by RAM and CPU         |
| Run on-demand                          | Continuously running           |
#### Amazon API Gateway
- Serverless and scalable service for developers to create, test, maintain, monitor and publish APIs.
- Expose Lambda functions as HTTP API.
#### AWS Batch
- Fully managed (serverless) batch processing at any scale.
- A "batch" job is a job with a start and an end.
- Batch will dynamically launch EC2 instances or Spot instances
- Defined as **Docker images** and run on **ECS**.
#### Amazon Lightsail
- Simpler alternative to EC2, RDS, ELB, EBS, Route 53.
- Great for very little cloud experience.
- High availability but no auto-scaling.
### Deploying and Managing Infrastructure at Scale
#### CloudFormation
- Declarative way of outlining your AWS Infrastructure
- Use a template (JSON/YAML) to explain what you want and CloudFormation creates it for you in the right order.
- Each resource within the stack is tagged with an identifier so you can see how much each stack costs.
#### Infrastructure Composer
- Shows diagram of all the resources and the relationships between them.
#### AWS Cloud Development Kit (CDK)
- Define the cloud infrastructure with your favourite programming language.
- The code is then compiled into a CloudFormation template (JSON/YAML).
- You can deploy infrastructure and application runtime code together
	- Great for lambda functions
	- Great for Docker containers/images in ECS/EKS.
#### Elastic Beanstalk
- Developer centric view of deploying applications on AWS.
- Platform as a Service (PaaS).
- Serverless and Managed by AWS.
- Only the application code is the responsibility of the developer.
#### AWS CodeDeploy
- Deployment automation service to deploy applications to EC2 instances, on-premises servers, Lambda functions, or ECS containers without downtime.
- Just need to provision EC2 instances/Servers with AWS CodeDeploy agent beforehand.
#### AWS CodeBuild
- Compiles source code, run tests, and produces packages that are ready to be deployed.
- Serverless
- Only pay for build time
#### AWS CodePipeline
- Orchestrate the different steps to have the code automatically pushed to production.
- Basis for CI/CD.
- Serverless.

![[ExampleOfCodePipelineUsage.png| center | 600]]
<p style="text-align:center">CodePipeline Usage</p>
#### AWS CodeArtifact
- Software packages depend on each other to be built. This is also called code dependencies.
- Storing and retrieving these dependences is called **Artifact Management**.
- CodeArtifact is artifact management tool for AWS which works with Developers and CodeBuild.
#### AWS Systems Manager (SSM)
- **Hybrid** AWS service.
- Manage EC2 and On-premises systems at scale.
- Patching automation for enhanced compliance.
- Runs command on an entire fleet of servers.
- Install the SSM agent onto the systems we control.
	- **SSM Session Manager**: allows to start secure shell on your EC2 or on-premises server.
		- No SSH access, bastion hosts, or SSH keys needed. No port 22 needed either
### Global Infrastructure Section
#### Amazon Route 53
- Managed Domain Name System (DNS)
- Routing policies
	- **Simple Routing Policy**: No health checks directly send the IP for the server.
	- **Weighted Routing Policy**: send 70% of traffic one server, 20% other server, and 10% last server.
	- **Latency Routing Policy**: Send traffic dependent on latency.
	- **Failover Routing Policy**: For disaster recovery.
#### Amazon CloudFront
- **Content Delivery Network** (CDN)
- Improves read performance as **content is cached at the edge**.
- **DDoS** protection with integration with AWS Shield, and AWS Web Application Firewall (WAF).
- Files are cached for a time to live (TTL).
#### Amazon S3 Transfer Acceleration
- Increase transfer speed by transferring files into an edge location which will forward the data to the S3 bucket using AWS private network in the target region (faster).
- **AWS Global Accelerator**: Improve global application availability and performance using the AWS global network.

| **AWS Global Accelerator**                                          | **CloudFront**                             |
| ------------------------------------------------------------------- | ------------------------------------------ |
| Uses Edge locations to proxy packets to AWS regions. Does not Cache | Improves performance for cacheable content |
| Improved performance                                                | Content served at the edge.                |
#### AWS Outposts
- **Hybrid Cloud**
- Brings AWS infrastructure, services and APIs on-premises.
- AWS ships full racks (like a full server) of servers to run workloads.
- AWS will setup and manage outpost racks.
- You are responsible for the physical security.
#### AWS WaveLength
- Wavelength zones are infrastructure deployments inside telecommunication providers' data centres at the edge of **5G networks**. 
- Brings AWS services to the edge of 5G networks.
#### AWS Local Zones
- Bring AWS services closer to users. For places that don't have AWS AZs closer to them.
- Good for latency-sensitive applications.
### Cloud Integration 
- When deploying multiple applications, they will need to communicate with each other. 
- Two patterns of communication: Asynchronous, and Synchronous.
- Synchronous bad if there are traffic spikes.
- **Decouple** applications through:
	- using SQS: queue model
	- using SNS: pub/sub model
	- using Kinesis: real-time data streaming model
#### Amazon Simple Queue Service (SQS)
- Serverless service to create a FIFO queue of messages from producer to consumer.
- Messages are processed in order (FIFO)
#### Amazon Kinesis
- Real time data streaming
- Managed service to collect, process, and analyse real-time streaming data at any scale.
#### Amazon Simple Notification Service (SNS)
- Send one message to multiple receivers.
- Follows publisher and subscriber model.
- Publisher sends one message to SNS Topic. The SNS Topic sends the messages to all the event subscribers.
#### Amazon MQ
- Managed message broker service for RabbitMQ and ActiveMQ in the cloud
### Cloud Monitoring
#### CloudWatch
- Provides many metrics for every service in AWS.
- **Metric** is a variable to monitor with timestamps.
- Can create **dashboards** of these metrics.
- **CloudWatch Alarms**: used to trigger notification for any metric.
	- You can auto scale, do EC2 actions and also do SNS Notifications from the alarm.
- **CloudWatch Logs**: real time monitoring of logs.
	- Need to run a **CloudWatch Logs Agent** on EC2 to push the log files. Can be setup on premises as well
#### Amazon EventBridge
- Schedule Cron Jobs, trigger lambda functions, send SQS/SNS messages.
#### Amazon CloudTrail
- **Governance, compliance, audit** for AWS accounts.
- Get history of events/API calls made by Console, SDK, CLI, AWS services.
- Can put CloudTrail logs into CloudWatch Logs or S3.
#### AWS X-Ray
- Debugging in production is hard especially with distributed services.
- X-Ray provides with a visual analysis of our applications.
- Makes it easy to troubleshoot bottlenecks.
- Understand dependencies in a microservice architecture.

![[ExampleOfXRayAWS.png| center | 600]]
<p style="text-align:center">Example of AWS X-Ray</p>
#### Amazon CodeGuru
- **ML-Powered service** for automated code reviews and application performance recommendations.
- Two types of services:
	- **CodeGuru Reviewer**: automated code reviews for static code analysis.
	- **CodeGuru Profiler**: visibility/recommendations about application performance during runtime.
#### Amazon Health Dashboard - Service History
- Shows all regions, all services health.
- Shows historical information for each day.
#### Amazon Health Dashboard - Your Account
- Provides **alerts and remediation guidance** when AWS is experiencing events that may impact you.
- Shows how AWS outages directly impact you and your AWS resources.
- Global service

### VPC Section 

#### What is a Virtual Private Cloud (VPC)?
A VPC is a secure, isolated private cloud which is hosted within a public cloud. 

| Private Cloud                 | Public Cloud                                                                                              |     |
| ----------------------------- | --------------------------------------------------------------------------------------------------------- | --- |
| Only used by one organization | Not limited to one organization. Owned and operated by third party cloud providers (AWS, Microsoft Azure) |     |
| Managed by one organization   | Managed by the third-party organization                                                                   |     |
| Dedicated Infrastructure      | Shared Infrastructure                                                                                     |     |

A good analogy is thinking of the public cloud as a restaurant and the virtual private cloud being a reserved table at that restaurant. Even though the restaurant is public, only the people that reserved the table would be able to access it. 

A VPC uses three components to enable isolation and security over a public cloud:
1. **Subnets**: Range of IP addresses in the network that are reserved (i.e. Not available for everyone).
	- **Public Subnets**: Subnets which are accessible over the public internet.
	- **Private Subnets**: Subnets which are **not** accessible over the public internet.
2. **VLAN**: A virtual LAN (Local Area Network) that are connected to each other without the use of the internet. Another way of partitioning the network but on layer 2 and not layer 3 of OSI Model. 
3. **VPN**: A virtual private network for encryption to create a private network over the public network.

Some extra stuff in VPCs:
- **Network Address Translation (NAT)**: A method to map private IP addresses to public IP addresses to access the internet.
	- These help with security as the private networks are not directly exposed over the public internet.
	- These also help with reducing the number of IP addresses used in total as there are not enough IP addresses available in IPv4.

![[VPCDiagram.png| center | 600]]
<p style="text-align:center">Diagram of a VPC</p>
**Network Access Control Lists**
- A firewall to control access from and to a subnet. 
- Can have ALLOW and DENY rules. (Only IP addresses)

**Security Groups**
- A firewall that controls traffic to and from an EC2 Instance.
- Can have ALLOW only. Rules can be IP addresses and other Security Groups.

| Security Groups                                      | NACL                                                    |
| ---------------------------------------------------- | ------------------------------------------------------- |
| Operates at the Instance Level                       | Operates at the Subnet Level                            |
| Supports Allow rules only                            | Supports Allow and Deny rules                           |
| Is stateful: Return traffic is automatically allowed | Is stateless: Return traffic must be explicitly allowed |
#### VPC Flow Logs
- Captures information about IP traffic going into you interfaces:
	- VPC Flow logs
	- Subnet Flow logs
	- Elastic Network Interface Flow Logs
- Helps to monitor and troubleshoot connectivity issues.
#### VPC Peering
- Connect two VPC, privately using AWS network.
- Make them behave as if they are in the same network.
- Must not have overlapping IP address ranges.
#### VPC Endpoints
- Allow secure connectivity between VPC and AWS services
- Uses a private network instead of the public www.
#### AWS PrivateLink (Type of VPC Endpoint)
- Most secure and scalable way to expose a service to VPCs.
- Does not require VPC Peering, Internet gateway, NAT, route tables
- Requires a Network Load Balancer (Service VPC) and Elastic Network Interface (ENI) (Customer VPC)
#### Site to Site VPN
- Connect on premises VPN to AWS.
- Automatically encrypted and goes over the public internet.
#### Direct Connect (DX)
- Establish a physical connection between on-premises and AWS.
- Goes over the private internet. Secure, fast, reliable.
#### AWS ClientVPN
- Connect from your computer using OpenVPN (Open source VPN) to connect to private network on AWS or on premises.
- Makes it seem like you are on the private VPC network.
- Goes over the public internet.
#### Transit Gateway
- Simplify and centralize network connectivity across multiple VPCs, on premises networks.
- hub-and-spoke model. All VPCs, networks connect to one central gateway.
- All the VPCs need one connection to the transit gateway, then it handles the routing between connected networks on its own.
### Security and Compliance

Remember that AWS is responsible for Security **of** the cloud, and the user is responsible for Security **in** the cloud.

Some shared controls between AWS and the user are: Patch management, Configuration Management, Awareness and Training.

**Distributed Denial of Service Attacks**: A type of cyberattack where multiple systems flood a target like websites etc.

AWS provides some services that can be used against DDos protection.
#### AWS Shield
- **AWS Shield Standard**
	- Free service activated for each user 
	- Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other layer 3/layer 4 attacks
- **AWS Shield Advanced**
	- Optional DDos mitigation service. 
	- 24/7 access to AWS DDos Response Team (DRP).
#### AWS Web Application Firewall (AWF)
- Protects application from common web exploits.
- Deploy on Application Load Balancer, API, CloudFront.
- You can also define Web ACLs (Access Control lists)
#### AWS Network Firewall
- Protect your entire Amazon VPC from Layer 3 to Layer 7 protection.
#### AWS Firewall Manager
- Manage security rules in all accounts in AWS organization.
#### Penetration Testing on AWS
- Penetration Testing allowed on 8 services without approval:
	-  Amazon EC2 instances, NAT Gateways, and Elastic Load Balancers
	- Amazon RDS
	- Amazon CloudFront
	- Amazon Aurora
	- Amazon API Gateways
	- AWS Lambda and Lambda Edge functions
	- Amazon Lightsail resources
	- Amazon Elastic Beanstalk environments

- Prohibited Activities
	- DNS zone walking via Amazon Route 53 Hosted Zones
	- Denial of Service (DoS), Distributed Denial of Service (DDoS), Simulated DoS,
	- Simulated DDoS
	- Port flooding
	- Protocol flooding
	- Request flooding (login request flooding, API request flooding)

![[EncryptionAtRestandInTransit.png| center | 600]]
<p style="text-align:center">Encryption of Data at Rest and Data at Transit</p>
#### AWS Key Management Service (KMS)
- Manages Encryption keys for us.
- If you hear "encryption" on the test, it is most likely KMS.

**Types of KMS Keys**
- Customer Managed Key
	- Create, manage, and used by the customer, can enable or disable. 
	- Possibility of key rotation policy
	- Possibility to bring your own key
- AWS Managed Key
	- Created, managed and used on the customer's behalf by AWS.
	- Used by AWS services (aws/s3, aws/ebs, aws/redshift).
- AWS Owned Key
	- Collection of CMKs that an AWS service owns and manages to use in multiple accounts. 
	- AWS uses these to protect resources in your account but you cannot view the keys.
- CloudHSM Keys
	- Keys generated from your own CloudHSM hardware device. 
	- CloudHSM is just a dedicated encryption hardware, to manage your own keys.
#### AWS Certificate Manager (ACM)
- Lets you provision, manage, and deploy public/private SSL/TLS certificates. Basically used for HTTPS.
- Automatically renews TLS certificates.
- Integrations with other AWS services (Elastic Load Balancers, CloudFront Distributions, APIs on API Gateway).
#### AWS Secrets Manager
- Service for storing passwords/secrets.
- Ability to force rotation of secrets every X days. 
- Can automate generation of secrets on rotation.
- Integration with Amazon RDS.
#### AWS Artifact
- Not a service, mainly used for internal audit or compliance.
- A portal to provide customers with on-demand access to **AWS compliance documentation** and **AWS agreements**.
- **Artifact Reports**: Allows you to download AWS security and compliance documents from third-party auditors.
- **Artifact Agreements**: Allows you to review, accept and track the status of AWS agreements.
#### Amazon GuardDuty
- Service that uses machine learning algorithms to protect your AWS account. One click to enable (30 days trial).
- Input data includes
	- CloudTrail Event Logs - unusual API calls, unauthorized deployments
		- CloudTrail Management Events 
		- CloudTrail S3 Data Events
	- VPC Flow Logs - unusual internal traffic, unusual IP address
	- DNS Logs - compromised EC2 instances sending encoded data within DNS queries.
	- Optional Features - EKS Audit Los, RDS & Aurora, EBS, Lambda, S3, Data Events.
- Can setup EventBridge rules to be notified incase of findings.
#### Amazon Inspector
- Automated Security Assessments. **Only for EC2 instances, Container Images and Lambda functions**.
- Reporting and integration with AWS Security Hub.
- **For EC2 instances**
	- Analyze against unintended network accessibility through open ports.
	- Uses the AWS Systems Manager.
	- Analyze the running OS against known vulnerabilities.
- **For Container Images push to Amazon ECR**
	- Assessment of Container Images as they are pushed
- **For Lambda Functions**
	- Identifies software vulnerabilities in function code and package dependencies.
	- Assessment of functions as they are deployed.
#### AWS Config
- Helps with **auditing and recording compliance** of your AWS resources. Per region service.
- Helps record configurations and changes over time.
- Can store the configuration data into S3.
- You can receive SNS notifications for any changes. 
#### AWS Macie
- Fully managed data security and data privacy service that uses **machine learning and pattern matching**.
- For discovering and protecting sensitive data in AWS. (like personally identifiable information \[PII])
![[MacieAWSExample.png| 600 | center]]
<p style="text-align:center">Diagram of Macie with S3 and EventBridge</p>
#### AWS Security Hub
- **Central security tool** to manage security **across AWS accounts** and **automate security checks**.
- Must enable AWS Config service to use this service.
- Automatically aggregates alerts from various AWS services and AWS partner tools.
- Integrated dashboards showing current security and compliance status to quickly take action.
![[AmazonDetectiveAWS.png| 600 | center]]
<p style="text-align:center">Diagram of Security Hub Usage</p>
#### Amazon Detective
- GuardDuty, Macie, Security Hub -> identify potential security issues or findings.
- Amazon Detective -> identify root cause of security issues or suspicious activities.
- This service analyzes, investigates, and quickly identifies the **root cause of security issues** or suspicious activities.
- Uses Machine Learning and Graphs.
- Produces visualizations with details and context to get to the root cause.
- Automatically collects and processes events from VPC Flow Logs, CLoudTrail, GuardDuty and creates a unified view.
#### AWS Abuse
- Report suspected AWS resources used for abusive or illegal purposes.
- Abusive and Prohibited behaviours are
	- **Spam** - receiving undesired emails from AWS-owned IP address, websites and forums spammed by AWS resources.
	- **Port Scanning** - sending packets to your ports to discover the unsecured ones.
	- **DoS or DDoS attacks** - AWS-owned IP addresses attempting to overwhelm or crash your servers/softwares.
	- **Intrusion attempts** - logging in on your resources
	- **Hosting objectionable or copyrighted content** - distributing illegal or copyrighted content without consent.
	- **Distributing malware** - AWS resources distributing softwares to harm computers or machines.
#### Root user privileges
- Not a service.
- Root user = Account Owner.
- Has complete access to all AWS services and resources.
- Not to be used for everyday, administrative tasks.
- Actions performed only by the **root user**:
	- Change account settings (account name, email address, root user password)
	- View certain tax invoices
	-  Close your AWS account
	- Restore IAM user permissions
	- Change or Cancel AWS support plan
	- Configure an Amazon S3 bucket to enable MFA.
#### IAM Access Analyzer
- Find out which resources are shared externally
- Helps define **zone of trust** i.e. what resources are being used by AWS Account or AWS organization and what resources are being used by outside of the AWS accounts.

![[ZoneOfTrustAWS.png| 600 | center]]
<p style="text-align:center">Zone of Trust Diagram</p>
### Machine Learning Section
#### Amazon Rekognition
- Find objects, people, text, scenes in **images** and videos using ML.
- Facial analysis and facial search to do user verification, people counting.
#### Amazon Transcribe
- Automatically convert **speech to text**
- Uses deep learning process called automatic speech recognition (ASR) to convert speech to text quickly and accurately.
- Use cases
	- Transcribe customer service calls.
	- Automate closed captioning and subtitling.
#### Amazon Polly
- Turn text into lifelike speech using deep learning.
- Allowing you to create applications that talk.
#### Amazon Translate
- Natural and accurate **language translation**
#### Amazon Lex and Connect
- **Amazon Lex** (Alexa)
	- Automatic Speech Recognition (ASR) to convert speech to text
	- Natural language understanding to recognize the intent of text, callers
	- Helps build chatbots, call centre bots.
- **Amazon Connect**
	- Receive calls, create contact flows, cloud-based **virtual contact centre**.
	- Can integrate with other Customer Relation Management systems or AWS.
#### Amazon Comprehend
- For Natural Language Processing
- Fully managed and serverless.
- Uses Machine Learning to find insights and relationships in text.
- Sample use case: analyze customer interactions (emails) to find what leads to a positive or negative experience.
#### Amazon SageMaker
- Fully managed service for developers/data scientists to build ML models.
#### Amazon Kendra
- Fully managed **document search service** powered by Machine Learning.
- Extract answers from within a document.
#### Amazon Personalize
- Fully managed ML-service to build apps with real-time personalized recommendations.
- Example: personalized product recommendations/re-ranking, customized direct marketing.
- Also used by amazon.com
- Implement in days, not months (you don't need to build, train, and deploy ML solutions)
#### Amazon Textract
- Automatically extracts text, handwriting, and data from any scanned documents using AI and ML.
- Extract data from forms and tables.
- Read and process any type of document (PDFs, images)
- Example: Financial services (invoices, financial reports etc.)
### Account Management, Billing, and Support Section
#### AWS Organizations
- Global service that allows you to manage **multiple AWS accounts**.
- Main account is the master account.
- **Cost benefits**
	- **Consolidated billing** across all accounts - single payment method
	- Pricing benefits from **aggregated usage** (volume discount for EC2, S3, etc.)
	- Pooling of Reserved EC2 instances for optimal savings.
- Restrict account privileges using **Service Control Policies (SCP)**.
- Can also use API to automate AWS account creation.

A good multi account strategy to follow is to create accounts per department, per cost centre, per dev/test/production based on **regulatory restrictions** (SCP) for better resource isolation.

![[AWSOrgnizations.png| 600 | center]]
<p style="text-align:center">Example of AWS Organization with Organizational Units (OU)</p>

##### Service Control Policies (SCP)
- Whitelist or blacklist IAM actions.
- Applied at the **Organization Unit** or **Account** level.
- Does not apply to the master account.
	- If you attach an SCP that denies `s3:DeleteBucket` to an OU, all member accounts in that OU **cannot delete S3 buckets**. But the **management account can still delete S3 buckets**, regardless of the SCP.
- SCP is applied to all the **Users and Roles** of the Account, including Root user.
- SCP does not affect service-linked roles
	- **service-linked roles** are the roles which are created to be used by AWS services. For example, Amazon ECS uses a role to register and deregister container instances.
- SCP must have an explicit allow (does not allow anything by default).
- Example: restrict access to certain services.
![[SCPBasedOnIAM.png| center | 600]]
<p style="text-align:center">Example of a IAM policy that if applied to an OU will be called an SCP</p>
##### Consolidated Billing
- **Combined Usage** - combines usage across all AWS accounts in AWS Organization to **share volume pricing, reserved instances, and Savings Plans discounts**
- **One Bill** - get one bill for all AWS Accounts in the AWS Organization
- Management account responsible for turning off discount sharing for any AWS Organization account.
#### AWS Control Tower
- Easy way to **set up and govern** a secure and compliant **multi-account AWS environment**. 
- Automates the set up of your environment in a few clicks.
- Automate ongoing policy management using guardrails.
- Runs on top of AWS Organizations.
#### AWS Resource Access Manager (AWS RAM)
- Share your AWS resources with other AWS accounts.
- AWS resources can be shared with any account, **inside or outside**, of your AWS Organization. 
- Helps avoid resource duplication.
#### AWS Service Catalog
- A service that allows organizations to **create, manage, distribute catalogs of IT services** which are approved for use on AWS.
- Provides a quick **self-service portal** to launch **authorized products pre-defined by admins**. 
- Main goal is to help organizations standardize deployments and enforce governance while still empowering teams to deploy the resources they need.
![[ServiceCatalogDiagram.png| center | 600]]
<p style="text-align:center">Service Catalog Diagram</p>
#### Pricing Models in AWS
- Four pricing models
	1. **Pay as you go**: pay for what you use, remain agile, responsive, and meet scale demands.
	2. **Save when you reserve**: minimize risks, predictably manage budgets, comply with long-terms requirements.
	3. **Pay less by using more**: volume-based discounts
	4. **Pay less as AWS grows**: pay less when AWS improves whether through infrastructure, or operational efficiency. Happens automatically.
##### Free services and free tier in AWS
- IAM
- VPC
- Consolidated Billing
- Elastic Beanstalk
- CloudFormation
- Auto Scaling Groups
(For the last three, you pay for the underlying resources created by the services.)
##### Compute Pricing – EC2
- **On-demand instances**
	- Minimum of 60s.
	- Pay per second (Linux/Windows) or per hour (other).
- **Reserved instances**:
	- Up to 75% discount compared to On-demand on hourly rate.
	- 1 or 3 years commitment.
	- All upfront, partial upfront, no upfront (more money paid upfront, the cheaper it is).
- **Sport instances**
	- Up to 90% discount compared to On-demand on hourly rate.
	- Bid for unused capacity.
- **Dedicated Host**
	- On-demand.
	- Reservation for 1 or 3 years commitment.
- **Savings plans** as an alternative to save on sustained usage
##### Compute Pricing – Lambda and Elastic Container Service (ECS)
- **Lambda**
	- Pay per call
	- Pay per duration
- **ECS**
	- EC2 Launch Type Model: No additional fees, you pay for AWS resources stored and created in your application (EC2, EBS).
	- AWS Fargate Launch Type Model: you pay for the amount of vCPU and memory resources that your containerized application requests.
	- AWS Outposts Launch Type model: ECS control plane is in the cloud and your container instances run on the Outposts EC2 capacity with no additional charge.
##### Storage Pricing – S3
- **Storage class**: S3 Standard, S3 Infrequent Access, S3 One-Zone IA, S3 Intelligent Tiering, S3 Glacier and S3 Glacier Deep Archive.
- **Number and size of objects**: Price can be tiered based on volume.
- **Number and type of requests**
- **Data transfer OUT of the S3 region**
- **S3 transfer acceleration**
##### Storage Pricing – Elastic Block Store (EBS)
- Volume type (based on performance)
- Storage volume in GB per month **provisioned**
- **Input Output Operations Per Second** (IOPS)
	- General Purpose SSD - balanced for most workloads
	- Provisioned IOPS SSD: High performance storage, allows you to specify exact IOPS.
	- Magnetic: cheaper, older spinning disk types for infrequent access storage.
- Snapshots: Added data cost per GB per month.
- Data transfer: Outbound data transfer are tiered for volume discounts.
##### Database Pricing – RDS
- Per hour billing
- Based on Database Characteristics
	- Engine.
	- Size.
	- Memory class.
- Purchase type
	- On-demand.
	- reserved instance 1 or 3 years commitment with optional up-front.
- Backup storage: no additional charge for backup storage up to 100% of your total database storage for a region.
- Additional storage (per GB per month).
- Number of input and output requests per month.
- Deployment type (storage and I/O are variable)
	- Single Avaialbility Zone
	- Multiple Availability Zones
- Data transfer: Outbound data transfer are tiered for volume discounts.
#### Content Deliver – CloudFront
- Pricing is different across different geographic regions.
- Aggregated for each edge location, then applied to your bill.
- Data Transfer Out (Volume discount)
- Number of HTTP/HTTPS requests
##### Networking Costs in AWS
- To reduce networking costs use Private IP instead of multiple Public IP for good savings and better network performance.
- Use same Availability Zone for maximum savings (at the cost of high availability).
#### AWS Compute Optimizer
- **Reduce costs** and **improve performance** by recommending optimal AWS resources for your workloads.
- Helps you choose optimal configurations and right-size your workloads (over/under provisioned).
- Uses Machine Learning to analyze your **resources' configurations** and their **utilization CloudWatch metrics**.
- Supported resources
	- EC2 instances
	- EC2 Auto Scaling Groups (ASG)
	- Elastic Block Store (EBS) volumes
	- Lambda functions
- Recommendations can be exported to S3.
#### Billing and Costing Tools
- **Estimating costs in the cloud**
	- Pricing Calculator
- **Tracking costs in the cloud**
	- Billing Dashboard
	- Cost Allocation Tags
	- Cost and Usage Reports
	- Cost Explorer
- **Monitoring against costs plans**
	- Billing Alarms
	- Budgets
##### AWS Pricing Calculator
- Estimate the cost for your solution architecture
##### AWS Billing Dashboard
- Shows various charts about AWS expenditures.
##### Cost Allocation Tags
- Used to track AWS costs on a detailed level.
- Generated by AWS, and automatically applied to the resource you create. (starts with prefix aws: e.g. aws:createdBy)
- Users can also define tags, they start with prefix user:
- Used for **organizing resources**
- Tags can be used to create **Resource Groups**. These are a collection of resources that share common tags.
##### Cost and Usage Report
- Contains the **most comprehensive** set of AWS cost and usage data available.
- Lists AWS usage for each service category used by an account and its IAM users in hourly or daily line items, as well as any tags that you have activated for cost allocation purposes.
##### Cost Explorer
- **Visualize, understand, and manage** AWS costs and usage over time.
- Create custom reports that analyze cost and usage data.
- Analyze your data at a high level or monthly, hourly.
- Choose an optimal Savings plan.
- Also provides forecast of up to 12 months based on previous usage.
##### Billing Alarms in CloudWatch
- Billing data is stored in CloudWatch us-east-1.
- Actual costs, not projected costs. 
- A simple alarm, not as powerful as AWS Budgets
##### AWS Budgets
- Create budget and **send alarms** when costs exceeds the budget.
- 4 types of budgets: Usage, Cost, Reservation, Savings Plans
- Up to 5 SNS notifications per budget
- You can track utilization for reserved instances (RI)
- 2 budgets are free.
##### AWS Cost Anomaly Detection
- **Continuously monitor cost and usage** with **ML** to detect unusual spends.
- Learns unique, historic spend patterns to detect one-time cost spike and/or continuous cost increases. 
- Can send anomaly detection report with root-cause analysis.
##### AWS Service Quotas
- Notify when you're close to a service quota threshold
- Create CloudWatch Alarms on the Service Quotas console.
- Request a quota increase from AWS Service Quotas or shutdown resources before limit is reached.
##### Trusted Advisor
- High level AWS account assessment.
- Analyze AWS accounts and provide recommendations on 6 categories:
	- Cost optimization
	- Performance
	- Security
	- Fault Tolerance
	- Service Limits
	- Operational Excellence
- Business and Enterprise Support plan
	- Full set of checks.
	- Programmatic Access using AWS Support API.
##### AWS Support Plans
AWS consists of 5 support plans.
- AWS Basic Support Plan
	- 24x7 access to customer service, documentation, whitepapers, and support forums.
	- AWS Trusted Advisor - Access to 7 core Trusted Advisor checks and guidance for provisioning resources to increase performance and security.
	- AWS Personal Health Dashboard - included.
- AWS Developer Support Plan
	- All Basic Support Plan and more.
	- **Business hours email access** to Cloud Support Associates.
	- Unlimited cases and unlimited contacts.
	- Response time based on case severity.
		- General guidance: < 24 business hours
		- System impaired: < 12 business hours
- AWS Business Support Plan (24/7)
	- Intended to be used for **production workloads**.
	- Trusted Advisor - Full set of checks + API access.
	- Access to Infrastructure Event Management for additional fee.
	- Response time based on case severity.
		- General guidance: < 24 business hours
		- System impaired: < 12 business hours
		- Production system impaired: < 4 hours
		- Production system down: < 1 hour
- AWS Enterprise On-Ramp Support Plan (24/7)
	- All of Business Support Plan and more.
	- Intended use for **production or business critical workloads**.
	- Access to **Technical Account Managers (TAM)**.
	- **Concierge Support Team** (for billing and account best practices).
	- Infrastructure Event management, Well-Architected & Operations Reviews.
	- Response time based on case severity.
		- General guidance: < 24 business hours
		- System impaired: < 12 business hours
		- Production system impaired: < 4 hours
		- Production system down: < 1 hour
		- Business-critical system down: < 30 minutes
- AWS Enterprise Support Plan (24/7)
	- All of Business Support Plan and more.
	- Intended use for **mission critical workloads**.
	- Access to **designated** TAM.
	- **Concierge Support Team** (for billing and account best practices).
	- Infrastructure Event management, Well-Architected & Operations Reviews.
	- Access to AWS Incident Detection and Response (for an additional fee)
	- Response time based on case severity.
		- General guidance: < 24 business hours
		- System impaired: < 12 business hours
		- Production system impaired: < 4 hours
		- Production system down: < 1 hour
		- Business-critical system down: < 15 minutes
---
### Advanced Identity Section
#### AWS Security Token Service (STS)
- Enables you to create **temporary, limited-privileges credentials** to access your AWS resources.
- You configure the expiration period for these short-term credentials.
- Use cases
	- IAM Roles for cross/same account access.
	- IAM Roles for Amazon EC2: provide temporary credentials for EC2 instances to access AWS resources.
	- Identity federation: manage user identities in external systems, and provide them with STS tokens to access AWS resources.

![[STSAWSExample.png| center | 600]]
<p style="text-align: center">Diagram of STS Service Workings</p>
#### Amazon Cognito (Simplified)
- Identity for your Web and Mobile application users. 
- Instead of creating them an IAM user, you create a user in Cognito.

![[AmazonCognitoExample.png| center | 600]]
<p style="text-align: center">Diagram of Amazon Cognito</p>
#### What is Microsoft Active Directory (AD)
- A directory service for Windows domain networks, acting as a centralized database and set of services. 
- Manages users, computers groups and other network resources, controlling access and authentication.
#### AWS Directory Services
- AWS Manages Microsoft AD
	- Create your own AD in AWS, manage users locally, supports MFA.
	- Establish trust connections with your on-premise AD.
- AD Connector
	- Directory Gateway (proxy) to redirect to on-premise AD, supports MFA.
	- Users are managed on the on-premise AD.
- Simple AD
	- AD-compatible managed directory on AWS.
	- Cannot be joined with on-premise AD.
#### AWS IAM Identity Center
- One login (single sign-on) for all your
	- AWS accounts in AWS Organizaitons
	- Business cloud applications (Salesforce, Box, Microsoft 365).
	- SAML2.0-enabled applications.
	- EC2 Windows Instances.
- Identity Providers
	- Built-in identity store in IAM Identity Center
	- 3rd party: Active Directory (AD), OneLogin, Okta
### Other AWS Services
#### Amazon WorkSpaces
- Managed Desktop as a Service (DaaS) solution to provision Windows or Linux desktops.
- Great to eliminate management of on-premise VDI (Virtual Desktop Infrastructure).
- Fast and quickly scalable to thousands of users.
- Secured data - integrates with Key Management Service.
- Pay-as-you-go service with monthly or hourly rates.
#### Amazon AppStream 2.0
- Desktop Application Streaming Service
- Deliver to any computer, without acquiring and provisioning infrastructure.
- The application is deliver from within a web browser.

| Amazon Workspaces                                                 | Amazon AppStream 2.0                                                       |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Fully Managed VDI and desktop available.                          | Stream a desktop application to web browsers (No need for a VDI).          |
| The users connect to the VDI and open native or WAM applications. | Works with any device with a web browser.                                  |
| Workspaces are on-demand or always on.                            | Allows to configure an instancy type per application type (CPU, RAM, GPU). |
#### AWS IoT Core
- IoT stands for "Internet of Things". It is the network of internet-connected devices that are able to collect and transfer data.
- AWS IoT Core allows you to easily connect IoT devices to the AWS Cloud.
- Serverless, secure, and scalable to billions of devices and trillions of messages.
#### Amazon Elastic Transcoder
- It is used to convert media files stored in S3 into media files in the format required by consumer playback devices (phones, laptops etc.).
- Benefits
	- Easy to use
	- Highly scalable - can handle large volumes of media files and large file sizes.
	- Cost effective - duration-based pricing model.
	- Fully managed and secure, pay for what you use.
#### AWS AppSync
- Store and sync data across mobile and web apps in real-time.
- Makes use of GraphQL.
- Client Code can be generated automatically.
- Integrations with DynamoDB/Lambda.
#### AWS Amplify
- Set of tools and services that help you develop and deploy scalable full stack web and mobile applications.
- Authentication, Storage, API (REST, GraphQL), CI/CD etc.
#### AWS Infrastructure Composer
- Visually design and build serverless applications quickly on AWS.
- Deploy AWS infrastructure code without needing to be an expert in AWS.
- Configure how your resources interact with each other.
- Generates Infrastructure as Code (IaC) using CloudFormation.
- Ability to import existing CloudFormation templates to visualize them.
#### AWS Device Farm
- Fully-managed service that tests your web and mobile apps against desktop browsers, real mobile devices, and tablets.
- Run tests concurrently on multiple devices. 
- Ability to configure device settings.
#### AWS Backup
- Fully-managed service to centrally manage and automate backups across AWS services.
- On-demand and scheduled backups. 
- Supports PITR (Point-in-time Recovery).
- Retention Periods, Lifecycle Management, Backup Policies.
- Cross-Region Backup.
- Cross-Account Backup (using AWS Organizations).
#### AWS Elastic Disaster Recovery (DRS
- Quickly and easily recover your physical, virtual, and cloud-based servers into AWS.
- Continuous block-level replication for your servers.
#### AWS DataSync
- Move large amount of data from on-premises to AWS.
- Can synchronize to Amazon S3, Amazon EFS, Amazon FSx for Windows.
- Replication tasks can be scheduled hourly, daily, weekly.
- The replication tasks are incremental after the first full load.
#### Cloud Migration Strategies: The 7Rs
- Retire
	- Turn off things you don't need (maybe as a result of re-architecting).
	- Helps with reducing the surface areas for attacks (more security).
	- Save cost.
	- Focus your attention on resources that must be maintained.
	- Retain
		- Do nothing for now.
		- Security, data compliance, performance, unresolved dependencies.
		- No business value to migrate, mainframe or mid-range and non-X86 Unix apps.
	- Relocate
		- Move apps from on-premises to its Cloud version.
		- Move EC2 instances to a different VPC, AWS account, or AWS Region.
	- Rehost "lift and shift"
		- Simple migrations by re-hosting on AWS (applications, databases, data..).
		- Migrate machines (physical, virtual, another Cloud) to AWS Cloud.
		- No cloud optimizations being done, applications is migrated as is.
	- Replatform "life and reshpe"
		- Example: migrate your database to RDS.
		- Example: migrate your application to Elastic Beanstalk.
		- Not changing the core architecture, but leverage some Cloud optimizations.
		- Save time and money by moving to a fully managed service or serverless.
	- Repurchase "drop and shop"
		- Moving to a different product while moving to the cloud.
		- Often you move to a SaaS platform
		- Expensive in the short term, but quick to deploy
	- Refactor/ Re-architect
		- Reimagining how the application is architected using Cloud Native features. 
		- Driven by the need of the buisiness to add features and improve scalability, performance, security, and agility.
		- Move from a monolithic application to micro-services.
#### AWS Application Discovery Service
- Plan migration projects by gathering information about on-premises data centers.
- Server utilization data and dependency mapping are important for migrations.
#### AWS Application Migration Service (MGN)
- Life-and-shift (rehost) solution which simplifies migrating applications to AWS.
- Converts your physical, virtual, and cloud-based servers to run natively on AWS.
- Supports wide range of platforms, Operating Systems, and databases.
#### AWS Migration Evaluator
- Helps you build a data-driven business case for migration to AWS.
- Provides a clear baseline of what your organization is running today.
- Analyze current state, define target state, then develop migration plan.
#### AWS Migration Hub
- Central location to collect servers and applications inventory data for the assessment, planning and tracking of migrations to AWS.
- Helps accelerate your migration to AWS, automate life-and-shift.
- AWS Migration Hub Orchestrator - provides pre-built templates to save time and effort migrating enterprise apps. 
- Supports migrations status updates from Application Migration Service (MGN) and Database Migration Service (DMS).
#### AWS Fault Injection Simulator (FIS)
- A fully managed service for running fault injection experiments on AWS workloads. 
- Based on Chaos Engineering - stressing an application by creating disruptive events, observing how the system responds, and implementing improvements.
- Helps you uncover hidden bugs and performance bottlenecks.
- Supports the following AWS services: EC2, ECS, EKS, RDS...
- Use pre-built templates that generate the desired disruptions.
#### AWS Step Functions
- Build serverless visual workflow to orchestrate your Lambda functions.
- Can integrate with EC2, ECS, On-premises servers, API Gateway, SQS queues.
- Use case: order fulfillment, data processing, web applications, any workflow.
#### AWS Ground Station
- Fully managed service that lets you control satellite communications, process data, and scale your satellite operations.
- Provides a global network of satellite ground stations near AWS regions.
- Allows you to send satellite data to your AWS VPC within seconds.
#### Amazon PinPoint
- Scalable 2-way (outbound/inbound) marketing communications service.
- Supports email, SMS, push, voice, and in-app messaging.
- Ability to segment and personalize messages with the right content to customers.
- Use cases: run campaigns by sending marketing, bulk, transactional SMS messages.
- Versus Amazon SNS or Amazon SES
	- In SNS & SES, you managed each message's audience, content and delivery schedule.
	- In Pinpoint, you create message templates, deliver schedules, highly-targeted segments, and full campaigns.
### AWS Architecting & Ecosystem Section

#### Well Architected Framework

It is a set of best practices and guidelines developed by Amazon Web Services to help you build secure, high-performing, resilient, and scalable infrastructure for your applications in the cloud.

There are 6 pillars
1. Operational Excellence
2. Security
3. Reliability
4. Performance Efficiency
5. Cost Optimization
6. Sustainability
##### Operational Excellence

This includes the ability to run and monitor systems to deliver business value and to continually improve supporting processes and procedures. 

Design Principles
- Perform operations as code - Infrastructure as code (Write code to define and automate the setup of your infrastructure).
- Make frequent, small, reversible changes so in case of failure, you can revert it.
- Refine operations procedures frequently.
- Anticipate failure.
- Use managed services to reduce operational burden
- Continuously monitor and analyze your system's behaviour (logs, metrics etc.) to quickly detect issues, make informed improvements to your system.

AWS Services
To Prepare
- AWS CloudFormation
- AWS Config
To Operate
- AWS CloudFormation
- AWS Config
- AWS CloudTrail
- Amazon CloudWatch
- AWS X-Ray
To Evolve
- AWS CloudFormation
- AWS CodeBuild
- AWS CodeCommit
- AWS CodeDeploy
- AWS CodePipeline
##### Security

This includes the ability to protect information, systems, and assets while providing business value through risk assessments and mitigation strategies.

Design Principles
- Implement a strong identity foundation by using principle of least privilege with Identity Access Management (IAM).
- Enable traceability - Integrate logs and metrics with systems so that the systems can automatically respond and take action without human intervention.
- Apply security on all layers like edge networks, VPC, subnet, load balance, every instance, Operating System, and application.
- Automate security best practices.
- Protect data in transit and at rest through encryption, access control etc.
- Keep people away from data as much as possible. 
- Prepare for security events by running incident response simulations and use tools with automation to increase your speed for detection, investigation, and recovery.

AWS Services
For Identity and Access Management
- IAM
- AWS Security Token Service (STS)
- Multi-Factor Authentication Token
- AWS Organizations
Detective Controls
- AWS Config
- AWS CloudTrail
- Amazon CloudWatch
Infrastructure Protection
- Amazon CloudFront
- Amazon VPC
- AWS Shield
- AWS Web Application Firewall (WAF)
- Amazon Inspector
Data Protection
- Key Management Service
- Amazon S3
- Elastic Load Balancing (ELB)
- Amazon Elastic Block Storage (EBS)
- Amazon Relational Database Service (RDS)
Incident Response
- IAM
- AWS CloudFormation
- Amazon CloudWatch Events
##### Reliability

It is the ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.

Design Principles
- Test recovery procedures by using automation to simulate different failures or to recreate scenarios that led to failures before.
- Automatically recover from failure by designing for fault tolerance like Multi-AZ and multi-region deployments.
- Scale horizontally by distributing requests across multiple, smaller resources to ensure that they don't share a common point of failure.
- Use auto scaling to satisfy demand without over or under provisioning
- Use automation to make changes to infrastructure

AWS Services

For Foundation
- IAM
- Amazon VPC
- Service Quotas
- AWS Trusted Advisor

For Change Management
- AWS Auto Scaling
- Amazon CloudWatch
- AWS CloudTrail
- AWS Config

Failure Management
- Backups
- AWS CloudFormation
- Amazon S3
- Amazon Route 53
##### Performance Efficiency

This includes the ability to use computing resources efficiently to meet system requirements, and to maintain that efficiency as demand changes and technologies evolve. 

Design Principles
- Use services provided for advanced technologies so that your focus can be more on product development.
- Use serverless architecture to avoid burden of managing servers.
- Be aware of AWS services to ensure that you are using resources efficiently.

AWS Services

For Selection
- AWS AutoScaling
- AWS Lambda
- Amazon Elastic Block Store (EBS)
- Amazon Simple Storage Service (S3)
- Amazon RDS

For Review
- AWS CloudFormation

For Monitoring
- Amazon CloudWatch
- AWS Lambda
##### Cost Optimization 

Includes the ability to run systems to deliver business value at the lowest price point. 

Design Principles
- Pay only for what you use. Do not pay for extra services
- Measure overall efficiency with CloudWatch
- Don't spend money on data centre operations by letting AWS handle the infrastructure.
- Accurate identification of system usage and costs, helps measure return on investment (ROI).
- Use managed services and applications to reduce the cost of ownership.

AWS Services

Expenditure Awareness
- AWS Budgets
- AWS Cost and Usage Report
- AWS Cost Explorer
- Reserved Instance Reporting

Cost-Effective Resources
- Spot Instance
- Reserved Instance
- Amazon S3 Glacier

Matching Supply and Demand
- AWS Auto Scaling
- AWS Lambda

Optimizing Over Time
- AWS Trusted Advisor
- AWS Cost and Usage Report 
##### Sustainability

This focuses on minimizing the environmental impacts of running cloud workloads.

Design Principles
- Understand your impact through performance indicators
- Establish sustainability goals for workloads.
- Maximize utilization to maximize the energy efficiency of the underlying hardware and minimize idle resources.

AWS Services

There are a ton of services that can be used to reduce environmental impacts through efficient use.
- EC2 Auto Scaling, Serverless services like Lambda, Fargate.
#### AWS Well-Architected Tool

Free tool that helps review your architecture against the 6 pillars well-architected framework and adopt architectural best practices.
#### AWS Customer Carbon Footprint Tool

- Track, measure, review, and forecast the Carbon emissions generated from your AWS usage.
- Helps you meet your own sustainability goals.
### AWS Cloud Adoption Framework (AWS CAF)

This helps you build and execute a comprehensive plan for your digital transformation through innovative use of AWS.
#### Difference between CAF and Well-Architected Framework

| Cloud Adoption Framework                                                                                  | Well Architected Framework                                                                              |
| --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Helps organizations plan and manage their cloud transformation journey.                                   | Helps you build cloud applications that are secure, reliable, and efficient.                            |
| This is more focused on strategy when an organization is starting or scaling their move out to the cloud. | This is focused on the technical expertise required to build cloud applications.                        |
| Gives us a high-level roadmap for cloud adoption across departments, leadership, and teams.               | Gives us a hands-on technical guide to design and review your architecture according to best practices. |

AWS CAF has six perspectives
- Business
- People
- Governance
- Platform
- Security
- Operations

![[AWSCAFDiagram.png| center | 600]]
<p style="text-align:center"> AWS CAF Diagram </p>

Business Perspective helps ensure that your cloud investments accelerate your digital transformation ambitions and business outcomes.

People Perspective serves as a bridge between technology and business, accelerating the cloud journey to help organizations more rapidly evolve to a culture of continuous growth, learning, and where change becomes business-as-normal, with focus on culture, organizational structure, leadership, and workforce.

Governance Perspective helps you orchestrate your cloud initiatives while maximizing organizational benefits and minimizing transformation-related risks.

Platform Perspective helps you build an enterprise-grade, scalable, hybrid cloud platform, modernize existing workloads and implement new cloud-native solutions.

Security Perspective helps you achieve the confidentiality, integrity, and availability of your data and cloud workloads.

Operations Perspective helps ensure that your cloud services are delivered at a level that meets the needs of your business.

#### Transformation Domains

Technology - This involved using the cloud to migrate and modernize legacy infrastructure, applications, data and analytics platforms.

Process - This involves modernization of how your business operates by digitizing, automating, and optimizing processes using AWS services.

Organization - Reimagining your operating model. This involves organizing your teams around products and value streams and leveraging agile methods to rapidly iterate and evolve.

Product - This involves reimagining what your business offers by creating new products, services or revenue streams by leveraging power of the cloud.

Envision - This is the starting point. You work with business and technical stakeholders to understand goals and identify where the cloud can create business value.

Align - After you know what you want to do, you assess how ready the organization is across the 6 AWS CAF perspectives.

Launch - You begin executing your vision in a controlled way by delivering small-scale, high-impact cloud projects.

Scale - After successful small-scale cloud projects, you broaden adoption across the organization.

#### AWS Right Sizing

EC2 has many instances types but choosing the most powerful instance type isn't the best choice because of the cloud being elastic. 

Right sizing is the process of matching instance types and sizes to your workload performance and capacity requirements at the lowest possible cost. It also includes looking at deployed instances and identifying opportunities to eliminate or downsize without compromising capacity or other requirements.

It is important to right size
- Before a cloud migration
- Continuously after the cloud onboarding process as requirements change over time.

#### AWS Marketplace

This consists of a digital catalog with thousands of software listings independent software vendors. You can find stuff like custom AMI, CloudFormation templates, Software as a Service, Containers.

#### AWS Professional Services & Partner Network

The AWS Professional Services organization is a global team of experts. They work alongside your team and a chosen member of the AWS Partner Network (APN), this is a network of companies that work with AWS. 

- APN Technology Partners: the companies within APN that specialize in providing hardware, connectivity, or software solutions.
- APN Consulting Partners: companies within APN that provide professional services to help build on AWs.
- APN Training Partners: companies authorized by AWS that deliver official AWS training courses.
- AWS Competency Program: AWS Competencies are granted to APN Partner companies who have demonstrated technical proficiency and proven customer success in specialized solution areas.
- AWS Navigate Program: helps APN Partners become better Partners.
#### AWS IQ

This is a service that connects AWS customers with AWS Certified experts for on-demand, hands-on assistance with AWS projects. Allows customers and experts to engage in project work. Allows video-conferencing, contract management, secure collaboration, integrated billing.
#### AWS re:Post

This is an AWS-managed Q&A service offering crowd-sources expert-reviewed answers to your technical questions about AWS. This is like the stack overflow of AWS.

- Part of the AWS Free tier.
- Questions from AWS Premium Support customers that do not receive a response from the community are passed on to AWS Support engineers.
#### AWS Managed Service (AMS)

This is a service that helps enterprises operate their AWS infrastructure more efficiently and securely by providing ongoing management, monitoring, and automation of core operational tasks.

- AMS offers a team of AWS experts who manage and operate your infrastructure for security, reliability, and availability.
- Fully managed service that implements best practices.
