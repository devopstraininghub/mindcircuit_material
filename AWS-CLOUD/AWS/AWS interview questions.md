#### What is a Key Pair in AWS?
A Key Pair is used for secure SSH access to EC2 instances. 

It includes a public key (stored in AWS) and a private key (downloaded by the user).

You create it in EC2 Dashboard → Key Pairs → Create Key Pair, and use it when launching an EC2 instance.

#### What is an AMI (Amazon Machine Image)?

An AMI is a template used to launch EC2 instances.

It includes the OS, application server, and application.

To create:

AWS Console → EC2 → Actions → "Create Image"

###  Difference between EBS and EFS

| Feature         | EBS (Elastic Block Store)                             | EFS (Elastic File System)                         |
| ------------------- | --------------------------------------------------------- | ----------------------------------------------------- |
| Type            | Block storage                                             | NFS-based shared file storage                         |
| Instance Access | Attached to a single EC2 instance at a time (in one AZ)   | Can be mounted on multiple EC2 instances (across AZs) |
| Performance     | High performance for a single instance                    | Scales automatically with many instances              |
| Use Case        | Databases, boot volumes, applications needing low latency | Shared storage, web servers, container storage        |
| Pricing         | Pay for provisioned size                                  | Pay for actual storage used                           |
| Scalability     | Must manually increase volume size                        | Automatically scales with storage needs               |
| Availability    | Limited to a single AZ (can use snapshots for backup)     | Accessible across multiple AZs (high availability)    |


#### What is the difference between IAM Role and IAM User?

IAM User: An entity representing a person or service with long-term credentials.

IAM Role: A temporary identity assumed by users/services, useful for cross-account access or EC2, Lambda, etc.

#### What are the types of storage in AWS?

EBS – Block storage for EC2.

EFS – Shared file storage across EC2.

S3 – Object storage for any type of file.

Glacier – Archive storage for long-term backups.

Instance Store – Temporary storage tied to EC2 lifecycle.

#### What is Auto Scaling?

Auto Scaling automatically adjusts the number of EC2 instances based on demand using scaling policies (CPU usage, memory, etc.), improving availability and cost-efficiency.

#### What are the types of Load Balancers in AWS?

Application Load Balancer (ALB) – Layer 7 (HTTP/HTTPS), path-based routing.

Network Load Balancer (NLB) – Layer 4, high performance, static IP.

Classic Load Balancer (CLB) – Legacy, supports both L4 & L7 (not recommended).

#### What is Route 53?

Route 53 is AWS’s scalable DNS and domain management service. It supports:

Domain registration

Routing traffic (e.g., latency, geo-based)

Health checks

Private DNS within VPCs

#### How do you secure an S3 bucket?

Enable Block Public Access

Use IAM policies or bucket policies

Enable versioning and encryption (SSE-KMS)

Enable MFA Delete

Use CloudTrail for logging access

#### What is VPC and what components does it have?

VPC (Virtual Private Cloud) is your isolated AWS network. Components include:

Subnets

Route Tables

Internet Gateway / NAT Gateway

NACLs & Security Groups

VPC Peering / Transit Gateway

####  What is RDS MySQL?

A managed MySQL database hosted on Amazon RDS.

* Automated backups, patching, and failover
* Supports Multi-AZ and read replicas
* No OS-level maintenance required

####  Explain VPC Peering (3-way: A ↔ B ↔ C)?

To enable full communication among 3 VPCs (A, B, C), you need:

* A ↔ B
* B ↔ C
* A ↔ C
  Each VPC peering is bidirectional but not transitive — you must explicitly peer each pair.


####  Difference between  Elastic IP and Public IP in AWS

| Feature     | Elastic IP (EIP)        | Public IP                    |
| -- | -- | - |
| Type        | Static                  | Dynamic (changes on restart) |
| Association | User-managed            | AWS-assigned                 |
| Persistence | Remains unless released | Released when instance stops |

####  How to Monitor EC2 with CloudWatch?

* Enable Detailed Monitoring for 1-minute metrics
* Use CloudWatch Agent to monitor:

  * Memory
  * Disk
  * Custom application logs
* Create alarms for CPU, status checks, etc.


####  What are AWS Step Functions?

A serverless orchestration service that connects AWS services using state machines.

* Automates workflows like data processing, ETL, microservices coordination
* Supports retries, branching, and parallel execution
* Visual workflow editor available

#### What is a NAT Gateway? And what is its use?

