# Section 2 - AWS - 10.000 Feet Overview

## 4. The History Of AWS So Far (~~exam~~)
- 2004: Simple Queue Service (SQS)
- 2006: AWS
- 2010: amazon.com on AWS
- 2012: first [re:Invent conference](https://reinvent.awsevents.com/)
- 2013: certifications
- 2017: AR and VR


## 5. AWS - 10.000 Foot Overview Part 1
#### AWS Global Infrastructure 
- Region
    - 22
    - Geographical area
    - Consists of two or more AZ's 
- Availability zone (AZ)
    - 61
    - Data center
    - Can be in the same region
    - Far enough apart so they do not all go down when one of them has an issue
- Edge location
    - 96
    - Endpoints used for caching content (CDN) 


## 6. AWS - 10.000 Foot Overview Part 2
#### Compute
###### EC2
- Elastic Compute Cloud
- Virtual machine

###### EC2 Container Services
- Run docker containers

###### Elastic Beanstalk
- Just upload your code
- Automatically configures autoscaling groups, load balancer, EC2 instances

###### Lambda
- Serverless
- Upload your code and control when it executes

###### Lightsail
- Virtual Private Server (VPS)
- Provisions a server and gives you the IP address to manage it

###### Batch (~~exam~~)
- Batch computing


#### Storage
###### S3
- Simple Storage Service
- Upload your files in buckets

###### EFS
- Elastic File System
- Network attached storage
- Store files and mount on multiple virtual machines

###### Glacier
- Archiving

###### Snowball
- Upload large amounts of data
- Write on disk, ... and send it to amazon

###### Storage Gateway
- Virtual machines which can be installed in your data center or office
- Replicates data on it to amazon


#### Databases
###### RDS
- Relational Database Service

###### DynamoDB
- Non-relation Database Service

###### Elasticache
- Caching commonly queried things from your database

###### Red Shift
- Datawarehousing and business intelligence
- Instead of running heavy queries or analysis on your production databases


#### Migration
###### AWS Migration Hub
- Tracking service for your applications while they are migrated to AWS
- Integrates with other migration services

###### Application Discovery Service
- Automated set of tools to detect which applications and dependencies you have

###### Database Migration Service
- To migrate on-premise database to AWS

###### Server Migration Service
- To migrate your virtual/physical servers into AWS

###### Snowball
- Migrate large amounts of data  


#### Networking & Content Delivery
###### VPC (__exam__)
- Virtual Private Cloud
- Virtual data center where you configure firewall, AZ, networking, ...

###### CloudFront
- Content Delivery Network (CDN)

###### Route53
- DNS

###### API Gateway
- Create own API's for your other services to talk to

###### Direct Connect (__exam__)
- Dedicated line from your office directly in amazon


#### Developer Tools (~~exam~~)
###### CodeStar
- Project management for code: CI

###### CodeCommit
- Place to store your code
- VCS: git, ...

###### CodeBuild
- Build and test your code

###### CodeDeploy
- Deploy your code to EC2, on-premise instances, lambda

###### CodePipeline
- CD service
- Automate and visualize the steps to release your software

###### X-Ray
- Debugger for serverless application

###### Cloud9
- IDE environment in the browser


## 7. AWS - 10.000 Foot Overview Part 3
#### Management Tools
###### CloudWatch
- Monitoring service

###### CloudFormation (__exam__)
- Scripting infrastructure

###### CloudTrail (__exam__)
- Logs changes to the AWS environment
- Stores records by default for one week

###### Config (__exam__)
- Monitors configuration of entire AWS environment
- Point in time snapshots, can move time forward/backward and see the AWS environment for the point in time

###### OpsWorks
- Similar to ElasticBeanstalk, but more robust
- Uses chef and puppet to automate the configuration of the environment

###### Service Catalog (~~exam~~)
- Manage catalog of IT services

###### Systems Manager (~~exam~~)
- Interface for mananing AWS resources, e.g. rollout security patches across EC2 instances
- Group AWS resources by departments, applications

###### Trusted Advisor (__exam__)
- Gives advice about AWS environment, e.g. unused resources, security issues,...

###### Managed Services (~~exam~~)
- Managed services for your AWS cloud, if you don't want to manage your EC2 instances and so on


#### Media Services
###### Elastic Transcoder (~~exam~~)
- Transcodes video for multiple devices

###### MediaConvert (~~exam~~)
- Transcoding service for broadcasting

###### MediaLive (~~exam~~)
- Live video processing service 

###### MediaPackage (~~exam~~)
- Prepare and protect videos for delivery over the internet

###### MediaStore (~~exam~~)
- Optimized to store media on AWS

###### MediaTailor (~~exam~~)
- Targeted advertising in video streams 


#### Machine Learning
###### SageMaker (~~exam~~)
- Deep learning
- Neural networks

###### Comprehend (~~exam~~)
- Sentiment analysis, whether people are saying good or bad things about your product

###### DeepLens (~~exam~~)
- Wearable camera which can figure out what it's looking at

###### Lex (~~exam~~)
- Artificial intelligent way of chatting whit your customers
- Powers Alexa

###### Machine Learning (~~exam~~) 
- Recommendations, ...

###### Polly (~~exam~~)
- Turns text into speach
- Choose different languages, regions

###### Reckognition
- For videos or images
- Tells you what is in the video or image

###### Translate
- Translate text in one language to another

###### Transcribe
- Speech recognition
- Turn speech into text


#### Analytics
###### Athena (~~exam~~)
- Run SQL queries against documents in S3 in e.g. csv's, ...

###### EMR (__exam__)
- Elastic Map Reduce
- Processing large amounts of data
- Big data

###### CloudSearch
- Search service

###### ElasticSearch Service
- Search service

###### Kinesis (__exam__)
- Big data
- Ingesting large amounts of data into AWS, e.g. tweets 

###### Kinesis Video Streams
- Ingesting video streams

###### QuickSight
- Business Intelligence tool

###### Data Pipeline (__exam__)
- Move data between different AWS services

###### Glue
- Extract Transform Load (ETL) tool
- When migrating large amounts of data it often is not in the format you want it
- First extract it, transform it and then load it into its new destination


## 8. AWS - 10.000 Foot Overview Part 4
#### Security & Identity & Compliance
###### IAM (__exam__)
- Identity Access Management

###### Cognito
- Way of doing device authentication using Facebook, Gmail, LinkedIn...
- Give user on a mobile device temporary access to use a certain AWS service

###### GuardDuty (~~exam~~)
- Monitor for malicious activity on AWS account

###### Inspector (__exam__)
- Agent which can be installed on virtual machines or EC2 instances to scan for vulnerabilities

###### Macie
- Scan S3 bucket for Personal identifiable Information 
- E.g. names, credit card numbers, ...

###### Certificate Manager (__exam__)
- Free SSL certificates when registering domain names using Route53

###### CloudHSM (__exam__)
- Cloud Hardware Security Module
- Dedicated hardware to store private keys

###### Directory Service (__exam__)
- Integration Microsoft Active Directory services with AWS services

###### WAF (__exam__)
- Web Application Firewall
- Layer 7 firewall
- Stops things like CSS, SQL injection

###### Shield (__exam__)
- By default for CloudFront, Load balancers, Route53
- DDOS mitigation

###### Artifact
- Portal to download compliance reports


#### Mobile Services
###### Mobile Hub (~~exam~~)
- Management console for mobile apps
- Connect mobile app to AWS backend using SDK

###### Pinpoint (~~exam~~)
- Use targeted push notifications when close to a certain place

###### AppSync (~~exam~~)
- Update data in web and mobile versions of an application
- Update data for offline usage 
 
###### Device Farm (~~exam~~)
- Testing apps on real life devices

###### Mobile Analytics (~~exam~~)
- Analytics service for mobile 


#### AR & VR
###### Sumerian (~~exam~~)
- For AR, VR, 3D application design
- Common set of tools to create these environments


#### Application Integration
###### Step Functions (~~exam~~)
- Way of managing lambda functions

###### Amazon MQ (~~exam~~)
- Message Queue

###### SNS (__exam__)
- Simple Notification Service
- Get alert when some condition is reached

###### SQS (__exam__)
- Simple Queue Service
- Decouple infrastructure by putting information on the SQS

###### SWF (__exam__)
- Simple Workflow Service
- Create workflows


#### Customer Engagement
###### Connect (~~exam~~)
- Contact center as a service
- Customer engagement

###### Simple Email Service (__exam__)
- SES
- Sending large amounts of emails


#### Business Productivity
###### Alexa For Business
- E.g. use it to order ink for the printer

###### Chime
- Video conferencing like Hangouts

###### Work Docs (__exam__)
- Storing work related documents like Dropbox

###### WorkMail 
- Email through amazon like Office365


#### Desktop & App Streaming
###### Workspaces (~~exam~~)
- VDI
- Run OS like windows, linux in amazon cloud

###### AppStream 2.0 (~~exam~~)
- Stream applications running on AWS to device like Citrix


#### Internet Of Things
###### iOT (~~exam~~)
- Devices sending back sensor data to AWS

###### iOT Device Management (~~exam~~)
- Management of all the devices sending back sensor data

###### FreeRTOS (~~exam~~)
- OS for microcontrollers

###### Greengrass (~~exam~~)
- Software for local compute messaging, data caching, sync and machine learning for connected devices


#### Game Development
###### GameLift
- Develop games


## 9. Don't Freak Out!
#### Solutions Architect Associate
Won't go into a lot of depth compared to Developer or SysOps associate.

Exam content: 
- AWS Global Infrastructure
- Compute
- Storage
- Databases
- Migration
- Networking & Content Delivery
- Management Tools
- Analytics
- Security & Identity & Compliance
- Application Integration
- Desktop & App Streaming


#### Developer Associate
Goes into much more detail, especially about DynamoDB. Easier than the Solutions Architect exam.
Watch the developer videos about S3, DynamoDB, application integration and analytics to pass the developer exam. 

Exam content: 
- AWS Global Infrastructure
- Compute
- Storage
- Databases
- Networking & Content Delivery
- Management Tools
- Analytics
- Security & Identity & Compliance
- Application Integration


##### SysOps Associate
Hardest of the three associate exams. A lot about CloudWatch and CloudTrail.

Exam content: 
- AWS Global Infrastructure
- Compute
- Storage
- Databases
- Networking & Content Delivery
- Management Tools
- Security & Identity & Compliance
- Application Integration


## 10. AWS This week
- AWS changes all the time. To keep up to date check out [AWS this week](https://acloud.guru/aws-this-week).


## 11. Sign Up To AWS Free Tier
- [Create an AWS free tier account](https://aws.amazon.com/free/)
- [Log in to the console](https://console.aws.amazon.com/) 