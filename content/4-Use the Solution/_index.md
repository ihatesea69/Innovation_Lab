+++
title = "Use the Solution"
date = 2025
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

### Using Innovation Sandbox on AWS

After fully deploying all stacks and configuring users, you can start using the Innovation Sandbox on AWS solution. Below is a detailed guide on how to use it with the **administrator** role.

#### Overview

The solution supports three main usage roles:

- **Administrator**
- **Manager**
- **End user**

Estimated practice time: about 30 minutes.

### Using with Administrator role

#### 1. Access Innovation Sandbox on AWS

- Get the solution access URL (CloudFrontDistributionUrl saved from deployment step).
- Open this URL in browser.
- Log in with administrator account. If first time logging in, choose **Forgot password** to set up password.

**Note:**  
If the interface has two tabs **Accounts** and **Applications**, choose **Applications** to enter the main application. The welcome page will appear.

![architect](/resources/_gen/images/Anha1.png "Architect")

#### 2. Add accounts for lease

- In left navigation bar, under **Administration**, choose **Accounts**.
- Click **Add accounts**.
- In **Select accounts**, you will see list of available accounts. If there are no accounts, ensure sandbox accounts have been moved to **Entry** OU in organizational unit with name like `your-namespace_InnovationSandboxAccountPool`.
- Select one or more accounts to add, click **Next** then **Submit**.

![architect](/resources/_gen/images/Anha2.png "Architect")

- Return to **Accounts** to check status of added accounts.

#### 3. Manage system settings

- In navigation bar, under **Administration**, choose **Settings**.
- There are three main tabs:
  - **General Settings**
  - **Lease Settings** (default maximum budget limit is $100 USD)
  - **Clean Up Settings**
- To change these settings, you need to use **AWS AppConfig** (will be guided in next section).

![architect](/resources/_gen/images/Anha3.png "Architect")

#### 4. Create Service Control Policy (SCP) to limit resources

Example: Create SCP to prevent sandbox users from launching `m5.large` EC2 instances.

**Implementation steps:**

- Log in to **organization management account**.
- Open **AWS Organizations**.
- In navigation bar, choose **Policies**.
- Under **Supported policy types**, choose **Service control policies**.

![architect](/resources/_gen/images/Anha4.png "Architect")

- Click **Create policy** and enter:
  - **Policy name:** RestrictEC2Instances
  - **Policy description:** This SCP restricts launching certain EC2 instance types.
