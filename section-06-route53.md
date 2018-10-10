# Section 6 - Route53 

## 57. DNS 101
- Domain Name Service
- Convert domain name into IP address
- IPv4: 32 bit address
- Ipv6: 128 bit address

#### TLD
- Top Level Domain name (`.be`): last word in a domain name
- Second Level Domain (`co.uk`, optional): second word in a domain name
- Managed by IANA (Internet Assigned Numbers Authority) in a [root zone database](http://www.iana.org/domains/root/db)

#### Registration
- Registrar is an authority which can assign domain names directly under one or more TLDs
- Each domain is registered in the WhoIs database
- Route53: analogy with Route66, 53 is the port on which DNS operates

#### SOA
- Start of Authority Record
- Record containing administrative information
- Name of the server supplying the data for the zone
- Administrator
- Version
- Default number of seconds for TT on resource records

#### NS
- Name Server Records
- Use by TLD servers to direct traffic to Content DNS servers which contains SOA record
- `helloroute53gurus.com. 172800 IN NS ns.awsdns.com`

#### MX
- Mail exchange record
- Points to mail server used for the domain

#### A
- Address record
- Mapping from domain name to IP address

#### CNAME
- Canonical name record
- Map one DNS name to another DNS name
- Can not be used to map naked domain names/zone apex records (e.g. example.com)

#### ALIAS
- Unique to Route53
- Map domain name to ELB, CloudFront distributions, S3 websites
- Works exactly like a CNAME
- Can be used to map naked domain names/zone apex records (e.g. example.com)


## 58. Register A Domain Name - Lab
- Route53 is a global service
- Register domain name in domain tab
    - Choose domain name and check availability
    - Enter registrant contact information
    - Verify and purchase
    - Wait for domain registration to complete
- Manage zone in hosted zones tab
    - NS records prefilled (spread across multiple TLDs)
    - SOA record prefilled
    

## 59. Different Routing Types on AWS
- Routing policy can be selected when adding a record set


## 60. Simple Routing Policy
- Register one record set with multiple IP addresses
- Route53 will return a random IP address
- No health checks possible


## 61. Weighted Routing Policy
- Register multiple record sets for same domain name with weighted routing policy
- Split traffic based on the different weights assigned


## 62. Latency Routing Policy
- Route traffic based on the lowest network latency for your end user
- Create a latency record set for each region
- Route53 selects record set of the region that gives the lowest latency


## 63. Failover Routing Policy
- Used for active/passive setup
- Route53 will monitor health of site using health check, can be configured in separate tab in Route53
- Create a failover record set both primary and secondary
- Route53 will direct traffic to primary unless it is not healthy


## 64. Geolocation Routing Policy
- Direct traffic based on geographic location of end user
- E.g. direct all traffic from EU users to instances in Europe
- Create a geolocation record set for each end user location
- Create a default location record set to allow people outside of configure locations to reach domain name  


## 65. Multivalue Routing
- Route traffic randomly to resources (with optional health check)
- Create a multivalue record set for each resource
- Route53 will resolve to a list of up to 8 random healthy IP addresses


## 66. DNS Exam Tips
- Check this document