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

Administrators of catalogs can use resource tags during deployment and then grant access to the final product using AWS IAM users, groups, policies, and conditions

Add a product to any number of portfolios without creating additional copy.
Updating the product to a new version will propagate the update to every portfolio that references it.

## Evaluating Compliance

One important aspect of evaludating compliance in your environment is to understand what needs to be protected and apply a correct classification to the data so you can, in turn, apply adequate security controls to protect it.

In this section, we review three important services that can help you effectively evaluate compliance in your AWS workloads.

### Data classification Using Amazon Macie

Using a service such as Amazon Macie, you can automatically discover sensitive data in your Amazon S3 buckets.
The service gives you the possibility of doing that in two different ways.
The first is by evaluating your inventory on a daily basis and smapling techniques to identify representative objects from your buckets.
The second way is enabling discovery jobs that provide a deeper, more targeted analysis where you can select the scope and criteria to be used.
In addition, you can configure on-demand only once or on a recurring basis to maintain continous monitoring of your data

#### Data Identifiers

Amazon Macie has two types of data identifiers that you can use to discover sensitive data in your buckets:
- AWS-managed: These are built-in identifiers designed to detect specific types of senstive data. For example, AWS access keys, credit card numbers or identification numbers from multiple regions and countries, personally identifiable information (PII), and financial information.
- Custom identifiers: You can create custom criteria for detecting senstivie data. Identifiers are build using regular expressions (regex), defining a text pattern to match character sequence and a proximity rule. These can be used to define specific use cases that are not covered by AWS-managed identifiers.

There is a additional element that you can use to refine the results.
The *allow lists* function gives you the ability to specify checks and patterns to be ignored during discovery of sensitive data, for example, credit card numbers when using a test enviornment.
Amazon Macie will not report an occurence of matching if an element is declared in your list and found in managed or custom identifiers.

Amazon Macie can help you meet compliance with your data security requirements by producing reports of sensitive data found during analysis.
A finding is sensitive data found in an S3 object, and a data discovery result is the actual record of the analysis performed.

At the time of writing this chapter, Amazon Macie only supports discovering sensitive data in Amazon S3 buckets.
However, keep in mind that you can still move data, such as snapshots of databases in parquet format or any other format supported by the service, temporarily to buckets for analysis.

#### Evaluate Configuration Using AWS Config

AWS Config is a service that can help you audit resources to ensure compliance with policies and best practices by accessing historical configuration and changes.
A good example is the possibility of accessing historical configuration of security groups assigned to an EC2 instance, including port rules that were open at a specific time.

You can use Config Rules or Conformance Packs to configure AWS Config.
The first allows you to evaluate compliance for the type of resource you select, to record activity, and send the results to an Amazon S3 bucket, or to set up a topic to get notifications using Amazon SNS.
The second is a collection of rules that can be deployed and monitored as a single entity.

The service also gives you the opportunity to use aggregators to get a centralized view of your resource inventory and compliance. 
An aggregator collects configuration and compliance data from multiple accounts and regions in a single place.

# Chapter 4 Identity and Access Management

## Introduction

Various ways to do AWS IAM services:
- AWS Management Console
- AWS Command Line Tools
- AWS Software Development Kits (SDKs)
- IAM Query API

**AWS IAM features that can help you secure your AWS cloud environment, including multifactor authentication (MFA) rotating keys, federation, AWS IAM roles, and other security best practices.**

You might want to consider Amazon Cognito for identity management?
**why?**

Because Amazon Cogninito is SSO and works with a lot of different providers

What are *principals*?

A principal is an AWS IAM entity that has permission to interact with resouces in the AWS cloud wheather permanent or temporary, this principal can represent a human user, a resource, or an application.

There are 3 types of principals:

- root users
- IAM users
- IAM roles (or temporary security credentials)

Principals include federated users and assumed roles.

## Root users

When you create your first account on AWS, you begin with a single identity that has complete access to all AWS resources in the account, this identity is called the *root user identity*.

You can log into the AWS Console using the email address and password that you provided when you created your account/

The root user credentials give you unrestricted access to all the AWS services in your account.

This access covers (but is not limited to) viewing billing information, changing your root account password, and performing the complete termination of your account and deletion of its resource

For daily operations in your AWS account in your AWS account, it is not necessary to use the root user.
Moreover, AWS highly recommends that you **not share the root user credentials with anyone, simply because doing so gives them unrestricted access to your account**
There is no way to restrict the access given to the root user in the account.
However, if the account is a member of an AWS Organization, it is possible to use SCPs to restrict root yser account permissions.

Once you have set up your AWS account, the most important thing that you should do is protect your root user credentials by following these recommendations:

- Use a **strong password** to help protect account-level access to the management console.

- Enable virtual or hardware MFA with time-based one-time passwords on yourAWS root user account.

- Remember that you should avoid creating an access key for programmatic access to your root user account unless such a procedure is manadatory. You should create another IAM user with the required permissions to perform the necessary actions on your account. It is possible to give administrative permissions to an IAM user.

- In case you must maintain an access key to your root user account, you should regularly **rotate it** using the AWS Console.
You need to log in using your account's email address and password to rotate (or even delete) your access key credentials for your root account.
- Remember, **never share your root user password or access keys** with anyone.
- Terminate and destroy all your resource access to the **root user account can terminate and destroy all resouces in your AWS account**.

## Resetting Root Users

You **MUST** be signed in as the root user to change the root user password or anything else related to the **user**.