A NAT Gateway is a service in AWS that lets servers in a private subnet connect to the internet, but it blocks internet traffic from coming in.

- Why is it used?
To allow private servers (like app servers or databases) to download updates or connect to the internet (e.g., for APIs).

To keep those servers safe by not exposing them directly to the internet.

Example:
You have a private EC2 instance with no public IP.

It needs to install software from the internet.

The traffic goes through the NAT Gateway, which sits in a public subnet, and then out to the internet.

#### Difference between NAT Gateway and Internet Gateway
| Feature              | NAT Gateway                                             | Internet Gateway                                                         |
| -- | -- | - |
| Purpose          | Allows outbound internet access for private subnets | Enables both inbound and outbound internet access for public subnets |
| Used By          | Private instances (no public IP)                            | Public instances (with public IP)                                            |
| Inbound Traffic  |  Not allowed                                               |  Allowed (if security groups allow)                                         |
| Outbound Traffic |  Allowed                                                   |  Allowed                                                                    |
| Placement        | In a public subnet                                      | Attached to the VPC                                                      |
| Managed by AWS   |  Yes                                                       |  Yes                                                                        |
| Needs Public IP? |  No (for private EC2s)                                     |  Yes (for EC2 to be reachable)    
                                          
#### What is AWS CloudTrail and how do you manage it in your project?

CloudTrail logs all API calls and activities across AWS accounts (who did what, when, from where).
We manage CloudTrail by:

Storing logs in versioned S3 buckets

Enabling multi-region trails

Using CloudWatch Logs integration to trigger alerts (e.g., for unauthorized actions)

Applying S3 bucket policies and encryption with KMS for security

#### Suppose your team member is accessing an AWS service, and you want to revoke their access?

Update or remove their IAM policy or role assignment

Use Service Control Policies (SCPs) in AWS Organizations (if in multi-account setup)

For immediate revocation: disable or delete the IAM user, or rotate their credentials

#### How do you connect from a public subnet to a private subnet?
Use bastion host (jump server) in the public subnet

Connect via SSH:
```ssh -i key.pem ec2-user@<bastion-ip> → ssh into private IP```


#### Why do we use a private subnet in a VPC?

For enhanced security. Resources (like databases) in private subnets are not accessible from the internet.

#### How many AWS accounts are used in your current project?

Usually at least 3: Dev, Staging, Prod. Sometimes separate accounts for Security, Billing, or Sandbox.

#### How can you download files from an S3 bucket within AWS while keeping public access completely blocked?

Use pre-signed URLs, IAM roles, or VPC endpoints to download securely without public access.

#### Have you worked with AWS Transit Gateway in your project?

Transit Gateway helps in centralized VPC-to-VPC and on-prem connectivity across accounts. It simplifies the network mesh.

#### How should developers write code to access AWS services (S3, SQS, SNS, RDS)?

- Use AWS SDKs
- Access via IAM roles with least privilege
- Store secrets in Secrets Manager
- Follow retry/backoff logic
- Use env variables for config
- Log via CloudWatch

#### How to handle developer authentication to AWS?

- Use IAM users/groups with MFA
- Prefer IAM roles with temporary STS credentials
- Use AWS SSO or federation
- Manage secrets via AWS Vault or CLI profiles
- Rotate creds, no hardcoding

#### How to enable communication between different VPCs?

- VPC Peering: Direct, simple, but no transitive routing
- Transit Gateway: Centralized hub, scalable
- PrivateLink: For exposing services, not full VPC access
- VPN: For secure cross-region or hybrid setups

#### What is an Internet Gateway, and where is it placed?

An Internet Gateway (IGW) is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the internet.
It is attached at the VPC level, not the subnet level.

#### What is VPC Peering, and how do you configure it?

VPC Peering connects two VPCs to route traffic using private IPs.
Steps to configure:
Create a VPC peering connection.
Accept the request from the target VPC.
Add route table entries in both VPCs.
Ensure security groups and NACLs allow traffic.

#### How do you give private access to an S3 bucket?


Remove public access settings.
Attach bucket policy that allows access from specific VPC or IAM roles.
Use VPC endpoint for S3 for private access without using the internet.

#### What is CloudFront?

CloudFront is AWS’s Content Delivery Network (CDN) that caches content at edge locations to reduce latency and speed up delivery.

#### What are bucket policies in AWS S3?
Answer
Bucket policies are JSON-based rules that control access to an entire S3 bucket or objects inside.

