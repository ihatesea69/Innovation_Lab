+++
title = "Deploy the Solution"
date = 2025
weight = 2
chapter = false
pre = "<b>2. </b>"
+++

### Deploying the lab
Need to run the stacks in the correct order and use the same region

#### Overview of CloudFormation Stacks

Consists of 4 stacks

- **AccountPool Stack**
  - Deploy on organization management account
  - Create AWS Organization
  - Set up policies

- **IDC Stack**
  - Deploy on organization management account
  - Configure AWS IAM Identity Center integration
  - Set up identity and access management

- **Data Stack**
  - Deploy on hub account
  - Deploy storage and data management components
  - Configure backend services

- **Compute Stack**
  - Deploy on hub account
  - Set up solution infrastructure
  - Create web interface and processing components

#### Relationship diagram between Stacks

Each CloudFormation stack has dependencies with other stacks and is deployed on different accounts, ensuring solution components work in coordination.

![architect](/images/stack-relationships.png "Architect")

#### CloudFormation Templates

To deploy the solution, use the following CloudFormation template URLs:

| Stack         | Template URL                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------------|
| AccountPool   | https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-AccountPool.template |
| IDC           | https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-IDC.template         |
| Data          | https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-Data.template        |
| Compute       | https://drive.google.com/file/d/1oJ9DeANLgpscQSEGuO_gk7rxiDSTWf-_/view?usp=sharing     |

Note: The lab workshop has become outdated so the compute part has errors, so the host has updated a new compute template on Google Drive for everyone



### Deploying AccountPool Stack

Deploying the AccountPool stack will create the organization structure and policies

#### Deployment steps

**Log in to AWS Management Console**

- Open AWS Management Console.
- Log in with your organization management account.
- Switch to the chosen home Region.

**Gather necessary information**

- **HubAccountId:** This is the AWS account ID where you will deploy two of the four stacks. To see all accounts in the organization, go to AWS Organizations in the console, choose an account as hub and note the ID for use in step 5 and subsequent steps.
- **ParentOuID:** This is the ID of the root or organizational unit (OU) where Innovation Sandbox OUs will be created. To find this ID, open AWS Organizations, the organization structure will display the ID under each OU name. You can also use the root organization ID.

![architect](/images/organiza.jpg "Architect")

**Initialize stack**

- Open AWS CloudFormation console.
- Choose Stacks, then choose "Create stack".
- Choose "With new resources (standard)".

**Specify template**

- Choose "Amazon S3 URL".
- Enter the template URL for AccountPool stack:  
  `https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-AccountPool.template`
- Choose "Next".

**Enter stack details**

| Parameter         | Description                                                           | Example              |
|-------------------|-----------------------------------------------------------------------|----------------------|
| Stack name        | Stack name (optional)                                                 | isb-accountpool      |
| Namespace         | Unique identifier for this deployment (3-8 characters, letters/numbers) | isb01                |
| HubAccountId      | AWS Account ID of hub account                                         | 123456789012         |
| ParentOuId        | Root ID or OU ID where Innovation Sandbox OUs will be created        | r-abcd               |
| IsbManagedRegions | List of allowed Regions, comma-separated                             | us-east-1,us-west-2  |

**Additional notes:**

- **Namespace:** Use the same value for all four stacks. Remember this value for subsequent steps.
- **HubAccountId:** Enter the hub account ID obtained in step 2.
- **ParentOuID:** Enter the root ID or OU ID obtained in step 2.
- **IsbManagedRegions:** Enter the list of Regions that Innovation Sandbox users are allowed to access.

**Configure stack options**

- Review advanced options if needed.
- Add tags if desired.
- Choose "Next".

**Review and create stack**

- Review all entered information.
- Choose "Submit" to start creating the stack.

**Check deployment progress**

- Open CloudFormation console again, choose Stacks.
- Find the newly created stack and check status. Wait until status changes to "CREATE_COMPLETE".

You have successfully deployed the first stack. Continue with deploying the second stack according to the next guide.

### Deploying IDC Stack

IDC (Identity Center) stack is responsible for configuring integration with AWS IAM Identity Center and setting up access management for the lab.
#### Deployment steps

**1. Log in to AWS Management Console**

- Open AWS Management Console.
- Log in with your organization management account.
- Switch to the chosen home Region.

**2. Gather information for step 5**

- Open IAM Identity Center console.
- Choose Settings.
- Copy and save two values: **Identity store ID** and **Instance ARN** for use in step 5.

**3. Initialize stack**

- Open AWS CloudFormation console.
- Choose Stacks, then choose "Create stack".
- Choose "With new resources (standard)".

**4. Specify template**

- Choose "Amazon S3 URL".
- Enter the template URL for IDC stack:  
  `https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-IDC.template`
- Choose "Next".

**5. Enter stack details**

| Parameter         | Description                                                  | Example                                 |
|-------------------|--------------------------------------------------------------|-----------------------------------------|
| Stack name        | Stack name (optional)                                       | isb-idc                                 |
| Namespace         | Namespace value same as AccountPool stack                   | isb01                                   |
| HubAccountId      | AWS Account ID of hub account                                | 123456789012                            |
| IdentityStoreId   | Identity store ID from IAM Identity Center (step 2)         | d-1234567890                            |
| SsoInstanceArn    | SSO instance ARN in IAM Identity Center (step 2)            | arn:aws:sso:::instance/ssoins-12345678901234567 |

