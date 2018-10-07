# Section 5 - EC2 - The Backbone of AWS 

## 30. EC2 101
#### What
- Elastic Compute Cloud
- Web service that provides resizable compute capacity
- Vertical scaling by changing instance type
- Horizontal scaling by adding instances behind a load balancer
- Pay only for the capacity you actually use, scale only when needed not when you buy a server

#### Pricing
###### On Demand
- Applications whit short term, spiky or unpredictable workloads
- Pay fixed rate by the hour/second
- No up-front payment

###### Reserved
- Applications with predictable usage
- Applications that require reserved capacity
- Up-front payments possible which reduces hourly charge 
- Contract (1y or 3y) for a capacity reservation
- Standard RI
- Convertible RI: option to change the attributes of the RI
- Scheduled RI: only needed on a certain schedule

###### Spot
- Applications with flexible start and end times
- Applications that are only feasible at very low compute prices
- Compute capacity can be sold for very low price on moments AWS has a quiet moment
- Bid a price for instance capacity, when prices drop to your bid your applications will run

###### Dedicated Hosts
- Useful for regulatory requirements
- Physical EC2 server
- Reduce costs by allowing you tu user your existing server-bound software licenses
- On demand or as a reservation

#### Instance Types
- Family: Letter + Generation number
- Mnemonic: FIGHT DR MC PX
- F: Field programmable gate array
- I: High speed storage
- G: Graphics intensive 
- H: High disk throughput
- T: Lowest cost, general purpose
- D: Dense storage
- R: Memory optimized
- M: General purpose
- C: Compute optimized
- P: Graphics, general purpose GPU
- X: Memory optimized

#### EBS
- Elastic block storage (can install OS, DB, file system, ... on it)
- Create storage volumes and attach them to EC2 instances
- Can't be mounted to two EC2 instances at once
- Placed in a specific AZ where they are replicated over multiple disks
- Root device volume
    - EBS volume attached to EC2 instance where windows or linux is installed
    - The data stored on this volume is not persistent by default and will be lost between restarts

###### Volume Types
- General Purpose SSD (GP2): use <1O.OOO IOPS
- Provisioned IOPS SSD (IO1): IO intensive applications, use >10.000 IOPS
- Throughput Optimized HDD (ST1): big data, cannot be a boot volume
- Cold HDD (SC1): lowest cost for infrequent access, cannot be a boot volume
- Magnetic (Standard): lowest cost bootable, legacy


## 31. Launch An EC2 Instance - Part 1
- EC2 instances are managed per region

#### Launch
1. Choose AMI
    - Amazon machine image
    - Snapshot of a virtual machine
    - Amazon linux AMI: AWS CLI, python, ruby, perl, java, ...
2. Choose instance type
    - `t2.micro` is free tier
3. Configure instance details
    - Number of instances
    - Spot pricing
    - Network: VPC
    - Subnet: one subnet is equal to one AZ
    - IAM role: e.g. to access S3
    - Shutdown settings: termination protection prevents termination of the instance without disabling this setting first (off by default)
    - Advanced: installation/bootstrap scripts
4. Add Storage
    - Manage root device volume, encryption encryption is not possible for default AMI's
    - Add additional EBS volumes
5. Add Tags
    - Manage tags
    - Help to control cost and see where costs are coming from
6. Configure Security Groups
    - Virtual firewall
7. Review and launch
    - Select a keypair for SSH
    
#### SSH
1. Open terminal
2. Go to folder where the keypair (`...pem`) is stored
3. Change permissions of keypair: `chmod 400 ...pem` (only the owner can read the file)
4. SSH: `ssh ec2-user@52.59.241.113 -i MyEC2KeyPair.pem`


## 32. Launch An EC2 Instance - Part 2
#### Instance Details
- Description: general information about the instance
- Status checks
    - System status check: verifies that the instance is reachable, if this fails there might be something wrong with the infrastructure hosting the instance
    - Instance status check: verifies that the instance OS is accepting traffic
- Monitoring: basic monitoring (sampled every 5 minutes)
- Tags


## 33. How To Use Putty
- Windows only


## 34. Security Groups Basics
- Virtual firewall
- All inbound traffic is blocked by default
- Possible to add allow rules, not to add deny rules
- All outbound traffic is allowed by default
- Change you make to a security group applies immediately
- EC2 instance can be in multiple security groups. Traffic is allowed if it is allowed by one of the security groups.
- Security groups are stateful. Everything you allow inbound will go outbound as well. 
- Default security group allows all traffic from all instances in the default security group.


## 35. Upgrading EBS Volume Types (__exam__)
- You can not mount an EBS volume in one AZ on a EC2 instance in another AZ
- You can change volume properties without downtime
- When terminating a EC2 instance only the root device volume will be terminated automatically

#### Snapshots
###### What
- Point in time copies of volumes
- Stored on S3
- Incremental

