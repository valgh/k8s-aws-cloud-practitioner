# AWS Cloud Practitioner Essentials

Notes on the AWS Cloud Practitioner Essentials Amazon Training course 06/02/2023.
AWS Cloud Practicioner Certification: https://aws.amazon.com/certification/certified-cloud-practitioner/

- What is Cloud Computing?
- Global Infrastructure and Reliability;
- Compute in the Cloud;
- AWS Networking;
- Storage and Databases
- Security
- Monitoring and Analytics
- Pricing and Support
- Migration and Innovation

## What is Cloud Computing?

Many many definitions, so we focus on the main advantages of Cloud Computing:

- Access services on demand;
- Avoid large upfront investments;
- Provision computing resources as needed;
- Pay only for what you use (**Pay-as-you-go**);

We have, generally, three deplyoment methods: Cloud-based, On-Prem, Hybrid. 
- **Cloud-based deployment**: all parts run in the cloud, usually everything is migrated on it from on-prem or it is directly designed to be on cloud. 
- **On-prem deployment** means you use virtualization and resource management tools to delpoy resources or increase resource usage. It is also called **Private Cloud**.
- **Hybrid approach**: connect cloud-based resources to on-prem infrastructure, or integrate cloud-based resources with legacy applications. It might happen that, when a customer is migrating from on-prem to cloud, he finds himself in the Hybrid deployment stage until every resource has been migrated.

One of the greatest advantages are the **variable expenses**: On-prem, without a Cloud approach, you have **CAPEX (upfront expenses)** where you invest in technology resources before using them, while on the Cloud you have **OPEX**, as you only pay for what you use (Pay-as-you-go) model. Indeed, migrating to the Cloud means **optimizing your costs as an enterprise**: on-prem you have to run your own data centers and need to invest on the activities to build, maintain and fix such data centers. This cost, when working on the Cloud, is entirely delegated to the Cloud provider (e.g. AWS), so you can invest more and focus on applications and your own customers, redirecting all of your resources that you do not have to invest on data centers anymore, on other activities.
With Cloud deployment, you do not have to guess on your infrastructure capacity needs, but you have the power to **scale in** and **scale out** as needed **dynamically**. In order to scale on-prem, you need to maintain your whole infrastructure (e.g. buy new servers and different hardware pieces and wait for them to arrive): basically, you have to wait weeks between wanting resources and having resources, whereas with Cloud providers this is actually immediate and very, very easy to achieve and it's a matter of minutes, so you have a huge advantage in terms of **speed** and **agility**.
Cloud deployment also enables **economies of scale**: you have a **smaller scale** on-prem, because you pay higher prices based on only your own usage, while with **economies of scale** on Cloud you benefit from customers' aggregated usage.
Last but not least, on the Cloud you can **go global in minutes** by quickly deploying applications worldwide, by exploiting the Cloud Provider's global infrastructure. This is something that you cna also achieve with an on-prem data center, but there are many many limitations to this - one of them being, for example, the latency your customers will experience if you only have one data center in Paris and they have to connect from Sydney.

More on this topic: 
- https://aws.amazon.com/it/what-is-cloud-computing/
- https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html

## Global Infrastructure and Reliability

- **AWS Global Infrastructure**: https://awsvirtual.webex.com/awsvirtual/url.php?frompanel=false&gourl=https%3A%2F%2Faws.amazon.com%2Fabout-aws%2Fglobal-infrastructure

AWS Global Infrastructure is mainly structured in **Regions** and **Availability Zones (AZs)**. Why do we need a global footprint? Basically, if we only had a single point where we deliver our services to the customers, if for some reason such point becomes unavailable, we cannot offer such services anymore, losing potential customers.
AWS Data Centers are divided in **Regions** which identify large areas where AWS servers reside. Users can decide to which Region to connect, switching from one Region to another when needed. Not all services are available on all Regions: there are **31 Regions**, so you have to determine the right Region for your services, data and applications based on:

