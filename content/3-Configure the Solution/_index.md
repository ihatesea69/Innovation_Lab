+++
title = "Configure the Solution"
date = 2025
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

### Configure the System

Okay, the stacks are done running, but you can't use it yet. You need to configure a bit more before users can get in.

#### What needs to be done

- Set up SAML for login
- Create users and groups in IAM Identity Center
- Configure the web app
- Move the sandbox accounts into the right place

#### Setting up SAML

**1. Configure SAML**

- Go to AWS IAM Identity Center in the management account
- Select "Settings" then switch to "Identity Source"
- Choose "External identity provider" to use SAML 2.0

**2. Download the certificate**

- In the SAML interface, download the AWS IAM Identity Center certificate file (.pem)
- Save this file for later use

#### Creating users and groups

**1. Create groups**

- In IAM Identity Center, select "Groups"
- Create groups based on roles (Admin, Developer, Viewer...)
- Assign permissions to each group

**2. Add users**

- Go to "Users" in IAM Identity Center
- Add new users or sync from an external system (if SAML is configured)
- Add users to the appropriate groups

#### Configuring the web app

**1. Update AWS AppConfig**

- Go to AWS AppConfig in the hub account
- Select the Innovation Sandbox app, update the necessary parameters
- Check if the information matches the environment

**2. Configure authentication**

- Ensure the web app uses IAM Identity Center for login
- Check the SAML connection information

#### Onboarding accounts

**1. Move accounts**

- Go to AWS Organizations using the management account
- Move the accounts (hub, sandbox...) into the correct designed OU

**2. Double-check**

- Check in AWS Organizations to see if the accounts are in the right place
- Ensure that policies and permissions have been applied correctly

**Time:** About 15 minutes

Completing these steps will make the system ready for users.

### Setting up SAML for Innovation Sandbox

This part is a bit technical, but it's necessary for users to log into the system securely.

#### Important notes

- Do everything in the **organization management account** - where IAM Identity Center is
- Make sure you are in the correct selected **home Region**

#### Steps

**1. Create a SAML app in IAM Identity Center**

- Open the **AWS IAM Identity Center console** with the management account
- In the navigation bar, select **Applications**
- Switch to the **Customer managed** tab

![architect](/images/organiza.jpg "Architect")

**2. Add a new app**

- Select **Add application**
- Select **I have an application I want to set up**
- Select **SAML 2.0**, then **Next**

![architect](/images/Buoc3.png "Architect")

**3. Configure the app**

- On the configuration page, enter a **Display name** (e.g., MyFCJ app) and description

![architect](/images/Buoc5.png "Architect")

- In the **IAM Identity Center metadata** section:
  - Download the **SAML metadata file** 
  - Download the **certificate**

- Note the **Sign-in URL** and **Sign-out URL** - needed for configuration in the hub account

![architect](/images/Buoc7.png "Architect")

**4. Enter metadata information**

- In the **Application metadata** section, select **Manually type your metadata values**
- Enter the values:

  | Field                   | What to enter                                                                                          |
  |-------------------------|--------------------------------------------------------------------------------------------------|
  | Application ACS URL     | `{FCJ_WEB_URL}/api/auth/login/callback` (Replace `{FCJ_WEB_URL}` with the saved CloudFrontDistributionUrl) |
  | Application SAML audience | App identifier name (can use namespace, e.g., `Isb-dev-namespace-Audience`). Save for AppConfig configuration |

![architect](/images/Buoc8.png "Architect")

**5. Finish**

- Click **Submit**
- A success message will appear
- Remember to configure **attribute mappings** for IAM Identity Center

#### Information to save

- **Sign-in URL** 
- **Sign-out URL** 
- **Audience value** 

This information is used to configure the web app and user authentication.

### Configure Users and Groups

Set up users and groups to control who can access and do what in the system. Do this in the **management account** and use the **home Region**.

#### 1. Map app attributes

