+++
title = "Deploy the Solution"
date = 2025
weight = 2
chapter = false
pre = "<b>2. </b>"
+++

### Deploy the Solution

The Innovation Sandbox on AWS solution uses four AWS CloudFormation stacks that need to be deployed in a specific order. Each stack must be deployed to the correct account and AWS Region to ensure proper operation. Throughout the deployment process, use the same home Region identified earlier.

#### Overview of CloudFormation Stacks

The deployment consists of four CloudFormation stacks, each building on the previous one:

- **AccountPool Stack**
  - Deployed on the organization management account
  - Creates the AWS organization structure
  - Sets up platform policies and controls

- **IDC Stack**
  - Deployed on the organization management account
  - Configures AWS IAM Identity Center integration
  - Sets up identity and access management

- **Data Stack**
  - Deployed on the hub account
  - Deploys data storage and management components
  - Configures backend services

- **Compute Stack**
  - Deployed on the hub account
  - Sets up the solution infrastructure
  - Creates the web interface and processing components

#### Stack Relationship Diagram

Each CloudFormation stack has a dependency relationship with other stacks and is deployed on different accounts, ensuring the solution's components work together.

![architect](/images/stack-relationships.png "Architect")

#### CloudFormation Templates

To deploy the solution, use the following CloudFormation template URLs (The original AWS workshop had some errors with the Compute Stack, so the author has replaced it with a drive link containing the new stack. If this link is broken, please contact hieunghiwork123@gmail.com):

| Stack         | Template URL                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------------|
| AccountPool   | https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-AccountPool.template |
| IDC           | https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-IDC.template         |
| Data          | https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-Data.template        |
| Compute       | https://drive.google.com/file/d/1oJ9DeANLgpscQSEGuO_gk7rxiDSTWf-_/view?usp=sharing     |

#### Before you begin

Before deployment, ensure you have completed the following steps:

- Complete all steps in the Prerequisites module
- Ensure you have access to both the organization management account and the hub account
  - The account used to set up AWS Organizations or AWS Control Tower is the organization management account

**Estimated time:** About 45 minutes

Deploying in the correct order and to the correct account/region will ensure the Innovation Sandbox on AWS solution operates effectively and complies with organizational governance policies.

### Deploying the AccountPool Stack

Deploying the AccountPool stack is the first step in the organization management account. This stack creates the organization structure and foundational policies needed for the solution.

#### Deployment Steps

**Log in to the AWS Management Console**

- Open the AWS Management Console.
- Log in with your organization management account.
- Switch to your chosen home Region.

**Gather necessary information**

- **HubAccountId:** This is the AWS account ID where you will deploy two of the four stacks. To see all accounts in your organization, go to AWS Organizations in the console, select an account to be the hub, and record its ID for use in step 5 and subsequent steps.
- **ParentOuID:** This is the ID of the root or organizational unit (OU) where the Innovation Sandbox OUs will be created. To find this ID, open AWS Organizations; the organization structure will display the ID under each OU's name. You can also use the root organization ID.

![architect](/images/organiza.jpg "Architect")

**Initialize the stack**

- Open the AWS CloudFormation console.
- Select Stacks, then choose "Create stack".
- Choose "With new resources (standard)".

**Specify the template**

- Choose "Amazon S3 URL".
- Enter the template URL for the AccountPool stack:  
  `https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-AccountPool.template`
- Choose "Next".

**Enter stack details**

| Parameter         | Description                                                                 | Example              |
|-------------------|-----------------------------------------------------------------------|----------------------|
| Stack name        | Stack name (optional)                                                  | fcj-accountpool      |
| Namespace         | Unique identifier for this deployment (3-8 characters, letters/numbers) | fcj                  |
| HubAccountId      | AWS Account ID of the hub account                                      | 123456789012         |
| ParentOuId        | Root ID or OU ID where the Innovation Sandbox OUs will be created      | r-emd9               |
| IsbManagedRegions | Comma-separated list of allowed Regions                                | us-east-1,us-west-2  |

**Additional notes:**

- **Namespace:** Use the same value for all four stacks. Remember this value for subsequent steps.
- **HubAccountId:** Enter the hub account ID obtained in step 2.
- **ParentOuID:** Enter the root ID or OU ID obtained in step 2.
- **IsbManagedRegions:** Enter the list of Regions that Innovation Sandbox users are allowed to access.

**Configure stack options**

- Review advanced options if needed.
- Add tags if desired.
- Choose "Next".

**Review and create the stack**

- Review all the information you have entered.
- Choose "Submit" to start creating the stack.

**Check deployment progress**

- Reopen the CloudFormation console and select Stacks.
- Find the newly created stack and check its status. Wait until the status changes to "CREATE_COMPLETE".

You have successfully deployed the first stack. Proceed to deploy the second stack following the next instructions.

### Deploying the IDC Stack

The IDC (Identity Center) stack is the second component of the Innovation Sandbox on AWS solution. This stack is deployed on the organization management account and is responsible for configuring integration with AWS IAM Identity Center and setting up access management for the solution.

#### Deployment Steps

**1. Log in to the AWS Management Console**

- Open the AWS Management Console.
- Log in with your organization management account.
- Switch to your chosen home Region.

**2. Gather information for step 5**

- Open the IAM Identity Center console.
- Select Settings.
- Copy and save the **Identity store ID** and **Instance ARN** values for use in step 5.

