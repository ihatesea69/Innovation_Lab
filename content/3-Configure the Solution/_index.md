+++
title = "Configure the Solution"
date = 2025
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

### Configure the Solution

After running all the stacks, you need to perform configuration steps to allow users to access and prepare the Innovation Sandbox on AWS environment for use.

#### Overview of configuration steps

- Set up SAML 2.0 federation
- Configure AWS IAM Identity Center users
- Configure web application
- Onboard accounts into Innovation Sandbox structure

#### Setting up SAML 2.0 federation

**1. Configure SAML integration**

- Access AWS IAM Identity Center in the organization management account.
- Choose "Settings" and switch to "Identity Source" section.
- Choose "External identity provider" to use SAML 2.0.

**2. Download certification**

- In the SAML configuration interface, download the certificate (.pem file) from AWS IAM Identity Center.
- Save this file for use when configuring on external identity systems

#### Configure AWS IAM Identity Center users

**1. Create and configure groups**

- In IAM Identity Center, go to "Groups".
- Create appropriate groups for usage roles (example: Admin, Developer, Viewer...).
- Assign corresponding permission sets to each group.

**2. Set up initial users**

- Go to "Users" in IAM Identity Center.
- Add new users or sync from external identity systems (if SAML is configured).
- Assign users to created groups.

#### Configure web application

**1. Update AWS AppConfig settings**

- Access AWS AppConfig in hub account.
- Choose Innovation Sandbox application, update necessary configuration parameters (example: endpoint, environment parameters, etc.).
- Confirm configuration information matches deployment environment.

**2. Configure authentication**

- Ensure web application uses authentication through AWS IAM Identity Center.
- Check SAML connection information or other authentication parameters if available.

#### Onboard accounts

**1. Move accounts into Innovation Sandbox structure**

- Access AWS Organizations with management account.
- Move member accounts (hub, sandbox...) into correct organizational unit (OU) designed for Innovation Sandbox.

**2. Verify account configuration**

- Check again in AWS Organizations to ensure accounts are in correct position in OU structure.
- Ensure related policies, access permissions and configurations are applied correctly.

**Estimated time:** About 15 minutes

Completing all the above steps will help the Innovation Sandbox on AWS solution be ready for user access and use, while ensuring compliance with organizational governance and security requirements.

### Setting up SAML 2.0 Application for Innovation Sandbox on AWS

Configuring SAML 2.0 integration between AWS IAM Identity Center and Innovation Sandbox on AWS helps ensure secure user authentication when accessing the solution's web interface.

#### Important note

- Perform all steps below in the **organization management account** – where you have configured AWS IAM Identity Center.
- Ensure you are in the correct **home Region** chosen for the entire system.

#### Configuration steps

**1. Create SAML application in IAM Identity Center**

- Open **AWS IAM Identity Center console** with organization management account.
- In navigation bar, choose **Applications**.
- Switch to **Customer managed** tab.
![architect](/images/organiza.jpg "Architect")

**2. Add new application**

- Choose **Add application**.
- Choose **I have an application I want to set up**.
- Choose **SAML 2.0**, then click **Next**.

![architect](/images/Buoc3.png "Architect")

**3. Configure application**

- On configuration page, enter **Display name** (example: MyISB app) and description for application.
- 
![architect](/images/Buoc5.png "Architect")

- In **IAM Identity Center metadata** section:
  - Download **SAML metadata file** (identity provider metadata).
  - Download **certificate** (identity provider certificate).

![architect](/images/Buoc6.png "Architect")


- Note the **Sign-in URL** and **Sign-out URL** – will be needed when configuring on hub account.

![architect](/images/Buoc7.png "Architect")

**4. Enter metadata information for application**

- In **Application metadata** section, choose **Manually type your metadata values**.
- Enter the following values:

  | Field                   | Value to enter                                                                                        |
  |-------------------------|-------------------------------------------------------------------------------------------------------|
  | Application ACS URL     | `{ISB_WEB_URL}/api/auth/login/callback`  (Replace `{ISB_WEB_URL}` with saved CloudFrontDistributionUrl, example: `https://duyXXXXXXXeh.cloudfront.net/api/auth/login/callback`) |
  | Application SAML audience | Application identifier value (can use namespace name or initial stack name, example: `Isb-dev-namespace-Audience`). Save this value to configure AppConfig on hub account. |
![architect](/images/Buoc8.png "Architect")
**5. Complete configuration**

- Click **Submit**.
- You will receive confirmation message for successful SAML application configuration.
- Note: You need to additionally configure **attribute mappings** for IAM Identity Center for authentication to work properly.

#### Information to save for subsequent configuration steps

- **Sign-in URL** (from metadata download step)
- **Sign-out URL** (from metadata download step)
- **Audience value** (value entered in Application SAML audience)

This information will be used in the process of configuring web application and user authentication for Innovation Sandbox on AWS.

### Configure Users and Groups in IAM Identity Center

Setting up users and groups in AWS IAM Identity Center helps control access and permissions for users using the Innovation Sandbox on AWS solution. Perform the steps below in the **organization management account** and correct **home Region** chosen.

#### 1. Map application attributes