**Additional notes:**

- **Namespace:** Use the same value for all four stacks, same as previous stack.
- **HubAccountId:** AWS Account ID where two of the four stacks will be deployed.
- **IdentityStoreId** and **SsoInstanceArn:** Obtained from Settings section of IAM Identity Center in step 2.

**6. Configure stack options**

- Review advanced options if needed.
- Add tags if desired.
- Choose "Next".

**7. Review and create stack**

- Review all entered information.
- Choose "Submit" to start creating the stack.

**Check deployment progress**

- Open CloudFormation console again, choose Stacks.
- Find the newly created stack and check status. Wait until status changes to "CREATE_COMPLETE".

You have successfully deployed the IDC stack. Continue deploying the third stack on the hub account according to the next guide.

### Deploying Data Stack

Data stack is the third component of the Innovation Sandbox solution on AWS. This stack is deployed on the hub account and is responsible for setting up data infrastructure and necessary backend services for the solution.

#### Important note

- Ensure you log in correctly to the hub account.
- Use the same AWS Region as previous deployments.

#### Deployment steps

**1. Log in to AWS Management Console**

- Open AWS Management Console.
- Log in with your hub account.
- Switch to the chosen home Region.

**2. Initialize stack**

- Open AWS CloudFormation console.
- Choose Stacks, then choose "Create stack".
- Choose "With new resources (standard)".

**3. Specify template**

- Choose "Amazon S3 URL".
- Enter the template URL for Data stack:  
  `https://solutions-reference.s3.us-east-1.amazonaws.com/innovation-sandbox-on-aws/latest/InnovationSandbox-Data.template`
- Choose "Next".

**4. Enter stack details**

| Parameter  | Description                                              | Example  |
|------------|----------------------------------------------------------|----------|
| Stack name | Stack name (optional)                                    | isb-data |
| Namespace  | Namespace value same as previous deployments            | isb01    |

- **Namespace:** Use the same value used in previous stacks (example: isb01).

**5. Configure stack options**

- Review advanced options if needed.
- Add tags if desired.
- Choose "Next".

**6. Review and create stack**

- Review all entered information.
- Choose "Submit" to start creating the stack.

**7. Check deployment progress**

- Open CloudFormation console again, choose Stacks.
- Find the newly created stack and check status.
- Wait until status changes to "CREATE_COMPLETE".

You have successfully deployed the Data stack. Continue deploying the final stack on the hub account according to the next guide.

### Deploying Compute Stack

Compute stack is the final component of the Innovation Sandbox solution on AWS. This stack is deployed on the hub account and is responsible for setting up solution infrastructure, including web interface and processing components.

#### Deployment steps

**1. Log in to AWS Management Console**

- Open AWS Management Console.
- Log in with your hub account.
- Switch to the chosen home Region.

**2. Get management account information**

- You need the organization management account ID for step 5.
- If you don't have it, log in to AWS Management Console with management account.
- Click on account name menu in top right corner.
- Copy the 12-digit account ID for use in next step.

**3. Initialize stack**

- Open AWS CloudFormation console.
- Choose Stacks, then choose "Create stack".
- Choose "With new resources (standard)".

**4. Specify template**

- Choose "Amazon S3 URL".
- Enter the template URL for Compute stack:  
  `https://drive.google.com/file/d/1oJ9DeANLgpscQSEGuO_gk7rxiDSTWf-_/view?usp=sharing`
- Choose "Next".

**5. Enter stack details**

| Parameter         | Description                                                    | Example       |
|-------------------|----------------------------------------------------------------|---------------|
| Stack name        | Stack name (optional)                                         | isb-compute   |
| Namespace         | Namespace value same as previous stacks                       | isb01         |
| OrgMgtAccountId   | AWS Account ID of organization management account             | 123456789012  |
| IdcAccountId      | AWS Account ID where IAM Identity Center was configured       | 123456789012  |

- **Namespace:** Use the same value for all stacks (example: isb01).
- **OrgMgtAccountId:** Organization management account ID.
- **IdcAccountId:** Account ID where IAM Identity Center was configured (usually also the management account).

**6. Configure stack options**

- Review advanced options if needed.
- Add tags if desired.
- Choose "Next".

**7. Review and create stack**

- Review all entered information.
- Choose "Submit" to start creating the stack.

**8. Check deployment progress**

- Open CloudFormation console again, choose Stacks.
- Find the newly created stack and check status.
- Wait until status changes to "CREATE_COMPLETE".

#### After deployment

- Go to CloudFormation console, choose Stacks.
- Choose the just deployed Compute stack.
- Access **Outputs** tab.
- Copy the **CloudFrontDistributionUrl** value.

Example:  
`https://duyXXXXXXXeh.cloudfront.net`

Save this value, you will need it in the next configuration section. This is also the main URL to access your Innovation Sandbox on AWS solution.
