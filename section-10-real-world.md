# Section 10 - The Real World - Creating a fault tolerant Word Press Site 

## 94. Getting Setup
- Security group for web: http, ssh
- Security group for RDS: mysql
- RDS Multi-AZ cluster
- S3 bucket: WordPress code 
- S3 bucket: WordPress media assets
- CloudFront distribution for media assets
- Route53 domain name
- EC2 S3 role
- EC2 instance


## 95. Setting Up EC2
- Start WordPress
- Go to EC2 and configure WordPress
- Copy media uploads to S3 bucket
- Copy WordPress code to S3 bucket
- URL rewrite for media files to CloudFront
- ELB


## 96. Setting Up Our AMI's
- Create Crontab to sync WordPress code from S3 bucket to reader node EC2
- Create AMI from EC2 instance for reader nodes
- Change crontab to sync WordPress code from writer node EC2 to S3 bucket 
- Create crontab to sync WordPress media from writer node EC2 to S3 bucket 
- Auto Scaling Group for the reader node AMI linked to ELB


## 97. CloudFormation
- Template: visual tool for building environment
- Stack
    - Select template
    - Provide parameters
    - Create stack: will create all necessary aws resources

    
## 98. Want To Be A Real Solutions Architect? You Need To Know CloudFormation!
-  ...