- Open **IAM Identity Center console** in management account.
- Choose **Applications** in navigation bar.
- In **Customer managed applications** list, choose the created application (example: MyISB app).
- On application detail page, choose **Actions** > **Edit attribute mappings**.
- In the second box ("Maps to this string value or user attribute in IAM Identity Center"), enter:  
  ```
  ${user:email}
  ```
- In third column, choose **emailAddress** format.
- Click **Save changes** to save configuration.

#### 2. Assign groups to application

- Return to the just configured application.
- Choose **Assign users and groups**.
- Find and add all three groups created by IDC stack:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- Choose **Assign** to complete.

#### 3. Assign group to user

- In IAM Identity Center console, choose **Users** in navigation bar.
- In user list, choose user to assign to group. To add new user, choose **Add user**.
- Choose **Groups** tab, then click **Add user to groups**.
- Choose one of three created groups:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- Click **Add user to 1 group** to complete.

![architect](/images/users-and-groups.png "Architect")

**Note:**  
To test all three user roles (user personas), ensure you have at least three users, each belonging to a different group.

After completing the above steps, you have successfully configured users and groups for the Innovation Sandbox on AWS solution. Next, you will proceed to configure the web application on the hub account.

### Configure Web Application with AWS AppConfig and AWS Secrets Manager

Configuring the Innovation Sandbox on AWS web application helps activate user authentication and set global parameters for the application. These operations need to be performed in the **hub account** and correct **home Region** chosen.

#### 1. Configure AWS AppConfig

- **Open AWS AppConfig console** in hub account.
- In navigation bar, choose **Applications**.
- Choose application **InnovationSandboxData-Config-Application-XXXXXXX**.
- Choose **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-XXXXX** (configuration profile).
- Choose **Create** to create a new hosted configuration version.


![architect](/images/appconfig.png "Architect")

**Update configuration:**
- Set `maintenanceMode` value to `false` to open application for users.

**In `auth` section, update the following fields:**

| Field               | Update value                                                                                          |
|---------------------|-------------------------------------------------------------------------------------------------------|
| idpSignInUrl        | Sign-in URL from SAML 2.0 Application configuration step                                            |
| idpSignOutUrl       | Sign-out URL from SAML 2.0 Application configuration step                                           |
| idpAudience         | Audience ID from SAML 2.0 Application configuration step                                            |
| webAppUrl           | CloudFront Distribution URL (Innovation Sandbox URL) from CloudFormation Outputs tab (hub account) |
| awsAccessPortalUrl  | AWS Access Portal URL from IAM Identity Center dashboard (organization management account)          |
| sessionDurationInMinutes | Login session duration, example: 240                                                           |
| emailFrom           | Your verified login email address                                                                     |

- After updating, choose **Create hosted configuration version**.
- Choose **Start Deployment** and choose the just created configuration version.
- Choose **Start Deployment** to apply new configuration.

#### 2. Update certificate value in AWS Secrets Manager

- Open **AWS Secrets Manager console** in hub account.
- Find and choose secret:  
  `/InnovationSandbox//Auth/IDPCert`
- In **Overview** tab, **Secret value** section:
  - Choose **Retrieve secret value**.
  - Choose **Edit**.
  - Choose **Plaintext**.
  - Choose **Edit** again.
  - Paste the IAM Identity Center certificate file (.pem) content downloaded when creating SAML 2.0 application.
  - Choose **Save** to save new value.

**Note:**  
- Ensure all values are accurate and consistent with information obtained in previous configuration steps (SAML, CloudFront, IAM Identity Center).
- Proper configuration helps the system authenticate and authorize users effectively, while ensuring the Innovation Sandbox on AWS web application operates stably and securely.

### Onboard Sandbox Accounts into Innovation Sandbox on AWS

Onboarding sandbox accounts is the final configuration step to complete the Innovation Sandbox on AWS solution. This step helps move AWS accounts into the correct organizational structure and apply necessary controls.

#### Important note

- Perform these steps in the **organization management account** – where you have configured AWS IAM Identity Center.
- Ensure you are in the correct **home Region** chosen for the entire system.

#### Steps to Onboard Sandbox Accounts

- Log in to AWS Management Console with organization management account.
- Open **AWS Organizations console**.


**2. Find and select accounts to move**

- Choose **AWS accounts**.
- Find the accounts you want to move into sandbox environment. You can browse by OU structure or choose **List** to see flat account list.
- **Note:** Do not select organization management account or hub account.
  
![architect](/images/account-onboarding.png "Architect")

**3. Move accounts to Entry OU**

- Select the accounts to move.
- In **Actions** menu (or corresponding action menu), choose **Move** under AWS account section.
- In **Move AWS account** dialog, click the arrow next to Innovation Sandbox on AWS OU (this OU was created when deploying AccountPool stack) to expand and view child OUs.
- Choose **Entry OU** (child OU for sandbox accounts).
- Click **Move AWS accounts** to confirm the move.

#### Result

- Sandbox accounts have been moved to the correct OU in the Innovation Sandbox on AWS organization structure.
- Related control policies and configurations will automatically be applied to these accounts.

Properly onboarding accounts into Entry OU ensures the sandbox environment operates separately, securely and complies with organization governance policies.
