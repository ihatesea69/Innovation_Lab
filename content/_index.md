+++
title = "Innovation Lab"
date = 2025
weight = 1
chapter = false
+++

# Building a Comfortable Environment for Innovation and Learning on AWS


## Workshop Overview

**Innovation Sandbox on AWS Workshop** is a guide to help learning groups or companies have a place to experiment and try things out without worrying about security or costs.




![architect](/images/high-level.png "Architect")
### Main Objectives


- **Manage separate AWS accounts for experimentation**
- **Convenient cost management through web interface**.


## Knowledge Requirements (Should know beforehand)

- Knowledge of **AWS Organizations** and **AWS IAM Identity Center**
- Basic understanding of **AWS CloudFormation** (just need to understand the concept since template files are already provided)

## Implementation Time

- **Total implementation time:** About 2 hours (may take longer if you want to dive deep)

## Main Solution Components

| Component                | Primary Role                                                                                           |
|--------------------------|-------------------------------------------------------------------------------------------------------|
| AWS IAM Identity Center  | User authentication, access management                                                               |
| Amazon CloudFront        | Distribute user interface, serve web from S3, forward API to Lambda                                  |
| AWS WAF                  | Security filtering for dynamic routes                                                                 |
| AWS AppConfig            | Store solution configuration                                                                          |
| AWS Organizations        | Manage multiple AWS accounts under one organization, group by OU, manage entire environment          |
| AWS Lambda               | Execute provisioning logic, cleanup, sandbox account management, dynamic interfaces                  |
| Amazon EventBridge       | Trigger tasks based on time or events                                                                |
| AWS Step Functions       | Manage sandbox account state according to logical processes                                           |
| AWS accounts             | Sandbox accounts prepared and distributed to end users                                               |

![Account LifeCycle](images/sandbox-account-ou-lifecycle.png "a title")

## Account Lifecycle

1. **Account onboarding**: Accounts are brought into the system and cleaned up before use.
2. **Available**: Accounts ready to be provisioned to users.
3. **Active**: Accounts in use, system monitors cost and usage time.
4. **Frozen**: Accounts frozen when exceeding budget or time, or by administrator.
5. **Cleanup**: When accounts expire, system automatically cleans up resources.
6. **Quarantine**: If cleanup process fails, accounts are quarantined for reprocessing.
7. **Exit**: Accounts can be removed from system when necessary.

## Detailed Explanation of Account Lifecycle Diagram

The account lifecycle diagram describes the main states of a sandbox account in the system:
- **Available**: Account ready for provisioning.
- **Active**: Currently in use, monitoring cost and time.
- **Frozen**: Frozen when exceeding spending/time limits or by administrator request.
- **Cleanup**: Clean up account when usage expires.
- **Quarantine**: Quarantine if cleanup fails, can retry.
- **Entry/Exit**: Onboard or remove account from system.





## Workshop Deployment Process

### 1. Prerequisites (Preparation)
- Prepare AWS environment, create accounts.
- Time: 15-20 minutes (may take longer if creating new accounts).

### 2. Deployment
- Deploy CloudFormation stacks, configure infrastructure, set up management.
- Time: ~45 minutes.

### 3. Configuration
- Configure SAML integration for authentication, set up user access, onboard sandbox accounts.
- Time: ~15 minutes.

### 4. Using the sandbox
- Learn how to use the solution through three different roles.
- Time: ~30 minutes.

#### References Links:

https://catalog.us-east-1.prod.workshops.aws/workshops/23f635fc-dc98-4f75-8b91-6f334d0e22c3

https://docs.aws.amazon.com/solutions/latest/innovation-sandbox-on-aws/solution-overview.html

https://aws.amazon.com/solutions/implementations/innovation-sandbox-on-aws/
