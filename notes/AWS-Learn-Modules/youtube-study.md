# Youtube study

From Computer Networks Decoded, "AWS Security Specialty Certification Full Course"


## 2.1 Abuse Notice Strategies

#### Question Domain Main Points

1. Given an AWS abuse notice, evaluate the suspected compromised instance or exposed access keys

2. Verify that the Incident Response plan includes relevant AWS services

3. Evaluate the configuration of automated alerting and execute possible remediation of security-related incidents and emerging issues

#### Abuse Notice Strategies

Evaluate the suspected compromised instance

- Amazon GuardDuty

- VPC Flow log

- Isolate from network

- Launch replacement from AMI

#### Abuse Notice Strategies

Evaluate exposed access keys

- **Access Advisor reports** 
    To determine which service was using that key and at what time

- GuardDuty

- **CloudTrail Logs**

- Disable keys, create new keys

## Lesson 2: Incident Response

- 2.1 Abuse Notice Strategies
- 2.2 Incident
- 2.3 IR Preparation
- 2.4 IR Detection & Analysis
- 2.5 IR Containment Eradiction & Recovery
- 2.6 IR Post-incident Acitivity
- 2.7 Case Study: Compromised EC2
- 2.8 Question Breakdown

## 2.2 Incident Response Basics

- Everyone should know incident response, but an **incident response plan** as a general security concept is going to be reviewed

Incident response plan:

1. Preparation

2. Detection & Analysis

3. Containment, Eradication, & Recovery

4. Post-Incident Activity

**Continue of already collected notes**

## Ability to Store Relevant Event Information

First step in incident response plan.

Do this when strategic planning security in AWS

- CloudWatch Logs
    Great way to store files durabily with excellent ways for accessing them for analysis

- **AWS Config streams**
    Can create snapshots of the metadata configuration for your resources

- CloudWatch Events
    To keep track what is going on in your AWS account

- Access logs stored in S3
    Can put logs in S3 to addition of CloudWatch Events

- CloudTrail Logs
    Help correlate between behavior and actions that were taken to create that behavior

## 2.4 IR Detection & Analysis

Second step in incident response plan.

### Recognizing Signs of an Intrusion Attempt

- CloudWatch
    Clearly are going to be used for as monitoring across a lot of different areas and is a great way to recognize when your infrastructure is being attacked

- CloudTrail
    CloudTrail gives you the audit log of everything happening in your account by API perspective

- GuardDuty

- VPC Flow Logs

### Incident Analysis

- Visualize performance baseline
    - CW (CloudWatch) Dashboards
    - SSM insights Dashboard

- Understand normal behavior
    - GuardDuty Dashboard

- Implement log retention policy
    - CloudWatch Logs expiration
    - S3 Lifecycle policies
    - Glacier Vault lock policy

- Correlate events between logs and metrics
    - Amazon ElasticSearch & Kibana
    - CloudWatch Logs Insights

### Incident Notification

- SNS
    Simple notification service; Just a topic and subscriber service; Notification can be sent to loads of subscribers; Doesn't just use email notifications too

- SES
    Simple Email Service; If you only need email notifications

- Trusted Advisor
    You can sign up for this with a couple of different email addresses and when AWS recognizes a security event then it triggers the email(s)

### Use All Help Resources

- Open a support case
    If suspect that your account is being subject to a security event, **OPEN A SUPPORT TICKET RIGHT AWAY**

- Search the AWS forums

- Great justification for premium support!

## 2.5 IR Containment Eradication & Recovery

### Containment Strategy

- Security Group rules

- Revoke IAM sessions

- WAF ACL rules
    Ablility to create them on the fly

- IAM Policies

- Access Key rotation

- KMS CMK rotation
    For encrypting data at rest

### Evidence Gathering and Handling

- CloudTrail

- CloudWatch Logs
    Access logs, execution logs, or security logs

- VPC Flow Logs
    With full accounting of the network traffic and whether it is passed or denied.

- IAM Access Advisor
    Determine whether or not a user or a particular set of keys have been used for abnormal purposes.

### Identify the Attacking Entity

- DNS lookup
    Using services like Route 53; Not specific to AWS but is a standard strategy for associating hostnames and domain with IP addresses

- GuardDuty findings

### Eradiction of Potentially Compromised Resources

- EC2 Instnace termination

- Disable compromised keys

- Segregate compromised data for analysis

### Recovery Steps

- Know your RTO/RPO
    AKIA Recovery Time Objective / Recovery Point Objective

- Repaired resources

- Replaced resources

### Cleanup

- Remove temporary resources
    To remove resouces that were created as part of our mitigation process

- KMS key audit

- Full IAM audit
    Make sure permissions are set at appropriate levels

- Review further findings

## 2.6 Post-Incident Activity

### Lessons Learned

- Evidence retention
    How do we hold on to the forensic data gathered in such a way that doesn't compromise the account?
    - S3
    - Glacier
    - AMI
    - Snapshots of comprimised volumes

- Proposals for improvement

    - Least privilege
        - Access control
        - Network permissions
    - Monitoring
        - Better dashboards
        - Active response

## 2.7 Case Study: Compromised EC2

Put incident repsonse plan into action with a scenario 

Scenario: One of your EC2 instances has been compromised by an unknown actor

How can you:
    1. Identify compromised resources
    2. Identify bast radius
    3. Mitigate the event
    4. Recover Safely?

### Identify Compromised Resource

Option 1: AWS abuse notice via email
    When you get the abuse email there are going to be some questions:
    
    Who?
    What?
    When?
    Where?
    Why?

    We aren't going to have a lot of details
    - No context
    - Few data points = suboptimal

Option 2: Identify through existing instrumentation
    - CloudWatch
    - VPC Flow Logs
    - EC2 OS Firewall

    ^This is going to help us get data points on security being ^

    In by doing this "Data point correlation" with our viewed behavior with actions that are being taken.

    This allows us to make Informed Decisions as how to move ahead

#### Identify Blast Radius

Use AWS ecosystem to determine inventory and relationships

- Compromised instance
- AWS Config

#### Migrate the event

Contain the compromised resource

- EC2 and application security group
    In the Security Group inbound it can look like

    Security Group Inbound

    | Protocol  | Port |  Source    |
    |-------|-----|-------|
    | ICMP | ALL | VPC CIDR |
    | TCP | 22 | Bastion SG |
    | TCP | 8080 | ALB SG |

    Security Group Outbound

    | Protocol  | Port |  Source    |
    |-------|-----|-------|
    | none | none | none |

    Create Containment SG to place the EC2 instance
    For this security group the SG rules will look like

    Security Group Inbound

    | Protocol  | Port |  Source    |
    |-------|-----|-------|
    | none | none | none |

    Security Group Outbound

    | Protocol  | Port |  Source    |
    |-------|-----|-------|
    | none | none | none |

    Detach SG from EC2 instance
    
    Then attach SG to EC2 instance

    net effect: EC2 instance isolated from network

    Once we have assurance that the compromised instance can not do any further damage, now becomes a matter of recovering

    Leave isolated resources in place for analysis

    Assuming we have created our backups of AMI of EC2 instance that is preconfigured we can simply launch another resource to replace the capacity

    The resource will be placed in the correct Application Security Group

    #### Recover Safely

    Restore capacity while retaining forensic data
        - Persist all logs and data points in durable storage for post mortem analysis
    
    Post mortem and feedback loop
        - We have Forensic data out goal is to have insights so we can make recommendations

        In order to make this happen there needs to be some middle ground with the storage and Insights & Recommendations

        One way of doing this is by using RedShift queries, by using redshift spectrum, you can query S3 data as if it was apart of a postgre database

        You can query the data directly as well with Athena queries

        We could perform jobs that require code to be executed using AWS Glue

        QuickSight dashboard is more a graphical interface for us to explore this data like it being a database

    Turn recommendations into action

    - More restrictive security group rules

    - Better monitoring, actice responses

## Module 3: Logging and Monitoring

Contains two lessons:

- Lesson 3: Security Monitoring

- Lesson 4: Logging Solutions

### Objectives

- 3.1 Infrastructure Security Monitoring

- 3.2 Application Security Monitoring

- 3.3 Account Security Monitoring

- 3.4 Troublshooting Security Monitoring

- 3.5 Case Study: Broken Monitoring

- 3.6 Question Breakdown

### Question Domain Main Points

1. Design and implement security monitoring and alerting

2. Troubleshoot security monitoring and alerting

3. Design and implement a logging solution

4. Troubleshoot logging solutions

### Infrastructure Security Monitoring

#### GuardDuty

Cloudtrail logs go to Amazon GuardDuty

VPC Flow logs go into Amazon GuardDuty

DNS Logs go into Amazon GuardDuty

#### OS

OS (Operating System) Logs can be sent to S3 where you can use Athen Queries on it

OS logs can be sent to CloudWatch Logs Insights

OS logs can be sent to Splunk

#### Config

Config rules where you create a stream of configuration changes and you apply logic to that stream to determine if a change matches the rule

Security Group Changes are captured by Config rules

A new instance launch that doesn't met criteria is captured by Config rules

Changes or additions to Route Table Addition

#### Amazon Inspector

Focuses specifically on EC2 operating system vulnerability

The way this works is by installing an agent, determining a rule set that you would like to execute on the install then actually running the job

Can identify unencrypted network traffic

Unused TCP listeners, over a period of time tells about which of them isn't being used and sends to you as a finding

## 3.2 Application Security Monitoring

### Application Security Monitoring

#### CloudWatch Logs

Lambda Execution Logs gets pushed to CloudWatch Logs

EC2 Application logs gets pushed to CloudWatch Logs

ECS/EKS Container logs gets pushed to CloudWatch Logs

#### CloudTrail

Coginto User Authentication Logs goes into CloudTrail

Step Function logs go into CloudTrial

Deployments via CodeDeploy go into CloudTrail

#### S3

ALB Access Logs go into S3

Cloudfront Access Logs go into S3

Redshift audit logs go into S3

## 3.3 Account Security Monitoring

### Account Security Monitoring

#### CloudWatch Events

GuardDuty Findings go to CloudWatch Events bus in addition to sending the actual log for CloudWatch Logs

CloudTrail Events can generate events that are sent to CloudBus

AWS Organization Events allows you to connect multiple accounts and manage them like you would with a directory and is sent to CloudWatch Events

#### Config rules

CloudTrail enabled; if not then no compliant and generates a notification 

IAM User Key Rotation if not compliant, then config rules should pick up on that

Root User MFA Enabled; if not compliant, then Config rules should pick up on that

## 3.4 Troublshooting Security Monitoring

### Troubleshooting Scenarios

- Security event mis-labeled as performance event

- Vulnerable libraries identified on EC2 instances

### Troublshooting Tools

- Amazon Inspector findings
- CloudWatch alarms and metrics
- CloudTrail logs
- CloudWatch Logs Insights
- Unclear requirements for security monitoring
- Poorly implemented CloudWatch Alarms
- Gaps in OS update procedures and compliance checking

## 3.5 Case Study: Broken Monitoring

Scenario: Recent security event never generalized notifications

How can you:

1. Trace why notifications have not recieved

2. Implement missing monitoring

3. Ensure compliance going forward?

### Question Breakdown

An application running in **EC2** has a requirement for **independent, periodic security checks against the application code**.

These checks can send **notifications upon warning**, but for **critical alerts they must shut down the application on the instance**. 

How can your security team perform these checks **without injecting code** into the application, while meeting the notification and active response requirement?

C. Install CloudWatch Logs agent on the instance, streaming all application logs.
Create a CloudWatch Logs metric filter with alarms for notifications and a lambda function to stop the applications.

**Not the right answer** as this ^ does not act as an independent audit, relying on application logs

**The right answer is** 
B. Deploy a second application on the EC2 instance with the security audit code. Send security audit results to CloudWatch Events, and create a rule to send warning events to SNS, and critical events to SSM Run Command to stop the application.

# Lesson 4: Logging Solutions

4.1 Access Logs

4.2 Execution Logs

4.3 Security Logs

4.4 Log Processing

4.5 Case Study: Automated Log Management

4.6 Question Breakdown

## 4.1 Access Logs

If your IT infrastructure is taking web requests from any clients from any location chances are you're generating access logs.

When you generate access logs it becomes important to monitor them both from a performance standpoint, but also from a security there are a number of services in AWS that generate access logs.

it's in your best interest to know how those access logs can be stored and what permissions mechanisms are used to do so.

### Access Logs - API Gateway

API gateway came out when Lambda functions came out as they are to be used in tandum with each other.

API gateway pushes it's access logs to CloudWatch Logs (This is an optional feature) if it has an IAM role to do so. 

This IAM role will have to have an allow API Gateway to communicate with cloudwatch logs

Cloudfront can also be used to generate access logs and with this service it's a little bit more primitive with the access control and the location it 

### Access Logs - CloudFront

CloudFront can be used to generate access logs.

These access logs goes into S3 buckets if you have a Bucket ACL

### Access Logs - ELB

ELB or Elastic Load Balancer will put its access logs to S3 as well as long as you have a bucket policy

This is a best effort delivery!!!

### Access Logs - S3

You can transfer access logs from one bucket to another as long as there is a Bucket ACL.

This is best effort delivery!!!

You can enable a S3 bucket to act as a static website and when you do so it uses similar mechanisms to CloudFront to store its access logs.

Using another S3 bucket and a bucket ACL but much like the elastic load balancer and S3 bucket access log mechanism is a **best effort delievery**.

## 4.2 Execution Logs

Execution logs are another important way to monitor the security of requests that are passing through your application infrastructure.

There are a number of services within the AWS ecosystem that generate execution logs the first of these.

### Execution Logs - Lambda

Lambda pushes to CloudWatch Logs if it has a IAM role created.

For each Lambda function it's going to log every output whether it is info or debug or some kind of error condition it's going to be logged and sent directly to CloudWatch Logs.

#### Execution Logs - Custom EC2

If you have custom applications that are running on EC2 instances you can set up the CloudWatch agent or the CloudWatch Logs agent.

## 4.3 Security Logs

After setting up security monitoring for your access logs and your execution logs you can't forget about security logs.

These are going to be logs that are generated by services in AWS that act in a security context and the first of these is Amazon Inspector 

### Security Logs - Amazon Inspector

Amazon Inspector findings are actually stored directly in the inspector service.

Amazon Inspector pushes to Amazon inspector as this service acts as a storage layer for those findings.

You also have an opportunity to send those findings directly to an SNS topic using an IAM role.

Amazon Inspector feeds into Amazon Inspector 

### Security Logs - Amazon GuardDuty

Amazon GuardDuty feed into its self and based on if there is an IAM role, into CloudWatch Events

### Security Logs - AWS CloudTrail

Cloudtrail if IAM role is implemented it feeds into CloudWatch Logs to store the Security Logs

### Mitigate the event

Contain the compromised resource

There is an EC2 with Application security group with rules below

**Security Group inbound**
| Protocol| Port | Source |
|---|---|---|
| ICMP | ALL | VPC CIDR |
| TCP | 22 | Bastion SG |
| TCP | 8080 | ALB SG |

**Security Group Outbound**
| Protocol | Port | Source |
|---|---|---|
| none | none | none |

If we want to dive deeper in to the EC2 instance then you need to put it in a containment SG (Security Group)

For this containment SG you can have the following:

**Security Group Inbound**
| Protocol | Port | Source |
|---|---|---|
| none | none | none |

**Security Group Outbound**
| Protocol | Port | Source |
|---|---|---|
| none | none | none |

Through the dashboard or CLI you need to make sure to detach SG from EC2 instance

Net effect: EC2 instance isolate from network

### Recover Safely

Restore capacity while retaining forensic data

EC2 is put into containment SG

Leave isolated resources in place for analysis

AMI of EC2 instance gets used to build EC2 in the same Application Security Group and have capability recovered

Restore capacity while retaining forensic data 

Data points go into a bucket

Persist all logs and data points in durable storage for post mortem analysis

Post mortem and feedback loop 

A bucket containing Forensic data send the data to Redshift queries and sent to Insights and recommendations as if it were part of PostGRE database

A bucket containing forensic data can send its log to Athena queries

We could perform jobs that require code to be executed using Glue Jobs

We could use QuickSight dashboard to be able to explore this data in a database

Now our final step is to take our recommendations and turn those into action and so what kind of actions might we expect to see that we can turn into preparation to improve our security

How about more restrictive security group rules if we find that an old rule was left in place and that is what our attacker used to get in

#### Turn recommendations into action

Better monitoring, active responses
Look into different metrics having more alarms so we are not reacting when the event occurs

## 4.4 Log Processing

There are so many ways to process logs in AWS,
I've choosen examples that are common enough that they may actually show up on the certification exam but this is by no means an exhausted list

### Amazon Kinesis Data Stream

Amazon Kinesis Data Stream does require that you have one or more producers that are generating log based data and these are placed into the data stream using a push mechanism from the producers

Then pulled from the stream using consumers running some sort of code and a Kinesis Client Library 

### Amazon Kinesis Firehose/Analytics
Similar to the Kinesis data stream is Kinesis Fire hose.

With Kinesis Firehose, you have multiple options for analyzing or modifying the data while it is in the service

Kinesis Analytics tool gives you the ability to execute SQL queries on the information as it passes through the fire hose

If you have the ability to actually transform the records using a Firehose source record transformation that can be used to execute a lambda function now that you have multiple options for destinations for your Kinesis Firehose.

You can place those records directly into S3, you can insery them into a RedShift database

You can place them into an ElasticSearch Instance

Or through that source record transformation you could actually place those records in an outside service such as Splunk

### AWS Athena

Now assume your data is already in storage and you would like to perform some kind of operations or analysis upon it

AWS Athena gives you the ability to query data that is rest in an S3 bucket 

### Amazon RedShift Spectrum

Amazon Redshift Spectrum gives you a similar ability but using the Redshift service

### AWS Glue

Works on multiple data sources that might be in one or more accounts and give you the ability to perform ETL jobs and other analytics

You create a data catalog that you can then use for performing other queires or gaining insights

### EMR - Elastic Map Reduce

One of the most common services that you should have in your toolkit for performing analytics on large data sets

It is a managed service for Hadoop and it can use an S3 bucket as a source for a data and simply place the data in the same or another S3 bucket when you're done with it

### Amazon ElasticSearch and Kibana

ElasticSearch is another really cool solution because it gives you the opportunity to front it with a Kibana dashboard 

There are some built-in integration points with the rest of AWS including Kinesis Firehose and CloudWatch Log Group and these can be placed directly into an elastic search instance and then queried directly from that Kibana dashboard using a broswer

### CloudWatch Logs Insights

- AWS CloudWatch logs insights gives you anyoher way for querying your data where it sits currently in a service like CloudWatch Logs and because you can place your data in CloudWatch logs from a number of other services in AWS or from custom applications 

This could be a one-stop-shop for analyzing all of your log data and you have the benefit of direct integration to create Cloud Watch Alarms where you can then take direct action or send notifications

### AWS Lambda

Swiss-army knife for analytics and because it has direct integration with services like Kinesis Firehose using the source record transformation

You can use S3 event notifications or even execute the Lambda function directly from Custom code and scripts

This gives you the ability to perform any kind of arbitrary analytics that might not be perfectly suited to another service

## 4.5 Case Study: Automated Log Management

You will find out you have requirements of storage of the logs and for expiration of those logs

### Case Study: Automated Log Management

Scenario: New monitoring requirements to persist all logs in S3 and expire all entries in CloudWatch log groups after 365 days

How can you:
    1. Transfer logs from CloudWatch Logs to S3?
    2. Implement automated log expiration across all log groups?

#### Transfer Logs to S3

CloudWatch Logs is going to use Kinesis Firehose to put the logs into a S3 bucket.

Integration between services require permissions and configuration

You will create an IAM role with trust policy allowing use by Kinesis Firehose

Associate IAM role with permissions policy allowing write access to S3 bucket

Now we move on to the configuration portion where ypi will create firehose delivery stream using IAM role ARN from previous step and S3 bucket ARN

For the next step, we are going to create an IAM role with trust policy allowing use by CloudWatch Logs

Associate IAM Role with permissions policy allowing access to Kinesis Firehose

Now we create a subscription filter using IAM role ARN created in previous step and delivery stream ARN (Requires CLI)

Bonus round: 
Use Kinesis Analytics for executing SQL queries on log stream for analysis or filtering

#### Implement Automated Log Expiration

For the second half of our scenario we need to implement automated log expiration on any CloudWatch Log group whenever it is created.

We will use CloudWatch Events, AWS Lambda, and CloudWatch Logs 

Start with permissions; Create IAM role with trust policy allowing use by AWS Lambda

Associate IAM role with permissions policy allowing modify privileges to CloudWatch Logs

Next, we will create Lambda function that sets expiration days on log group passed as a parameter

Create Events rule that matches new CloudWatch Logs group creation

# Module 4: Infrastructure Security

## 5.1 Edge Security

1. Design edge security on AWS
2. Design and implement a secure network infrastructure

### Edge security - Ingress Points

- CloudFront
- S3
- API Gateway
- Elastic Load Balancer
- AWS Service API endpoints
- VPC Ingress (next section)

Cloudfront when you cache assets, they will be stored in edge locations

Lambda @ Edge for logic evaluation

You can put a WAF for traffic filtering rules

Once it has gone through the global scope, it is not regional service scope

You can use S3 bucket for static assets

You can allow access to your objects by using an API Gateway for Restful interfaces

You can use the Elastic Load Balancer for traffic distribution

AWS service API endpoints are hosted at regional service scope

## 5.2 VPC Network Security

### Network Security - Single VPC

- Subnet
- NACL
- Route Table
- Security Group

#### Single VPC - Subnet

- Isolate workloads
- Apply NACL and route tables
- Finer granularity for CIDR related security features
- Monitor with VPC Flow Logs

#### Single VPC - NACL

- Deny outbound traffic to unauthorized internet destinations
- Enforce compliance for application tier traffic
- Reject inbound traffic to database subnets from non-application sources
- Prevent compromised instance from exploring the network

#### Single VPC - Route Table

- Enable Internet accessibility
- Limit outbound access to specific CIDR range - even /32
- Allow least-privilege access to on-premises networks
- Enable traffic flow to authorized endpoints

#### Single VPC - Security Group

- Implement least privilege by whitelisting appropriate external sources
- Allow inbound traffic only from specific upstream sources (like ALB)
- Remove default outbound rule for apps that should never initiate outbound traffic
- Limit access to endpoints by rules based on subnet CIDR or security group membership

## 5.3 VPC Egress Security

### Network Security - VPC Egress

- Internet Gateway
- Virtual Private Gateway
- VPC Peering Connection
- Gateway endpoint
- Interface Endpoint (PrivateLink)
- NAT Gateway
- DiY

#### VPC Egress - Internet Gateway

- Enable Internet access with no blacklist capability
- Use more specific route tables entries to limit outbound access
- If internet access not required, least privilege dictates avoiding use of IGW
- Used by both VPN and Direct Connect
- Can be used to route traffic to and between multiple customer networks (VPN CloudHub)
- No native features for blacklisting traffic
- Private network link between two VPCs
- Requires route table entries, use specific CIDR for least privilege
- Restrict traffic with security groups and NACL

#### VPC Egress - Gateway Endpoint

- Only used for access to DynamoDB and S3 (12/2018)
- Traffic that passes this endpoint is not charged
- The services you access, the DyanmoDB tables and S3 buckets must be present in the same region as the VPC where this gateway endpoint is configured
- Keep traffic private by avoiding public AWS network
- Apply resource-level permissions on all requests

#### VPC Egress - Interface Endpoint

- Access other service API, API Gateway, and Marketplace products (charged service)
- Keep traffic private by avoiding public AWS network
- No public IP addresses required
- Can access cross-region resources, cross-account resources, and even resources hosted by other customers

#### VPC Egress - NAT Gateway

- Only outbound access allowed
- Apply NACL for least privilege
- Apply specific route table entries for least privilege

#### VPC Egress - DiY

- Use for inline IDS/IPS and DLP
- Full control over resource, can install by hand or use MarketPlace products
- Can act as router, proxy, or security appliance

## 5.4 Multiple VPC Strategies

### Network Security - Multiple VPC

- Same region
- different account
- Different region
- Transit gateway

#### Multiple VPC - Transit Gateway

Manage network interconnectivity through attachments and route tables

#### Mutliple VPC - Considerations

- More security groups
- More NACL
- More route tables (use Transit Gateway)
- More routes
- More egress points
- Higher operational overhead

## 5.5 Case Study

Scanrio: Design a secure infrastructure with multiple networks for hosting and operations
How can you:

1. Design an application/network infrastructure for HIPAA compliant workloads
2. Design a DevOps infrastructure for shared use
3. Utilize AWS services to eliminate traffic between the two networks

### Securing PHI through Network Design

- Create subnets according to Internet accessibility
    - Direct Internet Access
    - Indirect Internet Access
    - No Internet Access

- Different route tables for each subnet
    - 0.0.0.0/0 routed in IGW for direct internet access subnet. Traffic will be forwarded with internet gateway subnet
    - 0.0.0.0/0 routed to NAT GW per subnet for indirect interney access subnet
    - No route for 0.0.0.0 for No Internet access subnet

- Ensures compliance through NACLs
    - Reject outbound to VPC-only subnets for direct internet access
    - Reject all non-application traffic in/out for indirect internet access
    - Accept inbound to DB from app only for No internet access subnet

### Securing PHI via Application Infrastructure

- Only managed services in public subnets
    - Use ALB with WAF ACL on front end

- Only temporary resources for application hosting
    - Use ECS/Fargate on indirect internet access

- On the backend, data persisted using synchronous replication
    - Use RDS/Aurora

### Securing PHI via Security Groups

- Front-end subnet
    - Inbound 443 Outbound 8080 to app

- Middle-end subnet
    - Inbound 8080 Outbound 3306 to DB

- Back-end subnet
    - Inbound 3306
    - No outbound

### Create a multi-purpose DevOps Infrastructure 

How do we access Jenkins?
How does Jenkins access source code?