To execute follow these steps to **/change root user password/**

1. Use your email address and password to log in to the AWS Console as yhe root user.

2. Click on the name of your account in the upper-right corner.

3. Choose Account option from the menu.

4. In the Account page, next to the Account Setting pane, click Edit.

5. Once you click, you will be redirected to the Login page to confirm your root credentials. From the options presented, choose Root User and enter the email registered for your root account.

6. Click Next, and on the following screen, enter your root password and click the Sign In button.

7. If you have MFA enabled on your root account, enter the MFA code at the prompt.

8. After you confirm your root credentials, you'll see the Update Account Settings page. Click the Edit button next to the password field.

9. Use a strong password for you account, with a minimum of 8 and a maximum of 128 characters. It should contain a combination of the following character types: uppercase, lowercase, numbers, and special characters. Don't use simple passwords, such as *january*, *password*. *pa4ssw0rd*, or your date of birth, which can be easily cracked through **dictionary attacks**.

**TIP**

  If you need to reset the root account credentials, remember to delete the previous two-factor authentication information, as well as any access and secret keys that might exist for the root account.

**EOT**

  It is important to remember to **enable MFA on the account and avoid creating root access keys**.


Follow these steps to enable virtual MFA for the root account:

1. Log in to your AWS account using your root account credentials.

2. Click the name of your account and then select Security Credentials.

3. On the My Security Credentials screen, close to the Multifactor Authentication (MFA) pane, click on Assign MFA device.

4. On the select MFA Device screen, type a name in the Device Name field and select the Authenticator App Option. Then click Next.

5. Ensure that you have an authenticator app on your mobile phone or computer. A list of compatible applications can be found on the setup device screen.

6. On the next screen, click Show QR Code and scan it using your authenticator app.

7. Enter the two consecutive codes in the MFA code 1 and MFA Code 2 fields.

8. Click Add MFA. Now, every time you login with your root credentials, you must provide an MFA code as part of the login process.

### IAM Users

IAM users are people or applications in your organization.

They live within the AWS accounts where they are created but may have cross-account permissions if configured to do so.

Each IAM user has its own username and password that give them access to the AWS Console.

Additionally, it is possible to create an access key to provide users with programmatic access to AWS resources.

IAM users is an entity that gives administrators the granularity required to control how users should interact with AWS resources.

Note that an IAM user is not necessarily a person.

It can be a an application (such as SalesApp) that runs on your AWS or on your corportate network and needs to interact with resources in your account.

If your application is running on AWS, you should use IAM roles to provide temporary credentials for the application to access AWS resources.

However, if your application is running on your corporate network, you can create an IAM user and generate access keys, so SalesApp can have the required credentials to access your resources.

In order to avoid using your root user account, you should create an IAM user or assume an IAM role and then assign administrator permissions for your account so that you can add more users when needed.

**Note**

Always remember to enforce the principle of least privilege when creating your users--that is, users should have only the minimum level of permissions they need to perform their assigned tasks.

You should define fine-grained policies associated with your IAM users.

**End of Note**

#### Exercise: Create an IAM user with adminstrator access permissions

In this exervice, you will learn how to create a new IAM user with adminsitrator permissions.

1. Log in to your AWS account and open the IAM service. Select Users from the left-sode menu.

2. From the Users pane, click Create User.

3. In the User Name field, type MyAdminUser. Click on the combo box to provide access to the AWS Management Console.

4. For User type, select "I want to create an IAM user".

5. Choose whether you want to use an autogenerated password to provide your own in the Console Password section.

6. Keep selected the option to create a new password at the next sign-in.

7. Click the Next button.

8. On the Set Permissions page, select the Attach Polcieis Directly option.

9. Choose Administrator Access from the policy list.

10. Click the Next button.

11. On the Review page, click the Create User button.

12. On the next screen, you can send the credential details by email or download a CSV file with the credential information.

### IAM Groups

An IAM group is a good way to allow administrators to manage users with similar permissions requirements.

Adminstrators can create groups that are related to job functions or teams, such as adminsitrators, developers, QA, FinOps, and operations. They can then assign fine-grained permissions to these groups

When you add IAM users to a group, they inherit the group's permissions.

With such a simple practice, it becomes easier to manage bulk changes in your environment and move IAM users around as they change or gain new responsibilities in the company.

One important thing for you to note: An IAM group is not an identity because it cannot be reffered to as a principal when you're setting up permission polcieis.

It is just a logical organization that allows you to attach policies to multiple users all at once.

Try to avoid assigning permissions directly to IAM users since these permissions are hard to track when your organization scales and users move to different organizational positions.

### IAM Roles

Similar to IAM users, *IAM roles* can have a permission policy that determines what the IAM role can and cannot do in AWS.
IAM roles are not exclusively associated with an IAM user or group; they can also be assumed by other entities, such as application or services.

Additionally, IAM roles do not have long-term credentials or access keys directly assigned to them during creation.

When a role is assumed, it is granted temporary credentials by a service called AWS Security Token Service (STS).

Such temporary credentials are valid throughout the role session usage.

The defauly lifetime of temporary security tokens issued by STS is for a maximum of 12 hours and can be configured from 15 minutes to 36 hours.

These security credentials can be requested for a root user but restricted to a duration of 1 hour due to security reasons.

Roles can be used to delegate access to resource that services, applications, or users do not normally have.

For example, you can allow an application to assume a role that provides access to a resource in a different AWS account even if its orgininal permissions did not allow such access.

Another example is if your user only has access to a development account and you grant them the ability to publish content in teh production account and you grant them the ability to publish content in the production account by assuming an IAM role.

Yet another example is if you grant an external application access to AWS resources, but instead of using fixed credentials inside the application (which are easy to extract and hard rotate once deployed), you can leverage an IAM an IAM role for this purpose.

Futhermore, if you want to give access to users who already have identities on your corporate authoritive directory or from another IdP, it is possible to change these credentials to **IAM role credentials that provide them with temporary access to AWS resouces**.

IAM roles can be used in following scenarios:

- Grant permissions to an **IAM user** in the same AWS account as the role.

- Grant permissions to an IAM user in a different AWS account than the role, which is also called **cross-account access**.

- Provide access for non-AWS workloads using temporary credentials.

- Provide access to your accounts by a third party by creating roles to assume with correct permissions.

- **Grant permissions to AWS services by controlling what a service can access using service roles.**

- In user federation scanrios, it's possible to use IAM roles to grant permissions to external users authenticated through a trusted IdP.

#### AWS Security Token Service

The AWS Security Token Service is designed to provide trusted users and services with **temporary security credentials** that control access to AWS resources.

This service provides the foundation for other features such as cross-account access, service roles, and identity federation.


The main differences between long-term access keys and temporary security credentials issued by AWS STS are as follows:

- When you issue a temporary security credential, you can specify the expiration interval of that credentials, which can range from a few minutes to several hours. **Once expired, these credentials are no longer recognized by AWS, and any API requests made with them are denied**.

- Temporary credentials are dynamic and generated every time a user requests them. **A user can renew the temporary credentials before their expiration if they have permission to do so**.

#### Roles for Cross-Account Access

Roles for cross-account access grant users of one AWS account access to resources in a different account. Such a procedure enables a different set of scenarios such as API, CLI, calls or AWS console access.

**One common use case can be two AWS accounts, such as Dev and Prod, where users from the Dev account need specific access on the Prod account** 

The regular permissions for a developer in the Dev account do not allow them to directly access the resources in the Prod account.

However, it is possible to define a trust policy.

# Chapter 4 Objectives/Review

- Principal: root users, IAM users, and IAM roles

- It is a good security principal to avoid creating an access key for programmatic access to your root user account unless such a procedure is mandatory.

- Remember to rotate your programmatic keys and access keys.

# Chapter 5: Security Logging and Monitoring

## Objectives

- Domain 1: Threat Detection and Incident Response

  - 1.2 Detect security threats and anomalies by using AWS services

- Domain 2: Security Logging and Monitoring

  - 2.1 Design and implement monitoring and alerting to address security events
  - 2.2 Troublshoot security monitoring and alerting
  - 2.3 Design and implement a loggin solution
  - 2.4 Troublshoot logging solutions
  - 2.5 Design a log analysis solution

- Domain 6:
  - 6.3 Evaluate the compliance of AWS resources

## Introduction

- "IF a tree falls in the middle of the forest and nobody hears it does it make a sound."

- Put your sensors in the right places as you cannot see inside a system.

- This practice is known as *observability*.

- These resources are not isolated, they interact with internal and external components and respond to those interactions therefore these resources are dynamic.

- In this chapter you will learn how to gather information about the status of your resources and most importantly the events related to them.

- These events represent how the resources are being affected and how they are interacting with other resources, either internal or external to your organization.

- Detecting, logging, monitoring, and presenting events in teh form of observable records is not the end of a process.

- With AWS cloud, you have great capabilities, such as big data analysis and automation, that add great value to to extract findings out of hude amount of raw detected data.

- Deliver as processed events in the form of observable records.

- Automation allows you to evolve from being a mere viewer to a security enforcer.

- Remediation controls are not in scope but important.

- You will learn the basics about automated remediation capabilities of the detective controls. 

- Blend these capabilities with the enlarged visibility the AWS Cloud provides and you will understand one of the main reasons multiple professionals acknowledge that they are more secure in the cloud.

- To explain the various detective controls, follow the framework below to see how incident response can be stagged.

**Insert diagram**

- The framework is split into four main stages:

  - resource state
  - events collection
  - event analysis
  - action

- The flow starts with a collection of resources and the act of keeping track of your collection of resources and it's configuration and status over time.

- These resources are the "objects" that are under observation.

- They can be AWS resources such as
  - Amazon EC2
  - Amazon API gateway
  - REST API on *Amazon API Gateway*
  - SaaS applications
  - AWS Cloud services

There are stages to incident response:

1. Resources State
2. Events Collection
3. Event Analysis
4. Action

- First stage, the *resources state*
  Just like a tree falling in the middle of the forest, an event can happen at any time, and you need the right tools to record it as an observable record.

- Secound stage, is *events collection*, deals with registering the events occuring in the environment.
  The creation of these records can be passive or active.
  A record is passively created when external sources are responsible for sending event notifications to the detective control.
  By contrast, a record is actively created if the detective services intentionally looks for information.

  There are AWS Cloud services that use both methods.

- Detective services can generate a large amount of information, and the security professional faces the challenge of extracting value from that information.

- Third stage, *event analysis*, where the raw records are processed to produce value-added information.
  This analysis can be done in different ways.
  For instance, it can compare the event with a best practice or a baseline and inform you when differences exist between the desired and the current situation.

  - A service can also use statistics to determine whether the event is normal, or even leverage machine learning techniques to identify suspicious operations.

  - At this stage, the service can also have a passive or active posture.

  - In comparison, a passive posture characterizes an analysis based on the recieved observable records, while an active posture intentionally gathers additional information from the environment.

  - The final result of the third stage is also a respository of observable records, but in this case, they are a direct result of an analytical process.

  - AWS services can provide a user-friendly view of these observable records (both before and after the event analysis stage) Tools are available to manage those records, supporting a workflow to give the proper follow-up to security events.

  - Both the unfiltered observable records and the analyzed records respositories provide mechanisms for consuming them, in addition to visualizing or requesting the records via API calls.

  - These mechanisms can be classified as batches or streams in light of how they grant access to the information.

  - Processing as a batch means producing a historical view of the records that you can work on.

  - Services can also provide access to observable records as a stream, which allows you to recieve the event as soon as it is reported.

- Forth stage, is the *action* stage, you connect to the detection controls with reactive actions, through the **magic of automation in the cloud**.

  You will learn more in Chapter 8, "Threat Detection and Incident Response" 

  This chapter introduces you how some detective services can generate a response to observable events.

  In the action stage, Amazon EventBridge is arguably one of the most powerful tools in the AWS Cloud.

  The role of EventBridge is to connect the source of events with consumers who can respond to those events.

  This chapter introduces several AWS Cloud services supporting differnent detective activities.

  Although most of them carry out tasks for multiple stages in the detective control flow framework, you analyzw each one in light of its primary goal within the framework.

  Stage 1: Resources State

  The first stage in the detective framework focuses on knowing the state of the monitored resources.

  AWS provides tools that assess the current situation of resources at different levels and keep historical track of their state.

  Of course, having that kind of information enables AWS services to work into other stages via trigger actions and call to other services.


### AWS Config

AWS resources inside your account have their own configuration at each point in time.

AWS Config is the services that allows you to keep track of the configuration of these AWS resources.

In Chapter 2 you learned about the AWS Shared Responsibility Model.

The observability of events is heavily based on that model.

AWS Config provides you with information about monitored resources, visible to AWS because it falls in their part of the shared responsibility model, AWS Config obtains information like:

- Instance ID
- IAM role
- IP addresses
- Security groups from Amazon EC2 instances

AWS Config does not have accesses to the processes that running into the instance.

AWS Config allows you to monitor several types of resources inside your account, including compute, serverless, databases, storage, and security, among many others.

It starts monitoring by turning on a configuration recorder.

This component's mission is to keep track of configuration items (a document containing the configuration information of a resource) for the monitored resources, updated them each time a resource is created, updated, or deleted.


The service provides one configuration recorder per account per region. 

You can define the resources you want to monitor per configuration recorder, as a choice between all supported resouces types (Current and future), define expectations, or define a subset of them.

AWS Config documentation provides an updated list of supported resource types.

This collection of resouces is called the *recording group*.

Once you have the configuration reoroder created you can stop and start it via API calls or through the AWS Management Console.

After the configuration recorder is successfully started, it is in "recording on" mode, and it tracks changes of the monitored resources by recording a new configuration item when a change in the configuration is detected, either in the monitored resources itself or in any of its related monitored resources (Called *relationships* which you learn about later in this section).

The CLI command `describe-configuration-recorder-status` returns the status of the configuration recorder.

**NOTE**

The configuration recorder captures information by calling the APIs of the monitored resouces. It takes a few minutes for the configuration recorder to update the changes after they occured.

**EON**

In essence, the configuration recorder saves each monitored resource's configuration and updates the information according to thr detected changes.

The information gathered from each monitored resource is store in a construct called the **configuration item**.

You can think of a configuration item as a JSON object that contains the configuration of a resource from the AWS point of view (POV).

In fact, this JSON file contains the following information:

- Metadata
- Attributes
- Tags
- Resource ID
- Resource Type
- Creation time
- Amazon Resource Name
- Availability zone
- Relationships
- Current configuration

The current configuration section corresponds to the information that is retrieved by calling the describe or list APIs of the resource.

### Relationships

Relationships are descriptions of connections among different resources.

For example, an Amazon EC2 instance has a relationship with a network interface.

From the network interface's standpoint, the relationship has the name "is attached to" (the instance); from the instance's point of view, the relationship has the name "contains" (the network interface).

Such information is described as part of the JSON object.

The available relationship are described in detail in the AWS Config documentation, if you are looking for more inforamtion

**Notes**

You can define the retention period for the configuration items, from 30 days to 2,557 dats (7 year, then default configuration)

**EON**

#### AWS Config Deep Dive

AWS Config provides you with a repositiory of configurations for each monitored resource. You can look for a specific resource (or group of resources) and ask for their current configuration item.

You can do it through the AWS Config Console, under the Resources menu, or you can use the `BatchGetResourceConfig` API.

You can also use a SQL-like syntax to query information from the current configuration state of a monitored resource. This feature, called advanced querires, allows you to look for resource information directly inside the configuration items, without directly called APIs to the resource.

The queries can be executed directly within the AWS Management Console or by calling the `SelectResourceConfig` API

Moreover, AWS Config can provide a holistic picture of the current configuration: a configuration snapshot. 

In practice, a configuration snapshot is a JSON file that contains the current configuration for all the monitored resources.

The file is delivered into an Amazon S3 bucket you own

You can manually create such a snapshot by calling the `DeliverConfigSnapshot` API at any time, or by scheduling a periodic delivery to occure every 1,3,6,12, or 24 hours.

At this point, you have seen AWS Config recording static configurations; however, these resources are not static in nature.

As they change their configuration over time, AWS Config (with its configuration recorder on) keep track of those changes.

Not only that, but AWS Config allso correlates the changes in a resource with the events that produced it (for example, the API call that produced a change in the resource's configuration).

AWS Config takes a "photo" of the new configuration each time a detected change happens and store that new configuration in conjuction with information about what caused the change.

The sequence of "pictures" for a specific resource is known as a **resource timeline**

Consequently, AWS Config keeps the observable records of events along with configuration items. The service acts in a passive way when resources inform AWS Config that a change occured, but it also acts in an active way because at that point, AWS Config calls the APIs to get informtaion abou the new status of the resource.

In the AWS Config Console, you can access the current configuration of a monitored resource by accessing the resource view by doing

**AWS Config > Resources > Name > Timeline**

From there, Click the Resource timeline button, and the AWS Config Console will present you with a time-ordered list of recorded events that affected the resource, compose of configuration, compliance, and CloudTrail events (as shown in picture).

If you want to obtain a list of configuration items (current and past) for a monitored resource, you should call the `GetResourceConfigHistroy` API

**Note**

By default, the AWS Config Console provides a list of events in reverse chronological order. SO the Start Date field referes to the most recent date for which the history is requested.

**EON**

As you can see, AWS Config records very useful information.

To expose such information to the external world, the service leverages the concept of a delivery channel.

Think about the delivery channel simply as the mandatory setup inside your configuration recorder, which defines an S3 bucket and an optional SNS topic that AWS Config uses to deliver information (such as the configuration snapshots) and notifications

Depending on how the information is organized and delivered through the delivery channgel, you can have different views.

Besides configuration snapshots and configuration timelines, AWS Config also provides a configuration history:

**A collection of recorded configuration items that changed over a specified time period**

AWS Config automatically delivers configuration history files every 6 hours to the S3 bucket configured in the delivery channel.

Such a collection contains all configuration items of the monitoring resources that changed since the last delivery (if there were several changes during that period, each one of the configuration items would be part of the configuration history files).

Each configuration history file corresponds to a different resource type.

The information collected by AWS Config can also be provided in a stream, which means being notified as soon as change is detected.

This method is called a configuration stream, and it uses the topic defined in the delivery channel.

The same topic is used to deliver several notifications (like the creation of a historical record or a snapshot), so AWS Config uses the `messageType` key inside the message body to signal with information the notification contains.

For the configuration stream, the value for the `messageType` key is `ConfigurationItemChangeNotification`.

| Configuration View | Contains | Frequency | Deliver Channel |
|--------|-----|-----------|-----------|
| Configuration History Files | Files with all configuration items (grouped by resource type) of resources that changed since last delivery | 6 hours (fixed) |Amazon S3 bucket (`ConfigHistory` prefix) |
| Configuration Snapshot | One file with all the current configuration items | Manual or configurable to 1, 3, 6, 12, 24 hours| Amazon S3 bucket (`ConfigSnapshot` prefix)|
| Configuration Stream| Configuration items as messages in a topic, delivered as soon as they were detected by config | Continuous (within minutes) | Amazon SNS topic `messageType: ConfigurationItemChange`|

With what you have learned, you can already insert the AWS Config capabilities in the detective framework proposed in this chapter's introduction.

The figure describes already inserting the AWS Config capabilities in the detective framework proposed in this chapter's introduction

  Description of the image: 
    - **Stage 1** has the resources which are AWS resources and Custom Resources as well as a database of current and historical configuration

    - **Stage 2** Is where the events of the resources are held in the database of Observable Records (Change Events). This is where also the changes (CRUD- Create, Read, Update, and Delete)

    - **Exposition (to the Outside World)**:
      - **On-Demand Access and visualization** has the configuration timeline and resources and advanced query to console and apps

    - **Batch** has the configuration snapshot and configuration history and uploads it to an S3 bucket
    
    - **Stream** is where configuration streams are sent to an SNS topic

The main goal of AWS config is to record configuration and changes of the resources and not analyze them.

In other words, AWS Config does not judge if a change is good or bad, or if it needs to execute an action.

Such a role, will be accomplished by a componenet called *AWS Config Rules*, which is dicussed in the "Stage 3: Events Analysis" section.

When considering multiple AWS accounts and regions, AWS Config provides the concept of an aggregator. 

An **aggregator** is a Config resource type that allows you to access information about resources and compliance outside your current region and account.

To set up an aggregator, first you choose one account and region to host it.

Then, you configure the accounts and regions your aggregator will have access to (referred as source account and source regions).

AWS Config integrates with the concept of AWS Organizations, so you can easily make all of the accounts inside one organization report to an aggregator.

An aggregator can read configuration and compliance data recorded by AWS Config, but it cannot modify a resource's configuration.

AWS Config can also integrate with external resources like on-premises servers and applications, third-party monitoring applications, or version control systems. You can publish configuration items for these resources to AWS Config, so you can manage the resource inventory.

You can also have the resources registered with CloudFormation.

In this case, if you apply changes to the resources using AWS CloudFormation templates, they will be recorded by AWS config.

**NOTE**
The AWS Config recorder component can be disabled at any time, which will cause it to stop checking for nre changes of the monitored resources, and so any other analysis will be disabled from that point on.

However the recorder will keep available previous records until the end of the configured retention period

**EON**

#### Exercise 5.1 Set Up AWS Config

1. Access the AWS Config Console.

2. If this is the first time you've configured AWS Config in the chosen region, use the Get Started wized. Otherwise, open the Settings menu, click in the Edit button, and make sure the Enable Recording check box is selected.

3. Under the General Settings section, choose "Record All Current and Future Resource Types Supported in this region" as your recording strategy. Select "Create AWS Config Service-Linked Role" (or "Use an Exisiting AWS Config Service-Linked Role" if you're not using the wizard) as the IAM role for AWS Config. Leave the "Include Globally Recorded Resource Types" check box unselected.

4. Under the Delivery Method section, select Create a Bucket. Use the S3 bucket name of your preference, and type `security-chapter5` as its prefix. This will be the S3 bucket where AWS Config will deliver its files.

5. Still within the Delivery Method section, mark the "Stream Configuration Changes and Notifications to an Amazon SNS Topic" check box. Select `Create a Topic` and name the topic `config-topic-security-chapter5`.

6. If you are using the Get Started wized, click `Next` to finish the first part of the configuration. Otherwise, click `Save` and skip to Step 9.

7. Click Next in the AWS Managed Rules page (at this point, you won't configure any AWS Config rules).

8. Review the final page and click Confirm.

9. Subscribe an email account to the Amazon SNS topic you created in Step 5.

10. Execute changes in your account: launch a new EC2 instance. Once the new instance is running, modify the configuration of the security group attached to it.

11. Wait 15 minutes.

12. In the AWS Config Console, open the Resources menu. Select AWS EC2 Instance and AWS EC2 Security Group under the Resource Type field. The recently launched instance and the modified security group appear in the list.

13. In the resource list, click on the security group ID. Browse through the configuration item and resource timeline details. Do the same with the resource representing and recently launched EC2 instance.

14. Check the email account you configured in Step 9. You should have recieved notifications relate to your newly launched EC2 instance, and the modified security group.

15. (Optional) Using AWS CloudShell, execute the `deliver-config-snapshot` command to have a snapshot (use the `describe-delivery-channels` command to gather information about Config delivery channels). Check the S3 bucket you created in Step 4. Look for snapshots files with `ConfigSnapshot` in the prefix.

16. (Optional) Wait for 6 hours and check the S3 bucket for history files with `ConfigHistory` in their prefix.

### AWS Systems Manager

AWS Systems Manager is a comprehensice service that assembles many capabilities. 

As a whole, the service simplifies the administration of large fleets of instances (Amazon EC2, on-premises servers, or other cloud providers), especially for operational activities. 

In the past, this service was known as AWS EC2 Simple Systems Manager.

Today, you can refer to it simply as SSM (an acronym you will find in this book, as well as in the AWS offical documentation).

Because of its myraid features, when approaching the AWS Systems Manager service, it is easier to analyze through the lens of its capabilities are grouped under these four categories:

- **Operations management**: Provides the current state of your environment and how its component are performing. It covers features such as *Explorer*, *OpsCenter*, *CloudWatch Dashboard*, and *Incident Manager*.

- **Application management**: Administers application distributed among several components, environments, and AWS accounts.

- **Change management**: Allows you to specify a sequence of actions to be executed on your managed instances, and how to control its execution.

- **Node Management**: Manages instances and nodes at scale.

Some SSM capabilities allow you to interact with monitored resources (instances) at deeper levels (like gathering information directly from the operating system or applications, executing commands inside the operating system, or establishing a terminal administration channel to the instances).

This deeper interaction is possible due to a software component called the *SSM agent*.

The agent acts as the representative of the service inside the instance.

One of the SSM's capabilities, called *inventory*, leverages the SSM agent to extract metadata (such as software and applications deployed within the instance), enabling the state tracking of these resources.

Once the information is collected, SSM can export it to an S3 bucket of your own (by configuring a component called *resource data sync*).

In SSM Inventory, you define the type of data you want to collect and how frequently; once an initial sync is completed, the resource data sync will keep the information in your Amazon S3 bucket updated accordingly.

To access such information, you can query the Inventory capabililty in SSM (via console or API calls) or information in S3 (through Amazon Athena, or Amazon QuickSight).

You can also use an embedded functionality in the SSM Inventory console called *detailed view*, which provides a way to directly query inventory information in your S3 bucket (exported by a resource data sync).

This functionality in face uses Amazon Athena and AWS Glue.

Next, the application manager capability (under the Application Management category) provides you with a better view of AWS resources in your account by grouping them under the concept of an *application* (a logical group of resources that you want to operate as a single unit).

You can create a custom application by defining resource groups, or by a set of tags that your components share.

You can also create a custom enterprise workload (like SAP HANA).

Finally, you can define applications by importing group of resources from *AWS CloudFormation*, *Amazon EKS*, *Amazon ECS*, *AWS Service Catalog AppRegistry*, and *AWS Launch Wizard*.

Once your application is defined in the *Application Manager*, it will consolidate data provided by other services like *AWS Config*, *AWS Billing*, and *Amazon CloudWatch* as well as providing other SSM capabilities.

The Operations Management category is mostly applicable to stage 1 of the detective framework. The *Explorer*, *Ops Center*, and *CloudWatch* dashboards capabilities grouped under this category do not create or analyze new information. Their focus is to provide a central point of view for several operationals metrics (like CloudWatch dashboards), and a list of created operational tickets (ops items).

As such, this capability is a visialization tool that aggregates different information into a single pane.

#### Stage 2: Events Collection

Events are at the core of the detection task. These event represent the activities that are happening, affecting the resources and producing changes.

In a dynamic environment, these changes are constantly occuring.

A dective system will capture those changes and convert them into observable records.

At the second stage of the framework presented in the chapter's introduction, the focus is not on deviding if the event is good or bad, but on capturing as many interesting events as possible and representing them as records in a repository.

Therefore, the collection of events into observable records is at the core of a detective control in this stage.

#### AWS CloudTrail

All the requests to AWS Cloud services happen through authenticated and authorized API calls.

AWS CloudTrail is the service in charge of keeping records of these calls.

The service also records non-API events related to service actions and to sign-in attempts for the AWS console, AWS Discussion Forums, and AWS support center.

Actions taken by an IAM principal (such as a user, a role, or an AWS service) are recorded as events in AWS CloudTrail.

The basic unit of activitiy recording in AWS CloudTrail is called an *event*.

An event is represented by the record that contains the logged activity. There are three types of events:

- **Management**: Operations performed on AWS resource, or control plane operations, and non-API events, like sign-in operations.

- **Data**: Logs of API operations performed on or within a resource, also known as data plane operations (for example, `PutOBject` or `GetObject` operations on an Amazon S3 object or an `Invoke` operation on an AWS Lambda function)

-**Insights**: "Meta-events", generated by CloudTrail when it detects unusal management API activity.

The fields of a recorded event include the activity description and metadata.

They provide information about *what* was done (for example, `eventName`, `eventSource`, `requestParameters`, `responseElements`, or `resources`), *who* did it (Information about the principal and how it was authenticated; for example, `userIdentity`, which itself includes other attributes like `type`, `principalID`, `arn` and `accountID`, when (eventTime), and *where* (for example, `awsRegion`, `sourceIPAddress`, `userAgent`, or `recipientAccountId`).

In addition, there is context information about the event itself, like `eventVersion`, `eventType`, `eventCategory`, and `tlsDetails` attributes. Depending on the event, the `eventCategory` field takes one of the three values:
  - Management
  - Data
  - Insight

{
  "eventVersion": "1.08",
  "userIdentity": {
      "type": "AssumedRole",
      "principalId:" : "<principalId-number>
      "arn" : "arn:aws:sts:<account-id>:assumed-role/<role>",
      "accountId": "<account-id>",
      "accessKeyId": "<accessket-Id-code>"
      "sessionContext": {
        "sessionIssuer" : {
          "type": "Role",
          "principalId": "",
          "arn" : "arn:aws:iam::<account-id>:role/<role-id>",
          "accountId": "<account-id>"
          "userName" : "<user-name>"
        },
          "webIdFederationData": {},
          "attributes": {
            "creationDate": "2024-01-01T18:14:24Z",
            "mfaAuthenticated": "true"
          },
          "ec2RoleDelivery": "2.0"
      }
  },
  "eventTime": "2024-01-01T19:06:28Z",
  "eventSource: "ssm.amazonaws.com",
  "eventName": "UpdateInstanceInformation",
  "awsRegion": "<input-aws-region>",
  "sourceIPAddress: "<source-ip-address>",
  "userAgent": "aws-sdk-go/1.44.260 (go1.20.10; linux; amd64)"
  "requestParameters": {
      "instanceId": "<enter-instance-id>",
      "agentVersion": "3.2.1798.0",
      "agentStatus": "Active",
      "platformType": "Linux",
      "platformName": "Amazon Linux",
      "platformVersion": "2",
      "ipAddress": "10.0.0.10",
      "computerName": "<enter-computer-name>",
      "agentName": "amazon-ssm-agent",
      "availabilityZone": "<region>"
      "availabilityZoneId": "<region-id>"
      "sSMConnectionChannel": ""
  },
  "responseElemenets": null,
  "requestID": "<insert-request-id>"
  "eventID": "<insert-event-id>"
  "readOnly": false,
  "eventType": "awsApiCall",
  "managementEvent": true,
  "recipientAccountId": "<insert-recipient-id>",
  "eventCategory" : "Management",
  "tlsDetails" : {
    "tlsVerson": "TLSv1.2",
    "ciperSuite": "ECDHE-RDA-AES128-GCM-SHA256",
    "clientProvidedHostHeader": "ssm.us-east-1.amazonaws.com"
  }
}

While management, data, and insights events share most of the attributes in the JSON structure, insights events contain an additional attribute, called `insight-Details`

#### Cloudtrail Insights Events

{
  "eventVersion": "1.09",
  "eventTime": "2024-01-01T20:46:00Z"
  "awsRegion": "<insert-aws-region>",
  "eventID": "<9d876c5b-ae01-432b-88ce-1a876a11fb9b>",
  "eventType": "AwsCloudTrailInsights",
  "reciepientAccountId": "123456789012",
  "sharedEventID": "fe6368fd-2286-4297-b127-157fd96d45ca",
  "insightDetails": {
      "state": "Start",
      "eventSource": "cloudshell.amazonaws.com",
      "eventName": "SendHeartBeat",
      "insightType": "ApiCallRateInsight",
      "insightContext": {
          "statistics": {
            "baseline": {
                "average": 0.0001765848
            },
            "insight": {
              "average": 0.0001765848
            },
            "insightDuration": 5,
            "baselineDuration": 11326
          },
          "attributions": [
            {
              "attribute": "userIdentityArn",
              "insight": [
                {
                  "value": "arn:aws:sts::123456789012",
                  "average": 0.6
                }
              ],
              "baseline": [
                {
                  "value": "arn:aws:sts::544491154384:<role>",
                  "average" : 0.0001765848
                }
              ]
            },
            {
              "attribute": "userAgent",
              "insight": [
                {
                  "value": "<agent-description>",
                  "average": 0.6
                }
              ],
              "baseline": [
                {
                  "value": "<agent-description>",
                  "average": 0.000.1765848
                }
              ]
            },
            {
              "attribute": "errorCode",
              "insight": [
                {
                  "value": "null",
                  "average": 0.6
                }
              ],
              baseline: [
                {
                  "value": "null",
                  "average" 0.0001765848
                }
              ]
            }
          ]
      }
  },
  "eventCategory": "Insight"
}

#### More info on AWS CloudTrail 

CloudTrail is enabled at the creation of an AWS account. Management events recorded within the past 90 days are available in the Event History menu of AWS CloudTrail.

This feature (available throught the AWS Management Console or via the `LookupEvents` API) allows you to view, search, and download management events related to the account during that timeline.

**NOTE**

AWS CloudTrail event history only shows management events, and not all management events are supposed to appear in event history.

**EON**

Insights events are events generated when CloudTrail detectes an abnormal volume of API call rates (anomalous call rate in write APIs) and API error rates (anomalous error rate in both read and write APIs).

CloudTrail Insights are mathematical models and continuous monitoring of management events.

Insights events are available through the AWS Management Console and through `LookupEvents` API.

In the Insights console, you can access a list of insights events recorded for the last 90 days, see details of reported unusual activity, and view a timeseries graph.

You have learned that you can access Management events and insights events for the last 90 days, see details of reported unusual activity, and view a timeseries graph.

You have learned that you can access Management events and insights events for the last 90 days. 

However, effectively managing an account requires a persistent layer that allows you to record events for longer than 90 days.

In addition, to store records for future use (e.g. as evidence for audits), a persistent layer can also provide the foundations to analyze, visualize, and respond to events.

CloudTrail provides two options for that persistent layer: trails and AWS CloudTrail Lake.

A *trail* allows you to store AWS CloudTrail events in an Amazon S3 bucket that you own, in the form of log files.

This way, you can control the life cycle policy and the retention and protection policies of the persisted data. You can create multiple trails.

For each trail, you choose the type of events to record (management, data, or insights).

Every trail must record management or data events (and it can record both), and a trail recording insights events must record management events.

When creating a trail to recieve data events, you use selectors to establish which data to collect in the trail.

Basic selectors are available for Amazon S3 and AWS Lambda data events.

For all other supported services, you use advanced selectors, where you can filter events you want to collect by Amazon Resource Name, read/write operations, or event name.

Each log file is compressed (in `gzip` format) and contains one or more JSON-for-matted records, where each record represents an event. AWS CloudTrail delivers log files several times an hour (about every 5 minutes).

Typically, the log files for management and data events appear in your S3 bucket within 15 minutes after the activity was executed in the account.

Insights events typically appear within 30 minutes after detecting the unusual activity.

AWS CloudTrail stores management and data events in different log files (objects) from insights events.

**NOTE**

A trail has several configuration options. However, the minimum information required to create a new trail is its name and the Amazon S3 bucket name to store log files. By default, a trail records all management events and no insight or data events. The trail is always encrypted (using S3 server-side encryption), but other defauly options differ depending on whether you use the console or the API.

**EON**

**NOTE**

AWS CloudTrail shows insights events in the Management console (or in the `LookupEvents` API) only if there is a trail configured to record those insights events.

**EON**

Once the records are stored in Amazon S3, you can use services, like Amazon Athena and Amazon QuickSight, to visualize and analyze them. Amazon Athena requires the creation of a table to define the structure of the data and its location within S3. Simply enough, AWS CloudTrail provides a way to create that table directly from its console, by clicking the Create Athena Table buttom on the Event History page/

**NOTE**

Using the AWS CloudTrail's console for creating the trail's table is not mandatory; you can also manually create the table in Athena.

**EON**

To protect the stored records, AWS CloudTrail provides encryption and integrity validation mechanisms.

Because the records are stored in an Amazon S3 bucket, they are protected at least with the base level of encryption (using Amazon S3 managed keys).

However, you can choose to use your own AWS Key Management Service (KMS) keys, specifying one key per trail.

If you choose this option, the key policy must allow CloudTrail to use it to encrypt the files and allow choosen principals in the AWS account to decrypt them.

You learn about AWS KMS is chapter 7, "Data Protection"

AWS CloudTrail also embeds a trail integrity validation mechanism. This mechanism uses asymmetric cryptographic techniques (digital signatures) applied to the files delivered in S3 buckets.

Using these techniques, AWS CloudTrail protects the records and allows you to determine if a delivered log file was modified, deleted, or unchanged.

On an horly basis, AWS CloudTrail delivers a digest file as a object in the trail S3 bucket, digitally signed using a service-managed private key (the digital signature is attached to the object in its metadata, specifically in the `x-amz-meta-signature` key) The digest file contains a list (in JSON format) of log files and corresponding SHA-256 hashes, delivered within the past hour.

**Note**

Each digest file includes information about the previous digest file hash and signature, which establishes a sequential chain of digest files

**EON**

#### AWS Cloudtail Digest File Header

{
  "awsAccountId": "12345678012",
  "digestStartTime": "2024-01-01-T23:19:16Z",
  "digestEndTime": "2024-01-02T00:19:16Z",
  "digestS3Bucket": "cloudtrail",
  "digestS3Object": "<path to current digest S3 object>.json.gz",
  "digestPublicKeyFingerprimt": "1234567890b5dc9467b26b16602a50ce",
  "digestSignatureAlgorithm": "SHA256withRSA",
  "newestEventTime": "2024-01-02T00:18:58Z",
  "oldestEventTime": "2024-01-02T23:08:48Z",
  "previousDigestS3Bucket": "cloudtrail",
  "previousDigestS3Object": "<path to digest S3 object from last hour.json.gz>",
  "previousDigestHashValue": "<hash value>",
  "previousDigestHashAlgorithm": "SHA-256",
  "previousDigestSignature": "<digest signature>",
  "logFiles: [
    <structured list including s3Bucket, s3Object, hashValue, hashAlgorithm, newestEventTime, and oldestEventtime for each log file delivered in the past hour>
  ]
}

#### More info on CloudTrail

AWS CloudTrail provides a simple way of validating the integrity of the log files, using the `validate-logs` AWS CLI command.

Trails also provide the option to export CloudTrail events as a stream of events (consistently with the detection framework proposed at the beginning of this chapter). AWS CloudTrail integrates with Amazon CloudWatch Logs, Amazon SNS, and Amazon EventBridge to deliver these streams. The first two integrations (Amazon CloudWatch Logs and Amazon SNS) are configured inside the trail, but it is in Amazon EventBridge configuration where you can set up the third integration.

**NOTE**

You learn more about Amazon CloudWatch Logs later in "Amazon CloudWatch Logs" and about Amazon EventBridge in "Stage 4: Action". At this point, it is important to note that this integration allows you to capture events in Amazon EventBridge for every sercove supported by AWS CloudTrail.

**EON**

Configuring an Amazon SNS topic for a trail allows you to receive notifications whenever a log file is delivered to the trail's S3 bucket.

If you subscribe to an Amazon SNS notification, you can choose a topic residing in your own account or in another account.

In any case, the topic access policy must allow AWS CloudTrail to post messages to it.

The second mechanism AWS CloudTrail provides for persistent storage of events is the CloudTrail Lake. CloudTrail Lake consolidates events records in structures called *event data stores*.

You have the option to create event data stores for CloudTrail events, CloudTrail insights events, AWS Config configuration items, and external events.

The data schema is different for each one of those four types of events.

Therefore, each event data store is able to contain only one event type.

Data ingested into an event data is stored in Apache ORC format, which makes it more efficent to store and run queries.

You can use SQL expressions to query multiple event data stores).

You can also define the retention period to a maximum of 10 years.

An event data store for CloudTrail events stores management and data events. 

You can use advanced selectors (the same concept you learned for trails) to define the data events you want to record.

An event data store for CloudTrail insights events is unique, in that is considered a destination event data store.

It requires an existing data store for CloudTrail events recording management events to act as its source event data store.

A CloudTrail events data store can be the source of one and only one CloudTrail Insights event data store.

AWS Config configuration items event data store allows you to collect configuration items generated by AWS Config recorders/

The recorders will provide information of configuration items when they detect a change, or when taking a Config snapshot.

Once you have data in your configuration items event data store, you can issue queries to correlate data with other data stores (in contrast to AWS Config queries, which only allow you to query AWS Config data).

The external events data store can hold data coming from sources outside of AWS. 

In this case, in addition to creating the data store, you also designate a channel (or integration). 

A channel is a resource that exposes an endpoint (web hook), used by external applications to ingest events into CloudTrail Lake.

