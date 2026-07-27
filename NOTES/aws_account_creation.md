# AWS Domains

## How many domains are there in AWS, and what are they?

There are **7 major domains** in AWS:

1. Compute
2. Storage
3. Database
4. Networking
5. Security
6. Management Tools
7. Messaging

---

# AWS Account Creation

To use the cloud services offered by **Amazon Web Services (AWS)**, you need to create an **AWS Account**. An AWS account enables the AWS Cloud platform to identify the user and provide access to AWS services.

An AWS account allows you to access AWS services using the following methods:

1. AWS Management Console
2. AWS SDK (Software Development Kit)
3. AWS CLI (Command Line Interface)
4. REST API

---

# Types of AWS Accounts

There are **2 types of AWS users/accounts**:

## 1. ROOT User

- The **ROOT User** is the AWS Account owner or administrator.
- Has complete access to all AWS services and resources.
- Can perform all administrative tasks.
- Should be used only for account-level administrative activities.

## 2. IAM User (Identity and Access Management User)

- An **IAM User** is created by the ROOT user.
- The ROOT user can delegate access to specific AWS services and resources.
- Permissions are granted using IAM policies.
- IAM users are typically created for employees or team members within an organization.

---

# Creating an AWS ROOT User Account

To start using AWS services, create a ROOT user account by visiting:

**AWS Console:**  
http://console.aws.amazon.com

---

# AWS Free Tier

When you sign up for a new AWS account, AWS provides a **12-Month Free Tier** for eligible services.

> **Important:** Not all AWS services are included in the Free Tier.

### Things to Remember

1. Some AWS services are Free Tier eligible, while others are **paid from the first day** of usage.
2. Even Free Tier services have usage limits based on:
   - Number of hours
   - Storage capacity
   - Requests
   - Data transfer

### Example

If you launch an EC2 instance:

- **Instance Type:** `t2.micro`
- **Operating System:** Ubuntu Linux AMI

It is free for **750 hours per month** (subject to Free Tier eligibility). Any usage beyond the Free Tier limits will incur charges.

Similarly, some database services are free only up to a specified storage limit.

---

# Billing Recommendation

Be careful while using AWS services.

Before creating any resource:

- Read the pricing and billing information.
- Understand the Free Tier limits.

### Standard Recommendation

After completing your work:

- **Terminate** resources that are no longer needed.
- **Stop** resources that support stopping (to reduce costs).
- Regularly monitor your AWS Billing Dashboard.

---

# ROOT User Account

The AWS **ROOT User Account** is created using your **Email Address**.

# Pictorial representation - AWS Account 
<img width="3568" height="4172" alt="01-SEP-2021-AWSACCOUNTs" src="https://github.com/user-attachments/assets/2322d945-f3b4-44c6-90bb-fbf2e4ca4904" />
