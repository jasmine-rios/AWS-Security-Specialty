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
- 