Example:
```
{
  "Effect": "Allow",
  "Principal": "*",
  "Action": ["s3:GetObject"],
  "Resource": "arn:aws:s3:::mybucket/*"
}
```
#### If you get a 403 error (Access Denied) in S3, what do you check?


Check bucket policy
Check IAM role/user permissions
Verify if object ACL is private
Ensure correct Region and signed URL, if applicable

#### We check the policy: GetObject, PutObject — why?
These are the S3 actions needed to download (GetObject) or upload (PutObject) files.
Without them, you’ll get 403 errors.

#### 81Q. What is 2/2 check in AWS EC2?
EC2 passes 2 checks:
System status check
Instance status check
Both must be "2/2 checks passed" for healthy status.

#### Why did you use S3 in your project?
Amazon S3 to store and manage static assets such as:

Images

Logs

Backups

Application data (like JSON, CSV, etc.)

The main reasons were:

Durability & Availability: S3 provides 99.999999999% durability, so my data was safe.

Scalability: I didn't have to worry about storage limits.

Cost-effective: I only paid for what I used, and it's cheaper than running EC2 for storage.

Integration: Easily integrates with other AWS services like Lambda, CloudFront, and EC2.

Access control: I could control access using IAM policies or pre-signed URLs.

#### What is a bucket policy in AWS?

JSON document attached to a bucket to define access rules for users, roles, or the public.

#### What is AWS Serverless and what are its key benefits?

AWS Serverless refers to a cloud-native development model that allows you to build and run applications without managing servers. Key services include:

•	AWS Lambda – run code in response to events.

•	Amazon API Gateway – expose APIs.

•	Amazon DynamoDB – NoSQL database.

•	AWS Step Functions – orchestrate workflows


#### What is AWS VPC peering?

 VPC peering is a networking connection between two VPCs that enables routing traffic between them using private IPs. Peering works across regions and accounts but does not support transitive peering.

#### What AWS resources have you worked with?
I’ve worked with a wide range of AWS resources, including:

•	EC2 (virtual machines)

•	S3 (object storage)

•	RDS (managed databases)

•	ECS/Fargate (containers)

•	Lambda (serverless compute)

•	IAM (access control)

•	CloudWatch (monitoring/logs)

•	VPC (networking)

•	Route 53 (DNS)

•	Auto Scaling Groups

•	ALB/NLB (load balancers)

•	Elastic Beanstalk, CloudFormation, and Terraform for provisioning

#### What AWS services do you use for scaling instances?

For automatic and manual scaling, I’ve used:

•	Auto Scaling Groups (ASG): Automatically add/remove EC2 instances based on CPU, memory, or custom metrics.

•	Elastic Load Balancer (ELB): Distributes traffic across instances to balance load.

•	CloudWatch Alarms: Used to trigger scaling actions.

•	ECS with Fargate or EC2: Task-based scaling based on request load or queue depth.

### Difference between ALB and NLB

| **Feature**             | **ALB (Application Load Balancer)**              | **NLB (Network Load Balancer)**                       |
| ----------------------- | ------------------------------------------------ | ----------------------------------------------------- |
| **Layer**               | Layer 7 (Application Layer)                      | Layer 4 (Transport Layer)                             |
| **Protocols Supported** | HTTP, HTTPS, WebSocket                           | TCP, UDP, TLS                                         |
| **Use Case**            | Web apps, microservices, APIs (advanced routing) | High-performance, low-latency, real-time applications |
| **Routing Type**        | Content-based routing (URL, host, headers, etc.) | Forwarding based on IP address and port               |
| **Target Types**        | EC2 instances, IP addresses, Lambda functions    | EC2 instances, IP addresses                           |
| **TLS Termination**     | Supported                                        | Supported                                             |
| **Health Checks**       | Application-level (HTTP/HTTPS)                   | Network-level (TCP/HTTP/HTTPS)                        |
| **Static IP Support**   | Not directly (uses DNS name)                     | Yes (supports Elastic IPs)                            |
| **Performance**         | Optimized for HTTP/S traffic                     | Optimized for extreme performance and high throughput |



#### How do you connect to a private subnet?


•	Use a bastion host (jump box) in the public subnet.

•	Or use Session Manager (SSM) if agents are installed.

•	Optionally use a VPN or Direct Connect.

#### What is OAI (Origin Access Identity) in CloudFront?

OAI is used to restrict access to an S3 bucket so only CloudFront can fetch content, preventing direct access via S3 URL.

#### What is SNS and how do you create it?

SNS (Simple Notification Service) is an AWS service used to send notifications via email, SMS, HTTP, or Lambda.

