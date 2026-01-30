# Chapter 3: Management and Security Governance
# Delegated Administration

The term delegated administrator is used when one or more member accounts are used as administrator of a specific service that allows you to reduce the usage of the management account.

When you register an account as a delegated administrator, you grant access to perform policy actions that are, by default, available only to the management account for that service

## Delegated Administrator Services

AWS Account management: Delegate the administrtion of alternate contact information for all accounts in your organization.

AWS Audit Management: Allows you to continuously audit your accounts across your organization. It automates evidence collection to access risk and compliance with regulations and industry standards.

AWS CloudTrail: You can create an organization trail that logs all events in all accounts in your organization. We strongly recommend setting a centralized management log of all activities in your organization.

AWS Config: You can assess, audit, and evaluate configuration of your AWS resaources. You can aggregate views of your resources' configuration and compliance data for your entire organization.

Amazon Detective: Analyze, investigate, and quickly identify the root cause of security findings or suspicious activities to conduct faster and more efficient security investigation in your organization.

AWS Firewall Manager: Allows you to centrally configure and manage AWS WAF, AWS Shield Advances, Amazon VPC security groups and network ACLs, AWS Network Firewall, and Amazon Route 53 Resolver DNS Firewall across the accounts in your organization.

Amazon GuardDuty: View findings to identify suspicious and potentially malicious activity for all accounts in your organization.

IAM Access Analyzer: Identify resources shared with external principals and unused access findings in one account for further analysis and investigation. Also it analyzes the policies attached to your resources to detect unintended or overly permissive access, helping to secure your environment and meet compliance requirements.

Amazon Inspector: Allows you to automatically scan your EC2, Lambda, and ECR resources for security vulnerabilities. You can also aggregate findings for the entire organization and manage suppression rules.

Amazon Macie: Consolidated view of all of your sensitive objects in your S3 buckets, discovering and classifying your sensitive data.

AWS Security Hub: View the security posture of the entire organization. You can configure the service to automatically enable it for all the memeber's accounts in your organization, aggregating findings including from third-party products and custom security tools.

AWS IAM Identity Center: COnfigure and manage user access to AWS accounts and applications

AWS Trusted Advisor: Provides guidance and recommendations to improve security, performance, cost optimization, and reliability

AWS Control Tower: Build and govern a multi-account environment following best practices and security recommendations by setting up a landing zone using multiple AWS services such as AWS Organization, AWS identity center, and AWS Service catalog. Enables best practices, extending capabilities of AWS Organizations.
Account factory in Control Tower alllows you to provision new accounts and infrastructure, facilitating account deployment and governance by enforcing standardized configuration and security controls in your environment

Landing zone: Reference architecture that holds all accounts in your orgnaization, and it is structured using organizational units, roles, users. and other resources.
The common structure of a landing zone includes the following:
  - Root: Parent that contains all other OUs
  - Security OU: This organizational unit contains two accounts created by Control Tower--Log Archive and Audit. It is possible to customize those names when launchuing your landing zone or when bringing existing accounts under Control Tower management
  - Sandbox OU: This is an optional organizational unit commonly used for experimentation purposes.
  - Identity Directory: This cloud-native directory in IAM Identity Center holds preconfigured groups and permissions. However, you can also manage your own identity provider if you do not want to use the default configuration.
  - Users: These are the identities that youe users can assume to log in to your accounts. We highly recommend you federate with your authoritice directory service.

## Controls

A control is a security guardrail that supports the governance of your AWS accounts.
There are three types of controls:
  - Preventative: As its name implies, it prevents an action from occuring, for example, removing CloudTrail from the Log Archive account.
  - Detective: This control detects specific events when they occur and logs the action for further analysis in a Config Compliance Rule.


