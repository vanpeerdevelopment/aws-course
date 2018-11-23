# Section 11 - The Well Architected Framework 

## [The Well Architected Framework](https://aws.amazon.com/architecture/well-architected/) (paper)
#### Introduction
- Best practices and strategies to use when designing and operating a cloud solution 

##### On Architecture
- Often: enterprise architecture team setting out best practices and technologies to use
- At AWS: each team decides based on customer needs, company practices are followed and checked by mechanisms doing automated checks; principal engineers can check architecture 

##### General Design Principles
- Stop guessing your capacity needs
- Test systems at production scale
- Automate to make architectural experimentation easier
- Allow for evolutionary architectures
- Drive architectures using data
- Improve through game days


#### The Five Pillars Of The Well Architected Framework
##### Operational Excellence
- Definition: run and monitor systems to deliver business value and to continually improve supporting processes and procedures

###### Design Principals
- Perform operations as code
- Annotate documentation
- Make frequent, small, reversible changes
- Refine operations procedures frequently
- Anticipate failure
- Learn from all operational failures

###### Best Practices
- Prepare
    - Mechanisms to monitor and gain insight into application, platform, and infrastructure components, as well as customer experience
    - Mechanisms to validate that workloads, or changes, are ready to be moved into production and supported by operations
- Operate
    - Define expected outcomes, determine how success will be measured, and identify the workload and operations metrics that will be used
    - Use established runbooks for well-understood events, and use playbooks to aid in the resolution of other events
    - Communicate the operational status of workloads through dashboards and notifications that are tailored to the target audience
    - Determine the root cause of unplanned events and unexpected impacts from planned events
- Evolve
    - Include feedback loops within your procedures to rapidly identify areas for improvement and capture learnings from the execution of operations
    - Share lessons learned across teams
    
##### Security
- Definition: protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies

###### Design Principals
- Implement a strong identity foundation
- Enable traceability
- Apply security at all layers
- Automate security best practices
- Protect data in transit and at rest
- Keep people away from data
- Prepare for security events

###### Best Practices
- Identity and Access Management
    - Define principals, build out policies aligned with these principals, and implement strong credential management
    - Apply granular policies
- Detective Controls
    - Reactive factors that can help your organization identify and understand the scope of anomalous activity
- Infrastructure Protection
    - Enforcing boundary protection, monitoring points of ingress and egress, and comprehensive logging, monitoring, and alerting are all essential to an effective information security plan
- Data Protection
    - Encrypting data at rest and in transit
- Incident Response
    -  Putting in place the tools and access ahead of a security incident, then routinely practicing incident response through game days, will help you ensure that your architecture can accommodate timely investigation and recovery

##### Reliability
- Definition: recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues

###### Design Principals
- Test recovery procedures
- Automatically recover from failure
- Scale horizontally to increase aggregate system availability
- Stop guessing capacity
- Manage change in automation

###### Best Practices
- Foundations
    - AWS sets service limits to protect you from accidentally over-provisioning resources,  monitor and change these limits to meet your business needs
- Change Management
    - Monitor the behavior of a system and automate the response to KPIs
- Failure Management
    - Regularly back up your data and test your backup files
    - Test your system-recovery processes so that you are confident that you can recover all your data and continue to serve your customers

##### Performance Efficiency
- Definition: use computing resources efficiently to meet system requirements, and to maintain that efficiency as demand changes and technologies evolve

###### Design Principals
- Democratize advanced technologies
- Go global in minutes
- Use serverless architectures
- Experiment more often
- Mechanical sympathy

###### Best Practices
- Selection
    - Select the patterns and implementation for your architecture use a data- driven approach for the most optimal solution
    - Compute: may use different compute solutions for various components: instances, containers, functions
    - Storage: use multiple storage solutions
    - Database: use different database solutions for various subsystems
    - Network: place resources close to where they will be used
- Review
    - Understanding where your architecture is performance-constrained will allow you to look out for releases that could alleviate that constraint
- Monitoring
    - Metrics should be used to raise alarms when thresholds are breached
- Tradeoffs
    - Depending on your situation you could trade consistency, durability, and space versus time or latency to deliver higher performance

##### Cost Optimization
- Definition: run systems to deliver business value at the lowest price point

###### Design Principals
- Adopt a consumption model
- Measure overall efficiency
- Stop spending money on data center operations
- Analyze and attribute expenditure
- Use managed and application level services to reduce cost of ownership

###### Best Practices
- Cost-Effective Resources
    - Use the most cost-effective resources
    - Use managed services
- Matching supply and demand
    - Automatically provision resources to match demand
- Expenditure Awareness
    - Use cost allocation tags to categorize and track your AWS usage and costs
    - Identify orphaned resources or projects
- Optimizing Over Time
    - Review your existing architectural decisions to ensure they continue to be the most cost-effective

#### The Review Process
- Light-weight process (hours not days) that is a conversation and not an audit
- Outcome of the review is a set of actions that should improve the experience of a customer using the workload
- AWS Well Architected is aligned to the way that AWS reviews systems and services

#### Conclusion
- The Framework provides a set of questions that allows you to review an existing or proposed architecture 
- It also provides a set of AWS best practices for each pillar


## 99. Introduction To This Section