To create it:

Go to AWS SNS in the console.

Click “Create topic” → Choose Standard or FIFO.

Name the topic and click Create.

To add a subscriber, choose the topic → Create subscription → select protocol (e.g., Email) and provide the endpoint.

#### How can you recover a deleted S3 object?

 If versioning was enabled, I can retrieve the deleted object using a previous version. Without versioning, the object is permanently deleted unless S3 backup (e.g., replication or lifecycle rule to Glacier) is configured.

#### How do you secure a 3-tier architecture?

 How do you secure a 3-tier app (web, app, DB)?
 I use security groups and NACLs to isolate layers:

•	Web tier: Public subnet with limited inbound (HTTP/HTTPS).

•	App tier: Private subnet, allows traffic only from web tier.

•	DB tier: Private subnet, accessible only by app tier.

Enable encryption (TLS, KMS), IAM roles, and monitoring (CloudWatch, GuardDuty).

#### How many VPCs can you create per region?

By default, you can create 5 VPCs per region per AWS account. This limit can be increased by requesting a quota increase from AWS.

#### What is the difference between a private and public subnet?

•	Public Subnet: A subnet that is associated with a route table that has a route to an Internet Gateway (IGW). Resources in this subnet can access the internet.

•	Private Subnet: A subnet that does not have a route to the Internet Gateway. Used for internal resources like databases.

#### What is a Transit Gateway?

An AWS Transit Gateway enables you to connect multiple VPCs and on-premises networks through a central hub, simplifying your network architecture and reducing the number of peering connections.

#### What is a VPC Endpoint?

A VPC Endpoint allows private connection between your VPC and AWS services (like S3, DynamoDB) without using the internet, improving security and performance.

#### How can you restore an RDS snapshot with a custom database name?

You cannot directly rename a database when restoring a snapshot. Instead:
#### 1.	Restore the snapshot.
#### 2.	Create a new DB instance.
#### 3.	Use tools like pg_dump/mysqldump, or AWS DMS to export and import data into a DB with the desired name.

#### How can you connect S3 to an EC2 instance?


•	Attach an IAM Role to EC2 with S3 access permissions (e.g., AmazonS3ReadOnlyAccess).

•	Use AWS CLI or SDK on EC2:

aws s3 ls s3://your-bucket-name

#### What is the difference between Security Group and Network ACL (NACL)?


| **Feature**          | **Security Group**                               | **Network ACL (NACL)**                                    |
| -------------------- | ------------------------------------------------ | --------------------------------------------------------- |
| **Level**            | Instance-level                                   | Subnet-level                                              |
| **Stateful**         | Yes (automatically allows return traffic)        | No (return traffic must be explicitly allowed)            |
| **Rules**            | Allow rules only                                 | Allow and Deny rules                                      |
| **Applies to**       | EC2 instances and ENIs                           | Entire subnets                                            |
| **Default Behavior** | Deny all inbound, allow all outbound             | Allow all inbound and outbound (by default; customizable) |
| **Rule Evaluation**  | All rules evaluated together (no priority order) | Rules evaluated in number order; first match wins         |
| **Use Case**         | Fine-grained control over instance traffic       | Broad control over subnet-level traffic                   |


### EC2 Instance Types (Based on Use Case)

| **Instance Series** | **Type**                    | **Use Case**                                  | **Examples**                |
| ------------------- | --------------------------- | --------------------------------------------- | --------------------------- |
| `t-series`          | Burstable General Purpose   | Low-cost, spiky workloads                     | `t2.micro`, `t3.small`      |
| `m-series`          | General Purpose             | Balanced compute, memory, networking          | `m5.large`, `m6g.medium`    |
| `c-series`          | Compute Optimized           | High-performance compute workloads            | `c5.large`, `c6g.xlarge`    |
| `r-series`          | Memory Optimized            | In-memory DBs, real-time analytics            | `r5.large`, `r6g.xlarge`    |
| `i-series`          | Storage Optimized           | High IOPS, low-latency storage workloads      | `i3.large`, `i4i.xlarge`    |
| `g/p-series`        | GPU / Accelerated Computing | ML training, AI inference, graphics rendering | `g4dn.xlarge`, `p3.2xlarge` |


#### What is nslookup?
- nslookup is a command-line tool to query DNS for domain or IP info, older than dig. Example: nslookup google.com.

#### What is dnslookup?
- No such standard command. Usually means performing DNS lookup using tools like nslookup or dig.

