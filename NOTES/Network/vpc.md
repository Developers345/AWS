# Networking Domain

The **Networking Domain** in AWS provides network-related resources such as:

- Routers
- Firewalls
- Gateways
- Virtual networks
- Load balancers
- Other networking services

These services help users securely connect and manage AWS resources.

---

# 1.1 Virtual Private Cloud (VPC)

A **Virtual Private Cloud (VPC)** is a logically isolated virtual network within AWS that allows you to launch and manage AWS resources securely.

Resources inside one VPC are isolated from resources in another VPC.

> **In Short:** A **VPC** is an isolated network environment within AWS.

---

## Features of a VPC

1. **VPCs are created at the AWS account level and are Region-specific.**

   - Each AWS account can create separate VPCs in each AWS Region.

2. **A VPC spans all Availability Zones within a Region.**

   - Resources can be deployed into any Availability Zone that belongs to the Region.

3. **AWS allows up to 5 VPCs per AWS account per Region by default (soft limit).**

   - This limit can be increased by requesting a service quota increase.

4. **Resources within the same VPC can communicate with each other.**

   - Communication occurs through the private network unless restricted by security rules.

5. **Resources in different VPCs cannot communicate by default.**

   - Even if the VPCs belong to the same AWS account, communication is blocked unless configured.
   - To enable communication between VPCs, **VPC Peering** must be configured.

6. **AWS automatically creates one Default VPC in every Region.**

   - The default VPC is created when an AWS account is initialized for a Region.
   - It allows users to launch resources without creating a custom VPC.

---

# Why Do We Need a VPC?

A VPC helps isolate resources for security, organization, and better network management.

It enables organizations to separate applications, environments, and business units while using a single AWS account.

---

# Use Case 1: Isolating Business Units

An enterprise may have multiple business units, such as:

- Finance
- Marketing
- Telecom
- Sales

Each business unit may have its own applications and infrastructure.

### Option 1: Multiple AWS Accounts

One approach is to create a separate AWS account for each business unit.

**Disadvantages:**

- Difficult to manage multiple AWS accounts.
- Billing becomes more complex.
- Reporting across accounts is challenging.
- Administrative overhead increases.

### Better Solution: Separate VPCs

Instead of creating multiple AWS accounts, create one VPC for each business unit.

Example:

- Finance VPC
- Marketing VPC
- Telecom VPC
- Sales VPC

This approach keeps each department's resources isolated while allowing centralized account management.

---

# Use Case 2: Isolating Project Environments

VPCs can also be used to separate different environments of the same project.

For example:

- Development VPC
- QA (Testing) VPC
- Staging VPC
- Production VPC

Each environment contains its own AWS resources and is isolated from the others.

### Example

A **QA VPC** can host:

- EC2 instances
- Databases
- Load Balancers
- Storage
- Other testing resources

Similarly, a **Production VPC** can host live application resources independently of the QA environment.

This isolation improves:

- Security
- Stability
- Resource management
- Operational efficiency

---

# Summary

| Feature | Description |
|---------|-------------|
| **VPC** | A logically isolated virtual network within AWS |
| **Scope** | Region-specific |
| **Availability Zones** | A VPC spans all Availability Zones in a Region |
| **Default Limit** | 5 VPCs per Region per AWS account (soft limit) |
| **Communication** | Resources within the same VPC can communicate |
| **Between VPCs** | Communication requires **VPC Peering** |
| **Default VPC** | AWS automatically creates one Default VPC per Region |
| **Common Use Cases** | Isolating business units and project environments (Development, QA, Staging, Production) |


# Pictorial representation - VPC 
<img width="3568" height="4172" alt="06-SEP-2021-VPC" src="https://github.com/user-attachments/assets/aba7e1e2-9e17-42d5-a191-c3d5fdbe58df" />
