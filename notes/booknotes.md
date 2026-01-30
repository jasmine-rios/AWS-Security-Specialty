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

## AWS Control Tower

AWS Control Tower: Build and govern a multi-account environment following best practices and security recommendations by setting up a landing zone using multiple AWS services such as AWS Organization, AWS identity center, and AWS Service catalog. Enables best practices, extending capabilities of AWS Organizations.
Account factory in Control Tower alllows you to provision new accounts and infrastructure, facilitating account deployment and governance by enforcing standardized configuration and security controls in your environment

### Landing Zone

Landing zone: Reference architecture that holds all accounts in your orgnaization, and it is structured using organizational units, roles, users. and other resources.
The common structure of a landing zone includes the following:
  - Root: Parent that contains all other OUs
  - Security OU: This organizational unit contains two accounts created by Control Tower--Log Archive and Audit. It is possible to customize those names when launchuing your landing zone or when bringing existing accounts under Control Tower management
  - Sandbox OU: This is an optional organizational unit commonly used for experimentation purposes.
  - Identity Directory: This cloud-native directory in IAM Identity Center holds preconfigured groups and permissions. However, you can also manage your own identity provider if you do not want to use the default configuration.
  - Users: These are the identities that youe users can assume to log in to your accounts. We highly recommend you federate with your authoritice directory service.

### Controls

A control is a security guardrail that supports the governance of your AWS accounts.
There are three types of controls:
  - Preventative: As its name implies, it prevents an action from occuring, for example, removing CloudTrail from the Log Archive account.
  - Detective: This control detects specific events when they occur and logs the action for further analysis in a Config Compliance Rule.
  - Proactive: These rules have the capacity to check compliance before the actual resource is provisioned in your accounts using Infrastructure as Code (IaC) with CloudFormation hooks, preventing the creation if resources are not compliant.

Three categories of guidance apply for controls: manadatory, strongly recommended, and elective. You can decude to enforce strongly recommended and elective controls from Control Tower, but mandatory controls cannot be changed.

### Account Factory

Control tower offers Account factory as a built-in account template that can be used to standardize the provisioning of a new account with approved account settings. 
It is possible to update, manage, and even close accounts that you create using Account Factory.
In addition, it is possible to use this feature to change the orgniazational unit for an account in your environment.

This Account Factory feature is also known as a "vending machine", as it creates and enrolls new AWS accounts and automates the process of configuring controls and policies for those accounts that are joining your organization.

## Secure and Consistent Infrastructure Deployment in AWS

It is possible to treat deployment of infrastructure in the same way that developers treat code development: using principles of DevOps. That means code is expected to be developed in a defined format and syntax and stored in a source control system that logs historic changes to create applications in a consistent, reliable, and repeatable way.

### Infrastructure as Code using AWS CloudFormation

AWS Cloudformation allows you to create templates to design, provision, and manage AWS resources in a predictable and reliable way by taking care of configurations and dependencies.

It is possible to replicate your infrastructure in different accounts or even different regions, reusing your templates to create resources in a repeatavle manner.
Since those templates are text (YAML or JSON), you can easily track changes using a version control system, opening the possibility to roll back changes or just use a previous version if needed

#### Stacks

CloudFormation offers a simple way to manage Infrastructure as Code, using templates to create a **stack** as a collection of resources that are provisioned in an orderly manner as a single unit.
The service makes underlying API calls to the same services declared in your template; therefore, it needs proper permissions to successfully complete those actions.

Since all resouces in a stack are treated as a group, they all need to be created or deleted for the stack to work properly.
If a resource cannot be created, the CloudFormation service rolls back and deletes all resouces created previously. If a resouce cannot be deleted, all other resouces are retained until the stack can be successfully deleted.

When you need to make changes to a stack, you can apply changes instead of deleting or creating a completely new stack. 
To do so, you submit the template, and CloudFormation compares the current state of your stack and updates only the changed resources.

#### StackSets

AWS Cloudformation StackSets is a feature that extends capabilities of stacks by enabling you to manage stacks in multiple accounts and regions.
You can manage your StackSets from a central administrator account and use your templates as a source for provisioning stacks with resources on target accounts and regions in your organization.

### Tagging Strategies

Tags are useful to manage, identify, search, and filter resouces. They are commonly used to add categories by purpose, owner, organizational unit, billing department, or even environment.
Tag names and tag values are case sensitive.

Another common use case is to get billing reports with costs broken down by tag.

It is important to highlight that IAM policies support tag-based conditions, letting you enforce permissions based on tags or tags values.
However, keep in mind that support for tag-based and resouce-level permissions is specific per service.
Be sure to restrict who can modify tags of your resources when using them to control access through IAM policies to avoid bypassing your access controls.

#### Best practices for Tagging

Some best practices to use for your tagging strategy are as follows:

1. Use a standardized, case-sensitive format.
2. Do not include sensitive data in tags.
3. Use automated tools such as Tag Editor to helo manage resouce tags.
4. Enforce tagging standards using AWS Organization through service control policies.

### Sharing Resources across AWS Accounts

In a multi-account environment, it is possible to create a resource and share it with other accounts or organizational units using AWS Resource Mananger (AWS RAM).
This feature gives you the possibility of avoiding duplication of resources in every account, reducing operational overhead and simplifying security management, since all access in RAM resources is managed by a single set of policies and permissions.

A good example of using AWS RAM is for sharing a private certificate authority (CA) resouce, allowing AWS Certificate Manager users in other accounts to issue certificates signed by your centralized CA.

### Deploying Portfolios of Approved Services

AWS Service Catalog is a service that allows you to centrally manage a portfolio of resources to govern your Infrastructure as Code (IaC) templates, which can be developed in CloudFormation or Terraform standards.

Using Service Catalog, you can offer a curated catalog of resouces for your customers to quickly provision approved architectures without needing direct access to underlying services. These services can include container images, servers, databases, and more.

Users can browse a light of products they have access to, select what they need, and launch it on their own, while administators can restrict where the product can be deployed, the type of instances to be used, and many other configuration options, resulting in a standardized provisioned product.

Administrators of catalogs can use resou