###### Actions
- To take a snapshot for an EBS volume which will serve as root device, you should stop the origin instance
- Encryption status stays the same when taking a snapshot from a volume or when creating a volume of a snapshot
- Moving a EBS volumes from one AZ to another
    - Take a snapshot
    - Creating a volume based on this snapshot
- Moving a EBS volumes from one region to another
    - Take a snapshot or make an image
    - Copy the snapshot to the other region
    - Change region
    - Creating a volume based on this snapshot in the other region
- Creating an AMI
    - Take a snapshot
    - Create image


## 36. Creating a Windows EC2 Instance & RAID Group
- When you're not getting the disk IO you need you can use RAID
- Create multiple EBS volumes and put them together as a single volume in RAID. Typically RAID 0 (striped) or RAID 10 (striped and mirrored)
- Configuring the RAID volume can not be done in AWS, but should instead be done in e.g. windows server, ...
- Taking a snapshot excludes data held in the cache by applications and the OS. Using multiple volumes in RAID, this can be a problem due to interdependencies of the array.
- Take a application consistent snapshot
    - Stop the application from writing to disk and flush all the caches to disk by
        - Freeze the file system
        - or unmount the RAID array
        - or stop the associated EC2 instance
    - Take a snapshot   


## 37. Encrypted Root Device Volume & Snapshots
- The default AMIs root device volumes are not encrypted and hence can not be encrypted during instance creation
- They are not encrypted because the encryption key is stored in your AWS account
- Encrypting an EC2 instance root device volume can be done:
    1. Stop the instance
    2. Take snapshot of the root device volume
    3. Copy the snapshot (to a different region) and check the box for encryption
    4. Create an AMI from the encrypted snapshot
    5. Launch an instance based on the AMI


## 38. AMI's - EBS Root Volumes vs Instance Store
#### EBS Root Volume
- Instance can be stopped. You will not lose your data when the instance is stopped.
- Rebootable
- By default deleted on termination, option to keep EBS root device volume
- Root device is an EBS volume created from an EBS snapshot
- Fast provisioning

#### Instance Store Root Volume (aka ephemeral storage)
- Instance can not be stopped. If the underlying host fails, you will lose your data.
- Rebootable
- By default deleted on termination, no option to keep instance store root device volume
- Root device is an instance store volume created from a template stored in S3
- Slower provisioning


## 39. Load Balancing Theory
- Balances the load of your application over different web servers
- Gateway timeout (504): when the load balancer has problems communicating with the instances
- `X-Forwarded-For` header: when the load balancer forwards request the origin IP address will be set to the load balancer IP address, the original IP address will be sent in this header.

#### Types
###### Application Load Balancers
- For http(s) traffic
- At OSI application layer (7)
- Advanced request routing

###### Network Load Balancers
- For TCP traffic
- At OSI transport layer (4)
- Extreme performance

###### Classic Load Balancers (__deprecated__)
- Elastic load balancers
- Can load balance http(s) and use some layer 7 features
- Can use strict layer 4


## 40. Load Balancers & Health Checks
#### Classic Load Balancer
1. Define Load Balancer
    - Name
    - Internal/external
    - Define forwarding rules: load balancer port -> instance port
2. Assign Security Group
3. Configure Security Settings
    - Configure https
4. Configure Health Check
    - Protocol, port and path to call on instances
    - Response timeout
    - Health thresholds
5. Add EC2 Instances
6. Review And Launch

#### Application Load Balancer
1. Configure Load Balancer
    - Name
    - Internal/external
    - Listeners: protocol
    - AZ to load balance over (at least 2)
2. Configure Security Settings
    - Configure https
3. Configure Security Groups
4. Configure Routing
    - Target group
    - Health check
    - Health thresholds
5. Register Targets
6. Review And Launch    


## 41. CloudWatch EC2
- Basic monitoring available in EC2 instance details, sampled every 5 minutes, every 1 minute in detailed mode

1. Give the dashboard a name
2. Add widgets
    - Text: free text with markdown formatting
    - Line: compare metrics over time, e.g. EC2 metrics (CPU, disk, network, status, custom metrics)
    - Stacked area: compare totals over time
    - Number: latest value for a metric
3. Alarms
    - Select metric
    - Select threshold over which period
    - Define action 
        - Notification
        - Autoscaling
        - EC2 action
4. Events
    - Respond to state changes in AWS resources
    - Create a rule to execute an action when a certain event is triggered by an AWS resource
5. Logs
    - Monitor EC2 instances at http level, e.g. response codes
    - Install agent on EC2 instance
    - Agent will pass monitoring information to CloudWatch logs
    - View logs in CloudWatch
6. Metrics
    - Show individual metric