- Open the **IAM Identity Center console**
- Select **Applications** 
- Select the newly created app (MyFCJ app)
- On the app details page, select **Actions** > **Edit attribute mappings**
- In the second box, enter: `${user:email}`
- In the third column, select **emailAddress**
- **Save changes**

#### 2. Assign groups to the app

- Go back to the configured app
- Select **Assign users and groups**
- Find and add the three groups created by the IDC stack:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- Select **Assign**

#### 3. Add users to groups

- In the IAM Identity Center console, select **Users**
- Select the user to assign to a group (or **Add user** to add a new one)
- In the **Groups** tab, click **Add user to groups**
- Select one of the three groups:
  - `{NAMESPACE}_IsbUsersGroup`
  - `{NAMESPACE}_IsbManagersGroup`
  - `{NAMESPACE}_IsbAdminsGroup`
- **Add user to 1 group**

![architect](/images/users-and-groups.png "Architect")

**Note:** To test all three roles, you need at least three users, each in a different group.

### Configure the Web App

This section enables login and sets parameters for the app. Do this in the **hub account** and use the **home Region**.

#### 1. Configure AWS AppConfig

- Open the **AWS AppConfig console** in the hub account
- In the navigation bar, select **Applications**
- Select **InnovationSandboxData-Config-Application-XXXXXXX**
- Select **InnovationSandboxData-Config-GlobalConfigHostedConfiguration-XXXXX**
- **Create** to create a new configuration version

![architect](/images/appconfig.png "Architect")

**Update configuration:**
- Set `maintenanceMode` to `false` to open the app for users

**In the `auth` section, update:**

| Field              | What to update                                                                                       |
|---------------------|---------------------------------------------------------------------------------------------------|
| idpSignInUrl        | Sign-in URL from the SAML configuration step                                                              |
| idpSignOutUrl       | Sign-out URL from the SAML configuration step                                                              |
| idpAudience         | Audience ID from the SAML configuration step                                                                |
| webAppUrl           | CloudFront Distribution URL from the CloudFormation Outputs tab (hub account)                       |
| awsAccessPortalUrl  | AWS Access Portal URL from the IAM Identity Center dashboard (management account)                      |
| sessionDurationInMinutes | Session duration, e.g., 240                                                               |
| emailFrom           | Your verified email address                                                                         |

- **Create hosted configuration version**
- **Start Deployment** and select the newly created version
- **Start Deployment** to apply

#### 2. Update certificate in Secrets Manager

- Open the **AWS Secrets Manager console** in the hub account
- Find the secret: `/InnovationSandbox//Auth/IDPCert`
- In the **Overview** tab, **Secret value** section:
  - **Retrieve secret value**
  - **Edit**
  - **Plaintext**
  - **Edit** again
  - Paste the content of the downloaded IAM Identity Center certificate file (.pem)
  - **Save**

**Note:** Ensure all information is accurate and consistent with the previous steps.

### Onboard Sandbox Accounts

The final step to complete the configuration. Move AWS accounts into the correct structure and apply controls.

#### Important notes

- Do this in the **organization management account**
- In the correct selected **home Region**

#### Steps

- Log in to the AWS Management Console with the management account
- Open the **AWS Organizations console**

**2. Find accounts to move**

- Select **AWS accounts**
- Find the accounts you want to move to the sandbox environment
- **Note:** Do not select the management or hub account

![architect](/images/account-onboarding.png "Architect")

**3. Move to Entry OU**

- Select the accounts to move
- In the **Actions** menu, select **Move** 
- In the **Move AWS account** dialog, click the arrow next to the Innovation Sandbox OU to expand it
- Select **Entry OU**
- **Move AWS accounts** to confirm

#### Result

- The sandbox accounts are now in the correct location in the Innovation Sandbox structure
- Control policies will be automatically applied to these accounts

Doing this correctly ensures the sandbox environment operates separately, securely, and in compliance with organizational policies.
