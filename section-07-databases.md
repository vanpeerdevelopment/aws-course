# Section 7 - Databases 

## 67. Databases 101
#### Relational Database
- Tables with rows existing of fields
- Types
    - Microsoft SQL server
    - Oracle
    - MySQL Server (open source)
    - PostgreSQL (open source)
    - Aurora (amazon)
        - MySQL
        - PostgreSQL
    - MariaDB
    
#### Non Relational Database
- Collections with documents existing of key-value pairs
- NoSQL
- Not all documents need to have same set of key-value pairs
- Documents often in JSON format
- DynamoDB

#### Data Warehousing
- For business intelligence
- Pull in very large and complex data sets
- Redshift

#### Elasticache
- In-memory cache 
- Improves performance by retrieving information from memory instead of disk
- Types
    - Memcached
    - Redis

#### OLTP vs OLAP
###### OLTP
- Online transaction processing
- Used to query rows of data

###### OLAP
- Online analytics processing
- Used to query complex analytics queries for which much data is needed
- Don't want to do this on production databases, but instead use a data warehouse


## 68. Launching an RDS Instance - Lab
#### Launching an instance
- Select database engine
- Choose use case: 
    - Production aurora
    - Production MySQL: multi AZ, Provisioned IOPS storage
    - Dev/Test Mysql: not for production or free tier
- Specify DB details
    - Instance specifications
        - License
        - Version
        - Instance type
        - Multi AZ
        - Storage type
        - Storage size
    - Instance settings
        - Identifier
        - Master username
        - Master password
- Advanced settings
    - Network and security
        - VPC
        - Subnet group
        - Public
        - AZ
        - Security group
    - Database options
        - Database name
        - Port
    - Encryption (only possible during creation)
    - Backup
    - Monitoring
    - Logging
    - Maintenance
    
#### Security group
- Make sure to allow you web security group as inbound traffic to the RDS security group 


## 69. RDS - Backups, Multi AZ & Read Replicas
#### Backups
###### Automated backups
- Full daily snapshot and store transactions logs
- Restore snapshot and apply transaction logs
- Recover to point in time down to a second, within retention period (1 - 35 days)
- On by default during defined backup window
- Stored in S3 (free storage equal to size of db)
- Deleted together with db

###### Snapshots
- Manual
- Saved even after db deletion
- During copy encryption can be enabled, can be copied to different regions
- Can be used to scale up. Take a snapshot, create a new RDS db with other instance type based on snapshot

###### Restore
- New RDS instance with new DNS name
   
    
#### Multi AZ
- Synchronous replication to different AZ on write operations
- Automatic failover to different AZ by change in DNS record
- For disaster recovery


#### Read Replicas
- Asynchronous replication from primary instance to read replicas on write operations
- Read operations can be done on all instances by using its unique DNS name
- Read replica of read replica are possible, watch out for latency
- Read replicas can have multi AZ
- Read replicas can be in different region
- Up to 5 read replicas per db 
- For performance improvement


## 70. DynamoDB
#### Facts
- NoSQL
- Very scaleable
- Supports document and key-value data
- SSD
- Spread across 3 facilities
- Read consistency
    - Eventual consistent reads: all copies consistent usually within 1 second, best performance
    - Strongly consistent reads: all copies consistent when write returns

#### Pricing
- Provisioned throughput capacity
    - Write throughput capacity: $0,0065 per hour for every 10 units
    - Read throughput capacity: $0,0065 per hour for every 50 units
    - 1 capacity unit can handle 1 read/write per second
- Storage cost: $0,25 per GB per month
- On-demand, reserved (1 - 3y contract)

#### Create DynamoDB Table
- Table name
- Primary key and type
- Sort key (optional)
- Secondary index (optional)
- Read/write capacity units

#### Scaling
- Scaling up can be done at runtime by increasing read and write capacities


## 71. RedShift
- Data warehouse
- OLAP
- Single node or multinode (leader node and compute nodes) in one AZ
- Columnar data storage
    - Data stored by column
    - Ideal for analytics
    - Less IO required to read columns of different records because not entire records need to be read
    - Process by executing operations on its columns: filter, sum, avg, ...
- Advanced compression
    - Because similar data is stored sequentially
- MPP
    - Massively Parallel Processing
    - Data and query load distributed across all nodes
- Pricing: compute node hours + backup + data transfer inside VPC
- Security: SSL in transit + AES at rest


## 72. ElastiCache
- In-memory cache
- Improve latency
    - For read-heavy workloads: cache date to avoid IO
    - For compute-intensive workloads: cache results of calculations

#### Memcached
- Memory object caching
- No multi AZ

#### Redis
- Key-value store
- Sets, lists, pub sub, ...
- Master/slave replication
- Multi AZ for redundancy


## 73. Aurora
- Relational DB engine created by amazon
- MySQL, PostgreSQL compatible
- Performance, availability of commercial database

#### Advantages
- Autoscaling from 10GB in 10GB increments
- Scaling up to 32 vCPUs during maintenance window
- Scaling up to 244GB RAM during maintenance window
- 2 copies of data in each AZ, minimum 3 AZ
- Can loose 2 copies without affecting write availability, up to 3 copies without affecting read availability

#### Replicas
- Aurora replicas (up to 15): separate aurora database, automatic failover when primary fails
- MySQL read replicas (up to 5): no automatic failover to read replica when primary fails


## 74. Databases Summary
- Check this document