#### Inode of Lambda function?
- AWS Lambda functions don’t have inodes. Inodes are filesystem metadata; Lambda runs serverless, so no direct inode.

#### Difference between  CloudWatch and CloudTrail

| **Feature**    | **CloudWatch**                               | **CloudTrail**                                 |
| -------------- | -------------------------------------------- | ---------------------------------------------- |
| **Purpose**    | Monitoring performance and resource metrics  | Logging and auditing AWS API calls             |
| **Data Type**  | Metrics, logs, dashboards, alarms            | Management events, API activity logs           |
| **Use Case**   | Operational monitoring, alerting, automation | Security analysis, compliance, governance      |
| **Real-time?** | Yes (near real-time metrics & alarms)        | No (delayed event delivery)                    |
| **Retention**  | Configurable (based on metric/log settings)  | 90 days (default) + long-term in S3 (optional) |


#### Difference between IAM Role and IAM Policy?

IAM Role: A set of permissions that can be assumed by AWS services or users. It is used to grant access to AWS resources without using permanent credentials.
IAM Policy: A JSON document that defines permissions. It is attached to users, groups, or roles to control what actions they can perform.


#### What are the types of IAM policies?

AWS Managed Policies: Predefined and maintained by AWS.

Customer Managed Policies: Custom policies created and managed by you.

Inline Policies: Embedded directly into a single IAM user, group, or role.


#### What is MFA and how do you use it?

MFA (Multi-Factor Authentication) adds an extra layer of security by requiring a second form of verification (e.g., a temporary code from a mobile app).
* You enable it through the IAM console for the root account or individual IAM users.



###  How do you secure your AWS account?

* Enable MFA on the root and all IAM users.
* Use IAM roles and follow the principle of least privilege.
* Regularly rotate access keys.
* Enable CloudTrail for auditing.
* Use AWS Organizations and Service Control Policies (SCPs) if managing multiple accounts.



###  What is Lifecycle policy in S3?

* A lifecycle policy automatically transitions objects to different storage classes (e.g., to Glacier) or deletes them after a defined period.
* Useful for managing cost and data retention.



### 6. What are S3 bucket policies and ACLs?

S3 Bucket Policy: A JSON document that defines access permissions for the entire bucket or specific objects. It supports fine-grained access control.
ACL (Access Control List): A legacy feature used to grant basic access to users at the object or bucket level.



### 7. What is Glacier?

Amazon S3 Glacier is a low-cost cloud storage service for data archiving and long-term backup.
* Retrieval times can range from minutes to hours, depending on the retrieval option.



###  What is a spot instance vs on-demand vs reserved instance?

| **Type**      | **Description**                                            | **Cost**         |
| ------------- | ---------------------------------------------------------- | ---------------- |
| **Spot**      | Uses unused EC2 capacity; can be interrupted anytime       |  *Lowest*      |
| **On-Demand** | Pay-as-you-go with no upfront commitment                   |  *Flexible*    |
| **Reserved**  | Commit for 1 or 3 years in exchange for discounted pricing |  *Cost-saving* |


###  **Difference Between RDS and DynamoDB**

| **Feature**    | **RDS**                                      | **DynamoDB**                              |
| -------------- | -------------------------------------------- | ----------------------------------------- |
| **Type**       | Relational (SQL-based) database              | NoSQL (Key-Value and Document model)      |
| **Scaling**    | Vertical scaling (manual or limited auto)    | Horizontal scaling (automatic)            |
| **Use Case**   | Structured data, complex joins, transactions | Real-time access to unstructured data     |
| **Management** | Managed service (requires DB engine tuning)  | Fully managed (serverless and auto-tuned) |


###  What is Auto Scaling and how does it work?

* Auto Scaling automatically adjusts the number of EC2 instances in response to traffic or load.
* It uses scaling policies and CloudWatch alarms to launch or terminate instances as needed.



###  What is a Target Group?

* A target group is a collection of resources (such as EC2 instances or IPs) that a load balancer routes traffic to.
* Used with ALB and NLB to manage traffic and apply health checks.



###  How do health checks work in ALB?

* ALB sends health check requests (e.g., HTTP GET) to each target at a specific path and port.
* If a target fails health checks repeatedly, it is marked unhealthy and removed from the routing pool until it recovers.



###  Difference between EBS and S3?


