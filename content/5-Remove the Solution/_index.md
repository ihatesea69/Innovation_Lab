+++
title = "5-Remove the Solution"
date = 2025
weight = 5
chapter = false
pre = "<b>5. </b>"
+++

### Complete Removal Guide for Innovation Sandbox on AWS
If you have deployed Innovation Sandbox on AWS in a testing environment and need to completely remove it, follow the steps below. Estimated time: about 15 minutes.

#### 1. Delete First Two CloudFormation Stacks (On Hub Account)
Log in to hub account and ensure you are in the correct home Region.

Open AWS CloudFormation console.

Choose compute stack (example: isb-compute), click Delete.

Wait for deletion process to complete.

Continue to choose data stack (example: isb-data), click Delete.

Wait for deletion process to complete.

#### 2. Delete IAM Identity Center Application Configuration (On Organization Management Account)
Log in to organization management account and ensure you are in the correct home Region.

Open IAM Identity Center console.

In navigation bar, choose Applications.

Switch to Customer managed tab.

Choose Innovation Sandbox on AWS application.

Go to Actions > Remove.

Confirm by entering application name to complete deletion.

#### 3. Delete Remaining CloudFormation Stacks (On Organization Management Account)
Open AWS CloudFormation console.

Choose IDC stack (example: isb-idc), click Delete.

Wait for deletion process to complete.

Choose AccountPool stack (example: isb-accountpool), click Delete.

Wait for deletion process to complete.

#### 4. Close AWS Sandbox Accounts (Optional)
Note: Before closing AWS accounts, carefully read the process, prerequisites and impacts of account closure in the AWS account management guidance documentation.

Open AWS Organizations console in organization management account.

Access organizational unit for Innovation Sandbox on AWS.

Open each sub-organizational unit to find sandbox accounts.

For each account to close:

Choose the account.

Go to Actions > Close.

Confirm by entering AWS account ID.

#### Complete!
You have completed the complete removal of the Innovation Sandbox on AWS solution from your testing environment.