## 42. The AWS Command Line & EC2
- AWS CLI is installed by default on AWS AMI's
- Login to EC2 instance using ssh: `ssh ec2-user@< ip > -i MyEC2KeyPair.pem`
- Configure AWS CLI: `aws configure`
- Type `aws ec2 help` for an overview of commands


## 43. Using IAM Roles with EC2
- Configuring AWS CLI on EC2 instance is not secure. If someone can login they can see your credentials on the file system.

1. AWS service role: allow AWS services to perform actions
    - Select the service that will use the role
2. Select the permissions that this role will grant to the service
3. Add name and description
4. When creating an EC2 instance a IAM role can be assigned
5. Now it is not necessary anymore to configure the AWS CLI on EC2 instances


## 44. S3 CLI & Regions
- When copying a file from a S3 bucket in a different region than the EC2 instance don't forget to add the `--region` flag.
- It is a good practice to always add the `--region` flag


## 45. Using Bootstrap Scripts
- During EC2 instance creation in instance configuration a bootstrap script can be added 
- A bootstrap script always starts with `!/bin/bash`
- Can be used to install patches: `yum update -y`
- Can be used to install tools: `yum install httpd -y`
- Can be used to copy S3 files to EC2 instance (S3 role needed)

## 46. EC2 Instance Metadata
- When logged in using ssh on an EC2 instance you can get metadata using `curl http://169.254.169.254/latest/meta-data/`
- E.g. get public IP address using metadata url, write it to a file, upload the file to s3, listen to the upload event, trigger a lambda to update public ip address in route53


## 47. Autoscaling 101
#### Autoscaling Group
1. Create launch configuration
    - Choose AMI
    - Choose instance type
    - Choose name, IAM role, bootstrap script
    - Add storage
    - Select security group
2. Configure autoscaling group details
    - Launch configuration
    - Number of start instances
    - Network
    - Subnets
    - Advanced: load balancer, health check
3. Configure scaling policies
    - Keep group at initial size
    - Scale between x and y instances when an alarm is triggered
4. Configure notifications
5. Configure tags
6. Review


## 48. EC2 Placement Groups (__exam__)
- Only certain instance types can be launched in a placement group

#### Clustered Placement Groups
- Group of instances within single AZ 
- For applications which need low network latency, high network throughput or both. E.g. cassandra
- Can't span multiple AZ

#### Spread Placement Groups
- Group of instances that are each placed on distinct underlying hardware
- For applications that have a small number of critical instances that should be kept separate
- Can span multiple AZ


## 49. EFS
- Elastic File System (NFS)
- File storage service for EC2 instances
- Elastic storage capacity
- Can be mounted to two EC2 instances at once
- Data stored across multiple AZ's in a region
- Block based storage
- Read after write consistency
- EFS and EC2 instances should be in same security group

#### Creation
1. Configure file system access
    - Select VPC
    - Create mount targets: EC2 instances connect to EFS using a mount target (IP address)
        - For each AZ
        - IP address
        - Security group
2. Tags, modes and encryption
3. Review and create

#### Mount
- Go to EFS in the AWS console
- Open EC2 mount instructions
- Copy mount command
- Execute command on EC2 instance


## 50. Lambda - Concepts
- Serverless
- Compute service where you upload code to lambda and configure a trigger
- Don't worry about OS, patching, scaling, ...
- Every time a lambda is triggered a new instance is started (scales out)
- AWS X-ray allows debugging lambdas
- Lambda can do things globally, spanning multiple regions

#### Use Cases
- Event-driven service where lambdas run your code in response to events. Events could be changes to data in S3, ...
- Run your code in response to HTTP requests using API gateway

#### Pricing
- Number of requests
    - First 1 million requests are free
    - $0,20 per 1 million requests thereafter
- Duration (max 5 minutes)
    - Calculated from when code starts executing until it returns
    - Price depends on amount of needed memory
    - $0,00001667 per GB-second


## 51. Build A Serverless Website
- Build a website using Route53, S3, API gateway and lambda
- S3 bucket name has to be the same as the DNS name of the site

#### Author From Scratch
1. Create function
    - Name
    - Environment: .NET, Go, Java, Node, Python
    - Role: allowing lambda permission to execute (simple microservice permissions)
2. Triggers
    - Select trigger type
    - Configure trigger
    - API Gateway, AWS IoT, CloudWatch events, CloudWatch logs, CodeCommit, Cognito sync trigger, DynamoDB, Kinesis, S3, SNS, SQS 
3. Function Code
    - Upload code to be executed
    - Environment variables
    - ...


## 52. Using Polly To Help You Pass Your Exam - A Serverless Approach - Part 1
- Lab


## 53. Using Polly To Help You Pass Your Exam - A Serverless Approach - Part 1
- Lab


## 54. Build An Alexa Skill
- Lab


## 55. Guru Of The Week
- A cloud guru competition


## 56. EC2 Summary
- Check this document