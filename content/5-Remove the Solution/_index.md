+++
title = "Remove the Solution"
date = 2020
weight = 5
chapter = false
pre = "<b>5. </b>"
+++

### Clean up when you're done

If you've finished testing and want to clean everything up, follow these steps. It should take about 15 minutes.

#### 1. Delete the first two stacks (in the hub account)

Log into the hub account, make sure you're in the right home Region.

Open the AWS CloudFormation console.

Select the compute stack (e.g., isb-compute), click Delete.

Wait for it to finish deleting.

Next, select the data stack (e.g., isb-data), click Delete.

Wait for it to finish deleting.

#### 2. Delete the IAM Identity Center App configuration (in the management account)

Log into the management account, make sure you're in the right home Region.

Open the IAM Identity Center console.

In the navigation bar, select Applications.

Switch to the Customer managed tab.

Select the Innovation Sandbox on AWS app.

Go to Actions > Remove.

Confirm by entering the app name to complete.

#### 3. Delete the remaining two stacks (in the management account)

Open the AWS CloudFormation console.

Select the IDC stack (e.g., isb-idc), click Delete.

Wait for it to finish deleting.

Select the AccountPool stack (e.g., isb-accountpool), click Delete.

Wait for it to finish deleting.

#### 4. Close the AWS Sandbox accounts (optional)

Note: Before closing an AWS account, carefully read the instructions and impacts of closing an account in the AWS documentation.

Open the AWS Organizations console in the management account.

Access the Innovation Sandbox on AWS organizational unit.

Open each sub-organizational unit to find the sandbox accounts.

For each account you want to close:

Select the account.

Go to Actions > Close.

Confirm by entering the AWS account ID.

#### Done!

You've now cleaned up the entire Innovation Sandbox on AWS from your test environment.
