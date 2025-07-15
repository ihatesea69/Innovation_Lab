+++
title = "Prerequisites"
date = 2020
weight = 1
chapter = false
pre = "<b>1. </b>"
+++

### Requirements before deploying Innovation Sandbox on AWS

#### Overview

To deploy the Innovation Sandbox solution on AWS, you need to prepare some basic steps for your AWS environment. These steps include:

- Identifying necessary AWS accounts
- Choosing AWS region
- Setting up AWS Organization
- Enabling the following:
  - AWS IAM Identity Center
  - AWS Resource Access Manager (RAM)
  - AWS CloudFormation StackSets
  - Service Control Policies (SCPs)

**Estimated time:** About 15-20 minutes (may take longer if you need to create new AWS accounts)

#### Identifying necessary AWS accounts

- Innovation Sandbox uses AWS Organizations features, helping you centrally manage and control your AWS environment when scaling.
- You need to choose an initial account (called the management account).
- There are two options for the initial account:
  - Use an existing AWS account
  - Create a new AWS account

**Note:**
- If you already have an AWS organization: the initial account is the existing management account.
- If creating a new AWS organization: you can use an independent AWS account (which will become the management account) or create a new account for this role.
- This account will be the management account for your entire AWS organization.

#### Choosing AWS home Region

- All Innovation Sandbox components must be deployed in the same AWS region.
- The chosen region needs to meet:
  - Full support for required services: AWS Organizations, IAM Identity Center, CloudFormation, DynamoDB, Lambda, CloudWatch, Secrets Manager, AppConfig.
  - Where IAM Identity Center has been enabled.
  - Accessible to all users expected to use the solution.

**When choosing AWS region, consider:**
- Geographic location, distance to users, data storage requirements, network latency.
- Service costs and price differences between regions.
- Data transfer costs between regions.
- Price differences for each service.

#### Setting up AWS Organizations

- You need an AWS organization with at least 4 accounts:
  - Management account
  - Hub account
  - Sandbox account 1
  - Sandbox account 2

- You can add more sandbox accounts as needed.



**Steps to create a new AWS organization:**
- Log in to AWS Management Console with administrative rights.
- Open AWS Organizations console and choose "Create an organization".
- Verify management email according to instructions sent to mailbox.
- Add member accounts:
  - Can create new accounts (provide name, root email, keep default IAM role).
  - Or invite existing AWS accounts to the organization (by email or account ID).
- Repeat until you have at least 4 accounts with the following structure:


```
Root
├── Management account
├── Hub account
├── Sandbox account 1
└── Sandbox account 2
```
![architect](../images/anh2.jpg "Architect")

![architect](../images/Anh1.jpg "Architect")

- Check in the Settings section of AWS Organizations console:
  - Consolidated billing is enabled
  - All features are enabled

#### Enabling required services

Before deploying the solution, ensure the following services are enabled on the management account:

- **AWS IAM Identity Center:** Centralized access management for AWS accounts.
  - Open IAM Identity Center in management account, check correct region, choose Enable.

- **AWS Resource Access Manager (RAM):** Securely share AWS resources between accounts in the organization.
  - Open RAM in management account, go to Settings, enable "Enable sharing with AWS Organizations", save.

- **CloudFormation StackSets trusted access:** Manage CloudFormation stacks across multiple accounts/regions.
  - Open CloudFormation in management account, choose StackSets, enable "Enable trusted access".

- **Service Control Policies (SCPs):** Manage access permissions across the entire organization.
  - Open AWS Organizations in management account, go to Policies, choose Service control policies, enable this feature.
