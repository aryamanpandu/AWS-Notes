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
<p style="text-align:center">Diagram of EC2 Image Builder Service</p>

##### Amazon FSX
- 3rd Party High-performance file systems on AWS
- Fully managed service
- **FSx for Windows File Server** or **for Lustre**

| **AWS**                                                                                            | **Customer**                                                                                                    |
| -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Infrastructure                                                                                     | Setting up backup/snapshot procedures                                                                           |
| Responsible for replication of Data for EBS and EFS drives (**Not** EBS and EFS drives themselves) | Setting up data encryption                                                                                      |
| Replacing faulty Hardware                                                                          | Responsible for any data on the drives                                                                          |
| Ensuring that AWS employees cannot access your data                                                | Understanding the risk of using [[Amazon Web Services (AWS) CCP Notes#EC2 Instance Store\| EC2 Instance Store]] |
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

---
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

![[Pasted image 20250429215647.png| center | 600]]
<p style="text-align:center">Encryption of Data at Rest and Data at Transit</p>
#### AWS Key Management Service (KMS)
- Manages Encryption keys for us.
- If you hear "encryption" on the test, it is most likely KMS.

**Types of KMS Keys**
- Customer Managed Key: 
- AWS Managed Key: 
- AWS Owned Key:
- CloudHSM Keys: