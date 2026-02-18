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
    - 

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

Execution logs are another important way to monitor the security of requests that are passing through your application infrastructure