**3. Initialize the stack**

- Open the AWS CloudFormation console.
- Select Stacks, then choose "Create stack".
- Choose "With new resources (standard)".

**4. Specify the template**

- Choose "Amazon S3 URL".
- Enter the template URL for the IDC stack:  
  `https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-IDC.template`
- Choose "Next".

**5. Enter stack details**

| Parameter         | Description                                                        | Example                                   |
|-------------------|--------------------------------------------------------------|-----------------------------------------|
| Stack name        | Stack name (optional)                                         | fcj-idc                                 |
| Namespace         | The same namespace value as in the AccountPool stack              | fcj01                                   |
| HubAccountId      | AWS Account ID of the hub account                             | 123456789012                            |
| IdentityStoreId   | Identity store ID from IAM Identity Center (step 2)            | d-1234567890                            |
| SsoInstanceArn    | ARN of the SSO instance in IAM Identity Center (step 2)      | arn:aws:sso:::instance/ssoins-12345678901234567 |

**Additional notes:**

- **Namespace:** Use the same value for all four stacks, same as the previous stack.
- **HubAccountId:** This is the AWS Account ID where two of the four stacks will be deployed.
- **IdentityStoreId** and **SsoInstanceArn:** Obtained from the Settings section of IAM Identity Center in step 2.

**6. Configure stack options**

- Review advanced options if necessary.
- Add tags if desired.
- Choose "Next".

**7. Review and create the stack**

- Review all the information you have entered.
- Choose "Submit" to start creating the stack.

**Check deployment progress**

- Reopen the CloudFormation console and select Stacks.
- Find the newly created stack and check its status. Wait until the status changes to "CREATE_COMPLETE".

You have successfully deployed the IDC stack. Proceed to deploy the third stack on the hub account following the next instructions.

### Deploying the Data Stack

The Data stack is the third component of the Innovation Sandbox on AWS solution. This stack is deployed on the hub account and is responsible for setting up the data infrastructure and necessary backend services for the solution.

#### Important Note

- Ensure you are logged into the correct hub account.
- Use the same AWS Region as in previous deployments.

#### Deployment Steps

**1. Log in to the AWS Management Console**

- Open the AWS Management Console.
- Log in with your hub account.
- Switch to your chosen home Region.

**2. Initialize the stack**

- Open the AWS CloudFormation console.
- Select Stacks, then choose "Create stack".
- Choose "With new resources (standard)".

**3. Specify the template**

- Choose "Amazon S3 URL".
- Enter the template URL for the Data stack:  
  `https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-Data.template`
- Choose "Next".

**4. Enter stack details**

| Parameter    | Description                                                    | Example   |
|------------|----------------------------------------------------------|---------|
| Stack name | Stack name (optional)                                     | fcj-data|
| Namespace  | The same namespace value as in previous deployments   | fcj01   |

- **Namespace:** Use the same value used in the previous stacks (e.g., fcj01).

**5. Configure stack options**

- Review advanced options if needed.
- Add tags if desired.
- Choose "Next".

**6. Review and create the stack**

- Review all the information you have entered.
- Choose "Submit" to start creating the stack.

**7. Check deployment progress**

- Reopen the CloudFormation console and select Stacks.
- Find the newly created stack and check its status.
- Wait until the status changes to "CREATE_COMPLETE".

You have successfully deployed the Data stack. Proceed to deploy the final stack on the hub account following the next instructions.

### Deploying the Compute Stack

The Compute stack is the final component of the Innovation Sandbox on AWS solution. This stack is deployed on the hub account and is responsible for setting up the solution's infrastructure, including the web interface and processing components.

#### Important Note

- Ensure you are logged into the correct hub account.
- Use the same AWS Region as in previous deployments.

#### Deployment Steps

**1. Log in to the AWS Management Console**

- Open the AWS Management Console.
- Log in with your hub account.
- Switch to your chosen home Region.

**2. Initialize the stack**

- Open the AWS CloudFormation console.
- Select Stacks, then choose "Create stack".
- Choose "With new resources (standard)".

**3. Specify the template**

- Choose "Upload a template file".
- Upload the template file for the Compute stack from your computer.
- Choose "Next".

**4. Enter stack details**

| Parameter     | Description                                                          | Example       |
|---------------|----------------------------------------------------------------------|---------------|
| Stack name    | Stack name (optional)                                                | fcj-compute   |
| Namespace     | The same namespace value as in previous deployments                  | fcj01         |
| EmailAddress  | Email address to receive notifications and alerts. Must be verified in Amazon SES. | user@example.com |

- **Namespace:** Use the same value used in the previous stacks.
- **EmailAddress:** Enter a verified email address in Amazon SES to receive solution notifications.

**5. Configure stack options**

- Review advanced options if needed.
- Add tags if desired.
- Choose "Next".

**6. Review and create the stack**

- Review all the information you have entered.
- Acknowledge that AWS CloudFormation might create IAM resources with custom names.
- Choose "Submit" to start creating the stack.

**7. Check deployment progress**

- Reopen the CloudFormation console and select Stacks.
- Find the newly created stack and check its status.
- Wait until the status changes to "CREATE_COMPLETE".

After the Compute stack is successfully created, go to the Outputs tab and save the value of `CloudFrontDistributionUrl`. This is the URL to access the Innovation Sandbox on AWS solution.
