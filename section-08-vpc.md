# Section 8 - VPC 

## 75. Introduction & Overview
- Virtual Private Cloud: virtual data centre in the cloud
- VPC lets you provision a logically isolated section of AWS cloud where you can launch AWS resources in a virtual network you define
- Customize network configuration. E.g. create public-facing subnet (in AZ 1) for webservers, private-facing subnet (in AZ 2) for databases, ...
- Security using security groups and network access control lists

#### What
- Create VPC in a region
- Subnet: always in one AZ within the region, choose its network address (subnet mask)
- Instances: launch in a subnet with a certain security group stateful (configure only inbound) and configure custom IP address
- Network access control lists: stateless (configure both inbound and outbound), block specific IP addresses, ...
- Route tables: configure which subnet can access which subnet
- Internet gateway: one internet gateway per VPC providing public access, automatically spread over all AZs
- Virtual private gateway: vpn gateway

#### Default VPC
- Automatically created by Amazon in each region
- All subnets public
- Each EC2 instance has both a public and private IP address

#### VPC Peering
- Connect one VPC to another via direct network route using private IP addresses
- Instances a virtually in same private network
- Not transitive


## 76. Build Your Own Custom VPC - Part 1
#### Create VPC
- Name
- IPv4 CIDR block: IP range for the VPC, e.g. 10.0.0.0/16
- IPv6: yes/no
- Tenancy: default or dedicated hardware
- Automatically created
    - Route table
    - Network access control list
    - Security group

#### Create Subnet
- Name, e.g. CIDR IP range - AZ
- VPC
- AZ
- IPv4 CIDR block, e.g. 10.0.1.0/24
    - 255 IP addresses
    - 5 reserved
        - 10.0.1.0: network address
        - 10.0.1.1: router address
        - 10.0.1.2: reserved for dns
        - 10.0.1.3: future use
        - 10.0.1.255: broadcast address
- IPv6 CIDR
- Enable auto-assign public IP address for public subnets: auto-assign public IP address for instances launched in subnet


#### Create Internet Gateway
- Name
- Attach to VPC

#### Configure Route Tables
- Routes
    - 10.0.0.0/16 - local: allows subnets to talk to each other
    - 0.0.0.0/0 - < internet gateway >: allow subnet to talk to internet
- Subnet Associations
    - Subnet automatically associated to main route table, so do not give it internet access
    - Create new route table for internet access out and attach public subnets
    

## 77. Build Your Own Custom VPC - Part 2
- Communication between public and private subnet
    - Configured in route table by default
    - Create security group allowing inbound traffic from public subnet and assign the security group to private instance
    

## 78. Network Address Translation (NAT)
- Allow outbound internet access from instances on the private subnet, e.g. to update packages
- [NAT comparison](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html)

#### NAT Instances (deprecated)
- Start EC2 instance based on an Amazon NAT AMI
- Start in the public subnet of your VPC
- Attach security group with SSH, HTTP and HTTPS configured so it allows inbound HTTP(S) traffic
- Disable source/destination check
- Add a route in the default route table to the NAT instance (0.0.0.0/0 - < NAT instance >), used by instances in the private subnet to have outbound internet access
- Managed by yourself

#### NAT Gateway
- IPv4
- Select public subnet
- Select/create elastic IP
- Not in a security group
- Add a route in the default route table to the NAT gateway (0.0.0.0/0 - < NAT gateway >), used by instances in the private subnet to have outbound internet access
- Add NAT gateway in each (public) AZ for high availability
- Managed by Amazon

#### Egress Only Internet Gateways
- IPv6


## 79. Network Access Control Lists vs Security Groups
#### Network Access Control Lists
- Default NACL allows all inbound and outbound traffic
- Subnet can only be associated with one NACL
- Stateless: configure both inbound and outbound rules
- Rules are evaluated in numerical order, first match is used
- Checked before security groups

###### Create
- Name
- VPC
- Denies all inbound and outbound traffic 
- Add inbound rules
- Add outbound rules (add ephemeral ports when using http(s))
- Associate subnet


## 80. Custom VPCs and ELBs
- Create ELB in custom VPC
- Select AZs, one public subnet per AZ


## 81. VPC Flow Logs
- Capture info about IP traffic to and from network interfaces within VPC
- Stored in CloudWatch logs
- Created at 3 levels: VPC, subnet, network interface level
    - Filter: all, accept, reject
    - Role: Flow Logs permissions
    - Destination log group
- Can not tag
- Not configurable after creation
- Not all traffic logged:
    - Traffic with Amazon DNS server
    - Traffic for windows instance license activation
    - Traffic with 169.254.169.254
    - DHCP
    - Traffic to default VPC router
    
    
## 82. NAT vs Bastion
#### NAT
- NAT instance/gateway in public subnet
- Can not SSH into NAT instance/gateway to connect to private instance
- Instance in private subnet can access internet over NAT instance/gateway

#### Bastion
- Bastion (jump box) in public subnet
- SSH into bastion and then connect to private instances over SSH
- For administration use
- One single entry point to secure
- Make highly available by having autoscaling group to make sure always 1 bastion available, combined with Route53 health checks


## 83. VPC Endpoints
- Private instances with an S3 role can connect to S3 over the NAT gateway in the public subnet
- VPC endpoint allows you to connect your VPC to another AWS service without having to use the NAT gateway

#### Create
- Select AWS service
    - Interface: elastic network interface attached to EC2 instance
    - Gateway: target for route in route table, shared by multiple EC2 instances (like NAT gateway)
- Select VPC
- Add to main route table so it can be used by private instances
- Policy: permissions


## 84. VPC Clean Up 
- Delete EC2 instances
- Delete NAT gateway
- Delete endpoints
- Detach and delete internet gateway
- Delete VPC, will also delete subnets, security groups, NACLs, route tables


## 85. Summary
- Check this document