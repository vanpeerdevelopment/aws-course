# Section 4 - AWS Object Storage and CDN - S3, Glacier and CloudFront 

## 16. S3 101 (__exam__: read the [S3 FAQs](https://aws.amazon.com/s3/faqs/))
#### What
- Simple Storage Service
- Secure, durable, highly scalable object storage
- Web services interface
- Files from 0 bytes to 5TB which are stored in buckets

#### Bucket
- Folder in the cloud
- Used to group files
- Contains folders and files
- Needs a globally unique name
- Will be assigned a DNS name (_https://s3-<region>.amazonaws.com/<bucket-name>_)

#### Data Consistency Model
- Read after Write consistency for PUTS of new objects: uploaded file immediately available
- Eventual Consistency for overwrite PUTS and DELETES: updates and deletes available eventually. The reason is that S3 is spread across multiple AZ's. Can take some time to update or delete the file everywhere. 

#### Object
- Key: name of the file
- Value: content of the file in bytes
- Version id
- Metadata: creation date, tags, ...
- Subresources: ACL's, torrent

#### Basics
- Tiered storage
- Lifecycle management: if file is older than _x_ do _y_
- Versioning
- Encryption: client side encryption or server side encryption
- Secure data using ACL's and bucket policies

#### Storage Tiers/Classes
###### S3 Standard
- Stored redundantly across multiple devices (disks) in multiple facilities (AZ's)
- Sustain loss of 2 facilities concurrently

###### S3 IA
- Infrequently accessed, but requires rapid access when needed
- Cheaper than standard, but charged for retrieval
- Stored redundantly across multiple devices (disks) in multiple facilities (AZ's)

###### S3 One Zone IA
- Formerly called reduced redundancy storage (RRS)
- Infrequently accessed, but requires rapid access when needed
- Cheaper than IA, but charged for retrieval
- Stored in one AZ

###### Glacier
- Data archival
- Cheapest
- Stored redundantly across multiple devices (disks) in multiple facilities (AZ's)
- Sustain loss of 2 facilities concurrently
- Expedited: retrieval in few minutes 
- Standard: retrieval in 3 to 5 hours 
- Bulk: retrieval in 5 to 12 hours

#### Charges
- Storage: for each GB
- Request: for each request
- Storage management: for metadata
- Data transfer: for replication across regions
- Transfer acceleration: upload files to edge locations closer to the end user


## 17. Create an S3 Bucket - Lab
- S3 buckets are managed globally not per region
- Create bucket, upload files, manage bucket and files in the console


## 18. Version Control - Lab
- [Versioning](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html) can be enabled in the bucket properties
- Once versioning is enabled it can not be disabled anymore, only suspended
- When a new file with an existing name is uploaded it will create a new version of the file on S3
- Every single version will kept as a separate object
- Deleting a file will add a delete marker. It will not delete the file.
- Previous versions can be restored
- MFA delete


## 19. Cross Region Replication - Lab
- Cross region replication can be configured in the bucket management tab
- Requires versioning
- Add a rule 
    - Which source files 
    - Replicated to which destination bucket
    - With which storage class in the destination bucket
    - Select the role to give the necessary permissions
- Only files added or updated after setting replication will be replicated
- Deleting a file in the source bucket will replicate the deletion marker in the destination bucket
- Deleting individual versions or deletion markers in the source bucket will not be replicated in the destination bucket
- [Cross Region Replication Monitor](https://aws.amazon.com/answers/infrastructure-management/crr-monitor/)


## 20. Lifecycle Management & Glacier - Lab
- Use lifecycle rules to manage your objects
- Automate transition to tiered storage
- Expire your objects
- Lifecycle management can be configured in the bucket management tab
- Add a rule
    - Which source files 
    - Transitions
        - Current and/or previous versions
        - After how many days to which tier
    - Expiration
        - Current and/or previous versions
        - After how many days


## 21. CloudFront CDN Overview
#### What
- A content delivery network is a system of distributed servers that delivers web content to a user based his geographical location, the origin of the webpage and a content delivery server.
- Origin: origin of the webpage, can be an S3 bucket, EC2 instance, Elastic Load Balancer or Route53 in a certain region. Can also be an non AWS service.
- Edge location is a location where content will be cached
- Distribution: name given to the CDN which consists of a collection of edge locations
    - Web distribution: used for websites
    - RTMP: used for media streaming
- The files in your origin must be publicly readable.

#### How
- Users request a webpage using the distribution url
- The distribution url will resolve to the IP address of the closest edge location
- If the file is not cached on the edge location (during the TTL) it will be requested from the origin and cached 
- If the file is cached on the edge location it is immediately returned
- Cached objects can be cleared but this will be charged 


## 22. Create a CloudFront CDN - Lab
#### Origin settings
- Origin domain nam: S3 bucket, ...
- Origin path (optional): folder
- Restrict bucket access: only make files in the bucket accessible using the distribution url
- Origin access identity: a special cloud front user who will need access to the S3 bucket

#### Default cache behaviour settings
- Path pattern: regex to define from which origin to get the content
- Access to files on CloudFront can be restricted using a signed url. Extra configuration is needed for this.

#### Distribution settings
- Price: which edge locations will be used
- Alternate domain names: more human readable domain name to use instead of the generated CloudFront url
- SSL certificate: when using an alternate domain name it is required to upload the SSL certificate 


## 23. S3 - Security and Encryption
#### Security
- New buckets are private by default
- Bucket policies are bucket-wide
- Access control lists can drill down to individual files
- Access logs can be enabled, can be stored in another bucket

#### Encryption
- In transit
    - SSL/TLS
- At rest
    - Server side encryption
        - SSE-S3 (S3 managed keys): each file is encrypted (AES-256) with a unique key, AWS will manage the keys for you 
        - SSE-KMS (AWS key management service, managed keys): similar to SSE-S3 with separate permissions for your envelope key protecting the data encryption key and provides audit trail when keys were used, option to manage keys yourself
        - SSE-C (server side encryption with customer provided keys): manage the encryption keys yourself
    - Client side encryption
    
    
## 24. Storage Gateway (__exam__)
- Connects on-premise software appliance with AWS storage infrastructure
- Virtual machine (VMware ESXi or Microsoft Hyper-V) which is installed inside your data center which will asynchronously replicate data to AWS

#### Types
###### File Gateway
- For flat files (object storage)
- NFS mount point on your application server to storage gateway, data is not stored on-premise
- Ownership, permissions, timestamps are stored in the S3 metadata
- Bucket policies such as versioning, lifecycle management, .... can be applied

###### Volumes Gateway
- Presents your applications virtual hard disk volumes using iSCSI
- Block based storage (can install OS, DB, ... on it)
- Asynchronous (incremental) backup of snapshots stored in the cloud as Amazon EBS (Elastic Block Store) snapshots
- Stored volumes: store entire data set locally and asynchronously backup to AWS
- Cached volumes: S3 is used as primary data storage, while caching frequently accessed data locally in your storage gateway

###### Tape Gateway
- Backup and archiving
- Use your existing tape-based backup application to call the VTL interface of the tape gateway to create virtual tapes and send them to S3    


## 25. Snowball
- Import/export large amounts of data into the AWS cloud using portable storage devices
- Petabyte-scale data transport solution bypassing the internet
- Avoid network costs for upload or download

#### Snowball
- 80TB
- Storage
- Security: tamper-resistant enclosure, 256-bit encryption, full chain-of-custody

#### Snowball Edge
- 100TB
- Storage and compute capabilities
- Small AWS data center you can use on-premise
- E.g. use on airplanes

#### Snowmobile 
- 100PB
- Container


## 26. Snowball - Lab
#### Request a snowball
- Choose plan
- Shipping details
- Setup S3 bucket
- Choose encryption
- Set notifications using SNS

#### Hardware install
- Connect it to power and local network

#### Client
- Install the snowball client to be able to connect to the snowball
- Get access credentials in the AWS console
- Connect: `./snowball start -i <ip> -m <path to manifast> -u <access code>`
- Copy files: `./snowball cp <file> s3://<bucket-name>`
- Disconnect: `./snowball stop`


## 27. S3 Transfer Acceleration
- Utilise the CloudFront edge network to accelerate uploads to S3. Use a distinct url to upload to an edge location which will then transfer the file to S3.
- Url: `https://<bucket-name>.s3-accelerate.amazonaws.com`
- Can be configured in the bucket properties


## 28. Creating A Static Website Using S3
- If you want to use your own domain name it has to be exactly the same as the bucket name
- Serverless
- Configure static website hosting in the bucket properties
- `http://<bucket-name>.s3-website.<region>.amazonaws.com`
- Configure index and error page
- Upload the files and make them publicly accessible


## 29. S3 Summary
- Check this document