- Paste the following JSON into policy editor:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "StringEquals": {
          "ec2:InstanceType": [
            "m5.large"
          ]
        }
      }
    }
  ]
}
```

- Click **Create policy** to save.
- Return to **Policies**, choose the just created policy.
- Choose **Targets** tab, click **Attach**.
- Choose OU named `your-namespace_InnovationSandboxAccountPool` and click **Attach policy**.

![architect](/resources/_gen/images/Anha6.png "Architect")

**Result:**  
Users in sandbox accounts will not be able to launch `m5.large` EC2 instances.

#### Next steps

- You can continue to learn how to change system settings through **AWS AppConfig** and how to use the solution with **manager** and **end user** roles in subsequent steps.

**Note:**  
- Administrative operations need to be performed in correct account and Region as configured.
- Ensure sandbox accounts have been onboarded to correct OU to appear in lease list.

------------------------
### Using Innovation Sandbox on AWS with Manager role

The Innovation Sandbox on AWS solution supports three roles: **administrator**, **manager**, and **end user**. This section provides detailed guidance on using the solution with the **manager** role.

#### Overview

In this section, you will:

- Edit Innovation Sandbox on AWS settings using AWS AppConfig.
- Create basic lease template.
- Create advanced lease template.

### Editing settings with AWS AppConfig

**Important note:**  
Perform these steps in the **hub account**, not the organization management account. Ensure you are in the correct **home Region** of this account.

**Steps to edit settings:**

- Log in to hub account.
- In console search bar, enter and choose **AWS AppConfig**.
- In navigation bar, under AWS AppConfig, choose **Applications**.
- Choose the path to application created for you.
- Under **Configuration Profiles and Feature Flags**, choose the path named **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-ID**.



![architect](/resources/_gen/images/Manager1.png "Architect")

- In **Hosted configuration versions**, choose **Create**.
- Change **maxBudget** field to **50** (USD).

![architect](/resources/_gen/images/Manager2.png "Architect")

- Choose **Create hosted configuration version**.
- Choose **Start deployment**.
- Choose the just created configuration version, keep other default settings and choose **Start deployment**.

### Creating Lease Template

#### Introduction to Lease Template

Lease template is a set of parameters and rules defining how users access and use resources in Innovation Sandbox on AWS environment. This is a template for creating individual leases (controlled access sessions to cloud resources). Main components of lease template include:

- **Resource allocation:** Define maximum budget that can be used during lease period.
- **Duration:** Determine maximum time a lease can be active.
- **Approval process:** Determine if manager approval is needed before granting lease.
- **Alerts and thresholds:** Set up alerts based on budget consumption or remaining time.

You will create two types of templates:

- **Basic template:** Maximum budget $25, automatic approval, suitable for small projects and routine testing.
- **Advanced template:** Maximum budget $50, requires manager approval, suitable for large projects needing stricter control.

### Creating Basic Lease Template

This guide helps you create lease template for accounts that don't need manager approval. This template allows maximum budget $25, alert at $15 consumption, duration 168 hours (1 week), alert 24 hours before expiration.

**Implementation steps:**

- On Innovation Sandbox on AWS console, choose user profile in top right corner and choose **Sign out**.
- Log in with manager account. If first time logging in, use **Forgot password** function.
- If screen shows two tabs **Accounts** and **Applications**, choose **Applications** to enter application. You will see Innovation Sandbox welcome page.
- In navigation bar, choose **Lease Templates**.
- Choose **Add new lease template**.
- In **Name** field, enter: `Basic`.
- In **Description** field, enter: `Basic account lease template`.
- Uncheck **Approval required** box (users don't need manager approval when requesting this type of account).

![architect](/resources/_gen/images/Manager3.png "Architect")

- Choose **Next**.
- In **Maximum budget** field, enter: `100`, choose **Next**.
- If you edited AppConfig correctly, you will see error message about maximum budget being $50. Change **Maximum Budget Amount** field to `25`.
- Under **Budget Thresholds**, choose **Add a threshold**.
  - In **amount consumed**, enter `15`.
  - In **action**, choose **Send Alert**. User will receive alert when consuming $15, and account will be deleted when consuming all $25.
- Choose **Next**.
- In **Maximum Duration**, choose **Set a maximum duration** and enter `168`.
- Under **Duration Thresholds**, choose **Add a threshold**.
  - In **remaining hours**, enter `24`.
  - In **action**, choose **Send Alert**. User will receive alert when 24 hours remain in lease.
- Choose **Submit**.
- Under **Lease Templates**, you will see the just created lease template.

![architect](/resources/_gen/images/Manager4.png "Architect")


### Creating Advanced Lease Template

This section guides you to create lease template requiring manager approval. This template has maximum budget $50, alert at $40 level, duration 168 hours, alert 24 hours before expiration. Suitable for large projects needing more careful monitoring.

**Implementation steps:**

- In navigation bar, choose **Lease Templates**.
- Choose **Add new lease template**.
- In **Name** field, enter: `Advanced`.
- In **Description** field, enter: `Advanced account lease template`.
- Keep **Approval required** box checked (users need manager approval when requesting this type of account).
- Choose **Next**.
- In **Maximum budget** field, enter: `50`, choose **Next**.
- Under **Budget Thresholds**, choose **Add a threshold**.
  - In **amount consumed**, enter `40`.
  - In **action**, choose **Send Alert**. User will receive alert when consuming $40, and account will be deleted when consuming all $50.
- Choose **Next**.
- In **Maximum Duration**, choose **Set a maximum duration** and enter `168`.
- Under **Duration Thresholds**, choose **Add a threshold**.
  - In **remaining hours**, enter `24`.
  - In **action**, choose **Send Alert**. User will receive alert when 24 hours remain in lease.
- Choose **Submit**.
- Under **Lease Templates**, you will see both created lease templates.

![architect](/resources/_gen/images/Manager5.png "Architect")

### Next steps

After creating two lease templates with manager role, in the next section you will:

- Log in to Innovation Sandbox on AWS as end user.
- Request basic account:
  - Experience automatic approval process.
  - Access newly created sandbox account.
- Request advanced account:
  - Send request requiring manager approval.
  - Wait for manager review and approval.
  - Access approved sandbox account.
- Manage and monitor your sandbox accounts.

### Using Innovation Sandbox on AWS with End-User role

The Innovation Sandbox on AWS solution supports three roles: **administrator**, **manager** and **end user**. Below is detailed guidance for end users when using the system.

#### 1. Request and access basic AWS account

Basic accounts will be automatically approved, allowing you immediate access to AWS resources within defined limits.

**Implementation steps:**

- On Innovation Sandbox on AWS interface, choose user profile in top right corner and choose **Sign out**.
- Log in again with end user credentials.
- If screen has two tabs **Accounts** and **Applications**, choose **Applications** to enter application. You will see Innovation Sandbox welcome page.
- Choose **Request a new account**.
- Choose **Basic lease template**, click **Next**.
- Check **I accept the above terms of service**, click **Next**.

![architect](/resources/_gen/images/User1.png "Architect")

- In **Comments** section, enter reason (example: `Request for a basic account`).
- Choose **Submit**.
- In left navigation bar, choose **Home**. In **Account** section, click **Refresh** to update account status.

![architect](/resources/_gen/images/User2.png "Architect")

- When account has been granted, under **Access** section, choose **Login to account** for the newly added account.
- Choose the role appearing in **Select a role** section to enter AWS Management Console.

![architect](/resources/_gen/images/User3.png "Architect")

#### 2. Check Service Control Policy (SCP) limits

You can check SCP effectiveness by trying to create a restricted EC2 instance.

**Check steps:**

- In AWS Management Console, enter **EC2** in search bar and choose the service.
- In left navigation bar, choose **Instances**.
- Choose **Launch instances**.
- In **Name** field, enter `test-scp`. Keep default Amazon Machine Image (AMI).

![architect](/resources/_gen/images/User4.png "Architect")

- In **Instance type** field, choose **m5.large** (this type is blocked by SCP).
- In **Key pair**, choose **Proceed without a key pair**.
- Keep other default settings, then choose **Launch instance**.
- You will see error message due to SCP not allowing **m5.large** EC2 instance type creation.

![architect](/resources/_gen/images/User5.png "Architect")

#### 3. Request advanced account

Advanced accounts require manager approval before being granted access.

**Implementation steps:**

- Return to Innovation Sandbox on AWS interface, log in as end user.
- Choose **Request a new account**.
- Choose **Advanced lease template**, click **Next**.
- Check **I accept the above terms of service**, click **Next**.
- In **Comments** section, enter reason (example: `Request for an advanced account`).
- Choose **Submit**.

**Approval process:**

- Log out of Innovation Sandbox on AWS and log in with manager account.
- On home page, you will see pending approval request. In left navigation bar, choose **Approvals**.
- Choose the request, go to **Actions** and choose **Approve request**.

**Lease management:**

- In navigation bar, choose **Leases**.
- You will see two leases: one for basic account, one for advanced account.
- Can choose lease and use **Actions** to terminate, suspend or update lease.

![architect](/resources/_gen/images/User6.png "Architect")

![architect](/resources/_gen/images/User7.png "Architect")

#### 4. Summary of functions and user experience

- **Administrator**: Set up system, create SCP to control resources, manage accounts and configure Innovation Sandbox environment, integrate with AWS AppConfig.
- **Manager**: Configure budget thresholds, create lease templates (Basic, Advanced), set up approval processes, monitor resource usage.
- **End user**: Request and access AWS accounts, test SCP effectiveness, use different account types with appropriate approval processes.