- **Compliance** with data governance and legal requirements;
- **Proximity** to your customers;
- **Available services** within a Region;
- **Pricing**.

Proximity to the customers is especially interesting in order to reduce latency (check your own latency with AWS Regions' servers here: https://ping.corb.uno/). Also, notice that some services on AWS are Regional (may be available on some Regions but unavailable on others), some others are **Global**, such as **Route 53** (and for obvious reasons, as it is a DNS service!).
Each **Region** contains at least two or more **Availability Zones**. An **AZ** is a cluster of data centers which is completely isolated from the other AZs in their Region: this is done in order to prevent the unavailability of the resources in case of rare but big incidents such as natural disasters. These data centers are **grouped based upon their risk factors**: some may be "weak" to earthquakes, some to floodings, so that the different Availability Zones in a Region should grant the resources to be available in case of one natural disaster, even if the others failed. This way, if AZ1 suffers from a disastrous incident, the applications deployed on such AZ will be made available on another AZ (such as AZ2). An example might be an **AWS S3 bucket** which maintains replicas of a single bucket on at least 3 AZs, or an **Amazon RDS DB** deployed with **Multi-AZ**.

### CDN

**Global Content Delivery**: when a customer needs to access content on a different Region from the one he's in, he might experience a huge latency. In order to prevent this, AWS offers a **Content Delivery Network (CDN)** service, **Amazon CloudFront**, so to deliver and cache contents to **edge locations** distributed all over the world, so that the end customer will access such contents on a closer position, reducing the latency.
Another **CDN** service is **AWS Outposts** (https://aws.amazon.com/it/outposts/), which is AWS hardware that is installed on-prem: it's an hardware solution which extends the AWS infrastructure and services to different locations, including on-prem data centers, and it is often used on Hybrid deployments. AWS will take care of maintaining such hardware.

Each AWS service is available through its own **APIs** and thus can be accessed through:

- **AWS Management Console**;
- **AWS Command Line Interface**;
- **AWS Software Development Kits**.

## Compute in the Cloud

### EC2 

This module explores the Compute services in AWS, where the main service is the **Elastic Compute Cloud (EC2)**. This service empowers the user with **instances or Virtual Machines (VMs)** that AWS creates, secures and manages for them. Virtual Machines are created by the AWS Hypervisor based upon the parameters that the user specifies, such as CPUs number, memory, image type (**AMI**) and so on.
**Pay only for what you use**: AWS users only par for the time the EC2 instances are actually active, so what happens is that the user launches the instance, connects to it and uses it and, when he's done, he can switch it off and AWS won't make him pay for the time the instance is not being used. Thus, the flow goes as follows:

- Launch Instance -> Connect to the instance -> Use the instance -> Switch it off eventually.

Instances can be either stopped or terminated. Terminating means you are going to delete the instance, stopping means the instance will still be there but you won't be able to use it until you run it - **in both cases, terminated and stopped statuses, you won't pay as you will only pay as you are executing your instances**.
Instances can be scaled either **horizontally** (provide more instances, or replicas, of your VMs) or **vertically** (provide more physical resources to your VM, such as CPU or RAM size): notice that in order to **scale vertically**, you **first need to stop your instance**, scale it and then re-deploy it, while scaling horizontally can always be achieved while the instance is still running, as this is an operation that is usually done in order to achieve **HA (High Availability)** on your instances.
When launching and **EC2 instance**, you always have to specify and **instance type**, which defines the type of processors the VM is going to use. There are many different **EC2 instance types**, such as:

- **General Purpose**: it balances compute, memory and networking resources and it's suitable for a broad range of workloads. Example: **T2, T3, M4, A1** instance families;
- **Compute Optimized**: high-performance processors ideal for compute-intensive applications and batch processing workloads;
- **Memory Optimized**: delivers fast performance for memory-intensive workloads, well suited for high-performance databases;
- **Accelerated computing**: uses hardware accelerators (GPU), ideal for streaming and graphics workloads;
- **Storage Optimized**: offers low latency and high I/O per seconds (IOPS), suitable for wokloads such as distributed file systems, big data centers, data warehousing apps.

More on instance types and families: https://aws.amazon.com/ec2/instance-types/

EC2 instances are offered with different **pricing options**:

- **On-Demand**: no upfront costs or minimum usage, it's ideal for short-term and irregular workloads where you can't predict peeks on your workloads;
- **Spot**: ideal for workloads with flexible start and end times, offers up to **90%** savings over On-Demand prices. However, these instances are provided following a best-effort strategy by AWS: it may happen that another customer requires such instances with a different pricing option (reserved, on-demand), so you may lose your instances and AWS will alert you 2 minutes before they will actually be made unavailable for you, that's why you should use Spot Instances if and only if your applications can tolerate interruptions;
- **Reserved Instances (RI)**: provides billing discount over On-demand, requires a 1 or 3 year commitment. To be used when you know you will be using these instances for a long period of time instead of On-Demand;
- **Compute Savings Plans**: offers up to **66% savings over On-Demand** costs for a consistent amount of compute usage, 1 or 3 year term commitment. Same as **RI**, but it's **much more flexible**, as you can use more than just EC2 but also Lambda and Containers/Kubernetes, so other services in the Compute family.
- **Dedicated Instance**: an EC2 instance that runs in a VPC on hardware for a single customer, higher costs compared to standard EC2 instances: these are **Single-Tenant** instances as the customer here is not sharing the underlying infrastructure with the other customers in the AWS Public Cloud;
- **Dedicated Host**: a whole **physical server** with EC2 instance capacity for a single customer, just like Dedicated Instances, most expensive option. Difference between Dedicated Instances and Dedicated Host is that with a Dedicated Instance you are not buying a physical host, so it may change when you switch it off&on, but with a Dedicated Host you are actually buying your own physical server. You can opt for a **Bring Your Own License (BYOL)** with Dedicated Host> every license can be reused within this physical server and you don-t need to buy them again.

### EC2 Load balancing and Auto Scaling

Amazon EC2 Instances can make use of the **Auto Scaling feature**. This is obviously meant only for horizontal scaling, as vertical one has to be achieved by switching off/on the instances, thus not automatically. Why would you need to scale horizontally? What happens is that when the demand changes (low demand/high demand) you manually scale the instances that are available on your backend, so you can handle an increasing or decreasing demand by simply providing more or less EC2 instances (**horizontal scaling**). You can also optimize costs by doing that, as you can scale out/down whenever the demand is low and you don't need to provision as many resources as when the demand is higher, helping the customer save on the whole billing.
**Auto Scaling** makes it possible for the customer to scale capacity as computing requirements change, and you can use **dynamic scaling** and **predictive scaling**. You can decide to **scale as needed** - so you may set a maximum number of instances that you are running - and you can also set a **minimum** and a **desired** number of EC2 instances: EC2 will always try its best to match the **desired** state and will always make sure to run the least **minimum** number that you set. 
Usually, what happens is that the customer instructs **Cloud Watch** in order to monitor some kind of parameter on its EC2 instances, such as the CPU utilization and, whenever a specified value is matched over a certain period of time, decide whether to scale up or down on the resources of the machine or to provide more instances.
When scaling horizontally, it is important to provide an **Elastic Load Balancer (ELB)**: it automatically distributes traffic across mutliple resources, and a single point of contact for a whole **Auto Scaling Group** of instances, so that when you scale and provide more EC2 instances, the **ELB** acts as the single entry point for all of them and then distribute the traffic to each of these instances automatically based upon different variables, such as their health status and workload. The **Load Balancer** continuously monitors all the instances in the **Auto Scaling Group** with health checks, to retrieve information abouth which instances are actually active and which are, instead, faulty.
There are different types of **ELB** dependning on the ISO/OSI level they operate at, mainly:

- **Application ELB** at level 7 (HTTP/S);
- **Network ELB** at level 4 (TCP).

### SNS, SQS, Lambda

When working with a Microservices Application architecture, each microservice in the application communicates with the other **loosely coupled** components with **messages**. **AWS** provides services in order to deploy Microservices-oriented applications directly on the Cloud. One of the messaging services that is targeted for deploying such applications that is provided by Amazon AWS is **Amazon Simple Notification Service (SNS)**, where messages are published to **topics** and those who **subscribe** to such topics immediately receive those messages. In a **fanout** scenario, you can also publish updates for multiple topics.
Another different service for messaging is **Amazon Simple Queue Service (SQS)** where the notification is sent in a **queue** which can be polled from a component, consume each message in it, process them and then erase them one by one, usually with a **FIFO** strategy (but other strategies are also possible).
Computing on AWS can also be achieved through **serverless computing**, and this is a Microservice-oriented way of computing: you avoid to deploy, manage and maintain a server such as a VM (EC2) and you still get to deliver your applications.You only need to provide your code to a **serverless platform** such as **AWS Lambda**, whcih will execute the code without provisioning or managing servers: you pay only for compute time while your code is running. Some other services, such as **Amazon SNS**, may trigger the code you provide to **Lambda**, for example when some kind of notification is received. So, instead of running a whole EC2 instance for a simple task, you only need to trigger the code in **Lambda** when it's needed - so, when for example you have some kind of event that triggers the code such as an upload of a file which has to be handled in some way through your code.

### ECS, EKS

**AWS Elastic Container Services** or **ECS** is the service that lets the customer deploy **containers**, such as **Docker containers**. It runs and scales containerized applications, with simple API calls to control Docker-enabled apps.
**AWS Elastic Kubernetes Service** or **EKS** runs and scales Kubernetes applications and it reqadily updates apps with new features.
**AWS Fargate** is a serverless service that lets you run **serverless containers** so you don't have to deal with instances anymore, and it can work with either **ECS** or **EKS**. It's basically the **Lambda** counterpart for **ECS/EKS**. Once again, you pay only for the resources you use.

## AWS Networking

On **AWS** you can define the **Amazon Virtual Private Cloud (VPC)** which is the VPN defined in your own Cloud. In order to launch instances, you need to choose under which **VPC** you desire to place your instance, thus isolating the virtual area in which your instance will operate. Each **VPC** can be defined in a specific **Region**. This way, **VPC** is a service that enables you to launch resources in a virtual network that you define. You can also define **subnets** in your **VPC**, which is a section in which you can place groups of isolated resources, and they can be either **public or private**, so for example you may have a public subnet for a frontend application and then a private one for your backend or database. In order to make a **public subnet reachable**, you need an **Internet Gateway** to open a port for your subnet and access it from the public internet. On the other end, you need a **Virtual Private Gateway** to access a **private subnet** over the public internet. Another option to make a **private subnet** available from "outside", is that of requesting the **AWS Direct Connect** service, which takes a few weeks for the setup, but grants a **private and dedicated (other than physical) connection** from your corporate data center to the private subnet, still using a Virtual Private Gateway but *without a VPN and a direct connection instead, not going through the public internet*.
With a **VPC Peering**, you can enable communication between instances and services that are deployed in different **VPCs**.

### Firewalling

Access to a **VPC** are granted either through **Network Access Control Lists (NACLs)** or **Security Groups**, which can be defined as two different levels of virtual firewalls. The first one is the **NACL** which controls the access to the **subnet**, the second one to the **EC2 instances**.
**NACL** is a virtual firewall for a **subnet**: the default network ACL activation option **allows** all inbound and outbound traffic. The custom ACLs activation option **denies** all inbound and outbound traffic. You can choose either one or the other and then decide to customize by either white listing or black listing. **NACLs** perform **stateless** filtering so they do not remember previous transactions of packest and *always perform their checks*.
**Security Groups** are a virtual firewall option that controls EC2 instance access: by default, they **deny** all inbound and **allow** all outbound traffic to/from EC2. This way, you can grant access or deny it inbound/outbound however you want. The type of filtering of a **Security Group** is a **stateful** one on the packets, so they remember previous decisions that were made for incoming packets.

### DNS

Amazon also defines a service for providing **Domain Name System (DNS)** on the Cloud to resolve IP addresses: such service is the **Amazon Route 53**. It routes users to internet applications, connects user requests to infrastructure in AWS and outside of it, and manages DNS records for domain names. It's a **global** service so it's not tied to Regions.

## Storage and Databases

In AWS customers can exploit services to provide storage or even databases for their VMs (EC2) or the entire applications.

### AWS Storage

On AWS, we have *three types* of storage:

- **Block Storage**;
- **Object Storage**;
- **File Storage**.

In **Block Storage** files are separated into equal-sized pieces (*blocks*) of data, just like on a physical machine: it is used for applications running on Amazon EC2 instances. We have two types of block storage instances: the first one is the **Instance store**, where you have all the data attached to your EC2 instance, meaning that if you switch it off, you'll lose all the data. The second one is the **Amazon Elastic Block Store (EBS) volumes**: this way, *EC2 instance and block storage are decoupled* and thus they will persist even if you switch off the EC2 instance. You can also choose to *detach* an **EBS volume** and attach it to another EC2 instance while maintaining all the data in it, and scaling the size of the EBS volume up (so increase the GB storage), but cannot decrease it obviously - or you'd lose data. The **EBS volumes** have to be placed in the same **Availability Zone** of the **EC2 instance** it is attached to. You can also perform backups of your **EBS volume** through *snapshots*, the first one being complete, the following ones being *incremental snapshots*.
In **Object storage**, each object consists of data, metadata and a key. The corresponding service is the **Amazon Simple Storage Service (S3)**: it allows the customer to store objects in **buckets**, set permissions to control the access to objects and choose from a range of storage clases for different use cases, other than provide **versioning for the objects in the buckets**. Each bucket has unlimited space, it scales up automatically and you only pay for the space you use. The durability of *S3 buckets* is *99.999999...%*: it automatically replicates data across 3 different AZs, so it's pretty reliable and fault-tolerant.
We have many different *storage classes*:

- **S3 Standard** designed for frequently accessed data, stored across 3 AZs;
- **S3 Sandard Infrequent Access (IA)** ideal for infrequently accessed data, has a lower storage price and higher retrieval price and still stores across 3 AZs;
- **S3 One Zone-IA** just like **S3 Standard/IA** but in a single AZ, even lower storage price;
- **S3 Intelligent-Tiering** is ideal for data with unknown or changing access patterns, through Machine Learning shifts the object from one class to another depending on which class is more convenient to the customer, requires a small monthly monitoring and automation fee per object;
- **S3 Glacier Instant Retrieval** low-cost storage for data archiving, but able to retrieve within milliseconds;
- **S3 Glacier Flexible Retrieval** lowest-cost for data archiving, but retrieval time within minutes or hours;
- **S3 Glacier Deep Archive** lowest-cost for data archiving, but able to retrieve within 12 hours.

In **File Storage**, multiple clients can access data that is stored in shared file folders, as a shared volume between different machines or endpoints. The corresponding service is **Amazon Elastic File System (EFS)**, it stores data in a scalable file system (automatic scaling just like S3), provides data to thousands of EC2 instances concurrently and stores data **in and across multiple AZs**. Underlying environment is **NFS** for *Linux* environments, another service called **EFSx** is instead used for *Windows* file systems.

### AWS Databases

In AWS, we have *Relational (SQL)* and *Non-relational databases (NoSQL)*. You could launch an EC2 instance and there install your database: in this case, you have a *self-managed* database. However, AWS offers also **Managed Databases** through the **Amazon RDS** service, which removes the complexity of managing your DB, updating it, patching it, and so on. You only need to configure a few parameters, such as the size of your database, the license, etc.
Amazon **RDS** operates and scale a *relational database* in AWS Cloud, automating time-consuming administrative tasks (patch, update, backups...) and is compatible with 6 DB engines: *Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL*. **Amazon Aurora** stores data in an enterprise-class relational database, it reduces costs by eliminating unnecessary I/O operations and replicates six copies of data across 3 AZs. *It's 3 times faster than PostgreSQL and 5 times faster than MySQL, designed upon these two open source solutions but optimized for AWS.*
Amazon also offers **DynamoDB**, a *non-relational DB*, serverless and key-value based: it automatically scales to adjust for capacity changes and maintain consistent performance, and is designed to handle over 10 trillion requests per day.
With **AWS Database Migration Service (DMS)** you can migrate entire databases (*relational or non-relational*)to a target database on the Cloud, you can even migrate different ones (e.g. MySQL to Amazon Aurora). **Schema Conversion Tool (SCT)** allows to correct the syntax of the queries, can be helpful when migrating databases.

Additional DB services: *Amazon Redshift* for data warehousing, *Amazon Document DB* runs MongoDB, *Amazon Neptune* is a db for specific datasets such as graphs, *Amazon Quantum Ledger Database (QLDB)* which is a fully managed ledger database that provides a transparent, immutable, and cryptographically verifiable log of data changes, *Amazon Managed Blockchain* designed specifically for blockchain, *Amazon ElastiCache* which is a caching service with Redis or MemCacheDB to accelerate performances by caching, and *Amazon DynamoDB Accelerator* to further increase *DynamoDB* performances.

## Security

AWS defines a **Shared-Responsibility Model** for security: AWS is responsible for the *security **OF** the Cloud*, so everything that is physical, hardware and software infrastructure (hypervisors as software for example), network infrastructure and account management, whereas the customer is responsible for the *security **IN** the Cloud*, such as instance operating system, applications, *security groups*, host-based firewalls, account management and *customer data*. The further you get from on-prem, the more the responsibility shifts towards AWS: e.g., when you exploit a serverless service such as the **AWS Lambda** one, you are only responsible for the code you run, everything else is actually managed by AWS and is responsibility of AWS in terms of security.
A very important Security tool that is available on AWS is the **Identity and Access Management (IAM)** which is a **global** and free service. **IAM** allows you to manage access to AWS Services and resources through different *identities*: usually, you would define **IAM Users**, **IAM Group**, **IAM Policies** or **IAM Roles**. 

### IAM Users

When an account is created on AWS, the very first *identity* that is created is the **root user** identity. The second step would be to create another **IAM User** with admin permissions, given the fact that as a best practice you should not use the *root* account for daily operations in your account. With the *admin user*, you can now create other users as you wish and perform daily administration tasks: only access the root user for a limited number of operations (https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html). A general best practice is that of creating an individual **IAM User** for each person who needs access to AWS, so a one-to-one relationship between physical person and its user *identity*.

### IAM Policies

To grant permissions to a single **IAM User**, you should apply an **IAM Policy**: it's a document that grants or denies permissions to AWS services and resources. Best practice: follow the security principle of **least privilege**, only grant the minimum needed permissions for the tasks the user has to carry out. When defining the policy, you have to choose between either **Allow** or **Deny** as an *Effect*, referring to a specific *Actions* such as *s3:ListObject* or *s3:GetObject*. Under the *Resource* field, you can specify through the **Amazon Resource Name (ARN)** of the services to which resources you are going to allow or deny the aforementioned actions.
Notice that you can also apply **IAM Policies** to specific services in AWS, for example you can apply a *policy* directly to an *S3 bucket* to allow or deny all accesses to such object.

### IAM Group

An **IAM Group** is a collection of **IAM Users**. Groups are very useful, as you can attach **IAM Policies** to **IAM Groups**, rather than to individual users, so members inherit the policies assigned to the group and this way it gets much easier to manage the permissions you grant to the users in your account, especially if you have lots of users.
An user can also be part of more than one group. If in this case, you have conflicts on the policies  (allow on one group but deny on another), the *deny* **always wins**.

### IAM Roles

An *identity* that you can assume to *gain **temporary** access to permissions*: this way an *identity* can assume a specific **role** to access specific resources.

### Multifactor Authentication (MFA)

it provides an extra layer of protection for your AWS account, so that in order to login you need *something you know (password)* and *something you have (OTP, token...)*: one of the first thing to do when creating an AWS account is that of enabling MFA for that account.

### AWS Organizations

What happens if you have dozens or hundreds of AWS accounts? Use **AWS Organizations**! It helps customers consolidate and managing multiple AWS accounts in a central location. You can also use **Service Control Policies (SCP)** which are only available in **AWS Organizations** to centrally control permissions for the accounts in your organization, either by assigning them to single accounts or groups (whose name, in this case, is **Organizational Units (OU)**), or directly to the *root* users so all the other accounts will actually inherit them.

### Compliance

**AWS Artifact** provides on-demand access to security and compliance reports and select online agreements: you can have access compliance reports on demand, review, accept and manage agreements with AWS and access compliance reports from 3rd party auditors.

### Application-level Security

- **AWS Web Application Firewall (WAF)** is a firewall which helps protect your web applications and APIs against common web exploits (SQL injection, XSS...) and works just like the **NACL** by having control lists at application level.
- **AWS Shield** is a service that helps preventing **DDoS attacks**, can be integrated with *AWS WAF*.
- **Amazon Inspector** allows to perform automated security assessments on your applications, identifying security vulnerabilities and deviations from best practices by also providing recommendations in order to fix the issues.
- **Amazon Key Management Service (KMS)** helps customers perform encryption operations through the use of cryptographic keys, and you can choose the specific levels of access control that you need for your keys, deciding who is allowed to list those keys, retrieve them, use them and so on.
- **Amazon GuardDuty** provides intelligent (Machine Learning) threat detection for AWS product and services at account level: compromised credentials, illegal access to data, cross-check with VPC traffic and logs, generates a report with detailed findings and recommended actions. It may also activate a **Lambda** trigger to automatically and immediately react to some kind of threat, or even send a notification through **SNS** whenever it finds some kind of possible threat.

## Monitoring and Analytics

We have three main tools for monitoring and gathering analytics on the AWS account and services/resources.

### Amazon CloudWatch

**Amazon CloudWatch** is a tool which enables monitoring and actions within the AWS Cloud environment. It monitors Cloud and On-Prem infrastructure, gathers all the logs in your environment for all of your services, provides access to all of your metrics from a single location and configures automatic alerts (**SNS**) and actions in response to such metrics, for example through **AWS Lambda**. Through the **CloudWatch dashboard** an administrator gathers and reviews all the metrics about every AWS service, upon which he can create an alarm if some kind of threshold is violated, and define corrective actions or alerts to be sent.

### Amazon CloudTrail

It's a logging system where the customer can track user activities and API requests throughout the AWS infrastructure. You can filter *logs* by API calls to assist with operational analysis and troubleshooting. Logs are saved for **90 days by default**, but can also be exported on S3 buckets to keep them for years. It automatically detects unusual account activity (**CloudTrail Insights**) by exploiting Machine Learning. Each log answers to the following questions: * **What** happened?, **Who** made the request?, **When** did this occur?, **How** was the request made?*

### AWS TrustedAdvisor

Compares your AWS infrastructure to AWS best practices in **5 categories** such as *cost optimization*, not all of them being actually free. Evaluates and implements guidance at all stages of deployment and provides recommendations for corrective actions. For instance, **TrustedAdvisor** may detect EC2 instances that have not been used for a long time, and may suggest to switch the instances to *Spot* pricing model in order to save some money. It also flags security issues.

## Pricing and Support

AWS defines different *pricing models* which also describes different *levels of support* that you can get from Amazon experts.
The first *pricing model* or *tier* is the **Free Tier**, which has three categories: *Always Free*, *12 Months Free*, *Trials*. The **Free Tier** grants a certain amount of hours or gigabytes for specific services so that the customer can actually try them.

### AWS Pricing Concepts

- *Pay as you go*: pay only for the resources that you use without provisioning capacity in advance;
- *Pay less when you reserve*: reduce costs by reserving capacity for those services you know you are going to use for a long time;
- *Pay less with volume-based discounts*: receive savings through volume-based discounts as your usage increases (**economies of scale**: the more you use it, the less you pay).

**AWS Pricing Calculator** is an AWS service which you can exploit to simulate and estimate your costs based upon the pricing model you choose. For example, you may estimate your annual costs for launching ten EC2 instance **On-Demand** with a *t3.micro* and a 100% usage (always running, every day), a single CPU, and compare it with ten **Reserved-Instances** for 1 year or 3 year to check how much you could save.

### Consolidated Billing

Available through **AWS Organizations**: receive a *single bill* for all the AWS accounts in your organization, review itemized charges and share all your savings across all the accounts in your organization. The last item is *very important*, as it means that if you have 3 accounts in your organization that are using *S3 buckets* each with their used size e.g. A1 2TB, A2 5TB and A3 7TB, none of them reaches a 10TB threshold to have an extra discount for values > 10TB. However, thanks to **Consolidated Billing** you can aggregate all these costs so you get 2+5+7 = 14TB > 10TB. This way, you can get your discount for your organization by aggregating the costs of your accounts. A3 contributed more so he will get a major discount, but the others will have it too!

### AWS Budgets, AWS Cost Explorer

**AWS Budgets** is a tool that you can use to set thresholds for your AWS service usage and costs, whereas **AWS Cost Explorer** provides the user with recommendations for savings or reducing costs.

### AWS Support Plans

- **Developer**: access to best practices, client-side diagnostics, building-block architecture support;
- **Business**: *all checks on TrustedAdvisor*, building-block architecture support, etc..;
- **Enterprise On-Ramp**: infrastructure event management, a pool of *Technical Account Managers **(TAM)*** full of experts providing support;
- **Enterprise**;

### AWS Marketplace

It's a digital catalog that provides listings to third-party solutions available on AWS, with several categories: https://aws.amazon.com/marketplace

## Migration and Innovation

Migrating from on-prem to Cloud is easy, but companies may still have lots of questions and doubts, especially when wondering the costs and time of such migration. AWS defines tools and services in order to guide enterprises in the Cloud migration.

### AWS Cloud Adoption Framework (CAF)

A series of best practices and Q&A to provide to the companies to enable a quick and smooth migration to AWS from On-Prem. It organizes the provided guidance into six areas of focus, called **perspectives**: *business, people, governance (business capabilities) - platform, security, operations (technical capabilities)*. **Seven migration strategies** are defined during the **Discovery Phase** of the applications to migrate, 5 that actually migrate the applications, 2 of them that do not or even retire the apps as they are not used anymore: *relocate, rehost, replatform (e.g. MySQL on-prem to MySQL on RDS), refactor/rearchitect, repurchase (move to another software solution, a SaaS) (migration, do migrate these machines) - retain, retire (do not migrate these machines, but rather keep them on-prem or retire them)*.

### AWS Snow Family

A family of hardware tools that makes the migration much easier, which are sent to the customer so that he can work with it by loading in it all the data he wants to migrate, and AWS will take care to put them in the landing zone of the Cloud where he will migrate.

- **AWS Snowcone** smallest, rugged but secure edge computing (up to 14 TB);
- **AWS Snowball** larger device with respect to Snowcone;
- **AWS Snowmobile** it's an entire tir of 100 PetaBytes size for your data, with video surveillance and so on.

### AWS Well-Architected Framework

A document containing best practices which offer guidance in order to understand how to design and operate reliable, secure, efficient and cost/effective systems in AWS. Based on six pillars:

- Operational excellence
- Security
- Reliability
- Performance efficiency
- Cost optimization
- Sustainability