| **Feature**    | **EBS (Elastic Block Store)**               | **S3 (Simple Storage Service)**            |
| -------------- | ------------------------------------------- | ------------------------------------------ |
| **Type**       | Block storage                               | Object storage                             |
| **Access**     | Mount as a disk to EC2 instance             | Access via APIs, SDKs, or AWS Console      |
| **Durability** | 99.999%                                     | 99.999999999% (11 nines)                   |
| **Use Case**   | Databases, boot volumes, transactional apps | Backups, static website assets, data lakes |


#### What is a StorageClass in Kubernetes?

A StorageClass defines how dynamic storage volumes are provisioned.
It abstracts the underlying storage type, allowing Kubernetes to automatically create volumes.
Example (AWS): `gp2`, `gp3`, `io1` provision EBS volumes with specific performance characteristics.


### Where is the Terraform state stored?

Terraform stores its state file (`terraform.tfstate`) to track infrastructure changes.

* Locally: In the working directory by default.
* Remotely: For teams and safety, store in Amazon S3 with DynamoDB for state locking and consistency.

### How to Manage Multiple AWS Accounts

Use AWS Organizations:

- Create accounts centrally under a Management Account

- Apply Service Control Policies (SCPs)

- Use Organizational Units (OUs) for grouping

- Enable Consolidated Billing

Tools:
- AWS Control Tower

- IAM Role Switching (sts:AssumeRole)

- AWS CLI Profiles:

[dev]

aws_access_key_id=...

aws_secret_access_key=...

[prod]

aws_access_key_id=...

aws_secret_access_key=...

### Whitelisting SCP on a Server

 1. SSH Port is Open (SCP runs over SSH)

SCP uses port 22 (TCP) by default. To whitelist it:

a. On AWS (Security Group)

* Go to the EC2 console → Security Groups.
* Edit Inbound Rules.
* Add a rule:

  * Type: SSH
  * Protocol: TCP
  * Port Range: 22
  * Source: `<your IP or CIDR>` (e.g., `203.0.113.5/32`)

b. With iptables (Linux firewall)

```bash
sudo iptables -A INPUT -p tcp --dport 22 -s <your_ip_address> -j ACCEPT
```

 2. Ensure SSH is Running

Make sure the `sshd` service is active:

```bash
sudo systemctl status sshd
```


 3. Use SCP for File Transfer

Example:

```bash
scp -i key.pem file.txt user@server:/path/to/destination/
```

### Different Types of AWS Policies

| Policy Type           | Attached To          | Purpose                                 |
|-----------------------|----------------------|-----------------------------------------|
| IAM Policies          | Users, Roles         | Grants permissions                      |
| Resource Policies     | S3, SNS, Lambda      | Grants access to the resource           |
| SCPs                  | Accounts / OUs       | Max boundary for Org accounts           |
| Permission Boundaries | IAM Roles/Users      | Limit what the role/user can do         |
| Session Policies      | STS sessions         | Temporary session-level limits          |
| ACLs                  | Legacy (e.g. S3)     | Basic allow/deny access                 | 


### Bash Script to Update Packages (for AMIs)
Amazon Linux:
#!/bin/bash
yum update -y
yum upgrade -y

Ubuntu:
#!/bin/bash
apt-get update -y
apt-get upgrade -y

Use via user-data or systemd service

### How to Update a Stale EC2 Instance (1 Year Old)

1. Update packages (yum/apt)

2. Reboot if kernel was updated

3. Update SSM and CloudWatch agents

4. Patch application code

5. Replace with latest AMI if outdated

6. Use SSM Patch Manager for automation

### Copy File from S3 to EC2

aws s3 cp s3://your-bucket-name/file.txt /home/ec2-user/file.txt

Ensure EC2 instance has IAM role with s3:GetObject permission

#### What is Elastic Beanstalk?

Elastic Beanstalk is a PaaS offering that deploys applications (Java, Python, .NET, etc.) automatically with provisioning of EC2, ALB, Auto Scaling, and monitoring.

#### What is AWS Session Manager?

A service under AWS Systems Manager that allows secure shell-less access to EC2 instances.

* No need for SSH or key pairs
* Logs sessions to CloudWatch or S3
* Ideal for secure, auditable access

####  What is AWS Backup?

A centralized backup service to automate backup of AWS resources:

* Supports RDS, EBS, DynamoDB, EFS, etc.
* Enables scheduled, compliant, and encrypted backups
* Integrates with AWS Organizations for centralized control
  
####  What is AWS Lambda?

A serverless compute service that runs code in response to events.

* No provisioning or managing servers
* Pay only for execution time
* Common triggers: API Gateway, S3 uploads, DynamoDB streams

