# AWS Architecture

# 1. AWS Region

AWS Cloud platform services are offered to customers from various geographical locations known as **AWS Regions**.

An **AWS Region** is a geographical location where AWS hosts a group of data centers to provide cloud services to customers.

> **Note:** Do not treat an AWS Region as a country. A single country can have multiple AWS Regions.

### Example

The **United States** has multiple AWS Regions:

- US-East-1
- US-East-2
- US-West-1
- US-West-2

In simple terms, an **AWS Region** is a geographical location from which AWS delivers its cloud services.

Currently, AWS operates in **22 Regions** worldwide.

---

## Selecting an AWS Region

After logging in to your AWS account, you must choose an AWS Region before provisioning any AWS resources.

All resources (such as EC2 instances, RDS databases, VPCs, etc.) are created within the selected region.

---

## Why Does AWS Provide Multiple Regions?

AWS offers multiple regions to help customers optimize performance, availability, and cost.

### 1. Network Latency

If most of your customers are located in **India**, hosting your application in the **US-East-1** region will increase the network latency because requests must travel a longer distance.

Therefore, you should choose an AWS Region that is geographically closer to your users to reduce latency and improve application performance.

### Example

- Customers are in **India** → Choose **Asia Pacific (Mumbai)** region.
- Customers are in **North America** → Choose **US-East** or **US-West** region.

---

### 2. Pricing

AWS service pricing varies from one region to another.

- High-demand regions generally have higher pricing.
- Some regions offer the same services at a lower cost.

If your application is **not sensitive to network latency**, you can choose a lower-cost region to reduce infrastructure expenses.

### Pictorial Representation - AWS Region 
<img width="3568" height="4172" alt="02-SEP-2021-AWS-REGIONS" src="https://github.com/user-attachments/assets/c433dd94-2ad2-4a11-bb4b-3bc486534439" />

# 2. Availability Zone (AZ)

Within an **AWS Region**, Amazon has set up multiple **Availability Zones (AZs)**.

An **Availability Zone** is a group of one or more data centers operated by AWS to provide cloud services.

Each AWS Region contains multiple Availability Zones. AWS provides **at least two Availability Zones per Region** to support **High Availability (HA)**.

If an application is deployed across multiple Availability Zones within the same Region, and one Availability Zone fails, another Availability Zone continues serving the application. This ensures **high availability** and minimizes downtime.

---

## How AWS Chooses Availability Zones

AWS carefully selects the locations of its Availability Zones based on several factors to minimize the possibility of multiple Availability Zones failing simultaneously.

Some of these factors include:

1. Climatic conditions
2. Power availability
3. Hurricanes
4. Tornadoes
5. Other natural disaster risks

Because of these considerations, the likelihood of multiple Availability Zones within the same Region going down at the same time is extremely low.

---

## Connectivity Between Availability Zones

Availability Zones within a Region are interconnected through **AWS's dedicated high-speed private network**.

Benefits include:

- Very low (near-zero) network latency
- High-bandwidth connectivity
- Secure communication
- Seamless network traffic between Availability Zones

This enables applications to communicate efficiently while maintaining high availability and fault tolerance.

---

# 3. Edge Locations

**Edge Locations** are small AWS facilities located around the world, close to end users.

They can be considered **mini data centers** that provide AWS shared services with low latency.

Some AWS services that use Edge Locations include:

1. Amazon CloudFront
2. Amazon Route 53

---

## Why Are Edge Locations Needed?

Certain AWS services must be accessible globally with minimal latency.

### Example: Amazon Route 53

Amazon **Route 53** is AWS's DNS (Domain Name System) service.

When a user enters a domain name (for example, `www.example.com`), Route 53 resolves it to the corresponding IP address.

If the DNS service is located far away from the user, DNS resolution takes longer, increasing the overall response time and negatively impacting the user experience.

To reduce this latency, AWS serves Route 53 requests through Edge Locations that are geographically closer to users.

---

## Purpose of Edge Locations

AWS uses Edge Locations to:

- Reduce network latency
- Improve application performance
- Deliver content faster
- Provide globally distributed access to AWS shared services
- Increase availability and reliability

Edge Locations act as **replication centers** for AWS shared services, ensuring users around the world can access them quickly and efficiently.

---

## Summary

### Availability Zone (AZ)

- A group of one or more AWS data centers.
- Each Region contains multiple Availability Zones.
- Supports high availability and fault tolerance.
- Connected through AWS's high-speed private network.

### Edge Location

- Small AWS facilities located close to users.
- Used by global AWS services such as:
  - Amazon CloudFront
  - Amazon Route 53
- Helps reduce latency and improve the end-user experience.
- Acts as a replication center for AWS shared services.


### Pictorial Representation - Availability Zones
<img width="3568" height="4172" alt="03-SEP-2021-AZ" src="https://github.com/user-attachments/assets/70c836fc-bf4a-42ef-99c5-f953e6a7c97e" />

### Pictorial Representation - Local Zones
<img width="3568" height="4172" alt="03-SEP-2021-EDGE-LOCATIONS" src="https://github.com/user-attachments/assets/1f7a9663-66df-47ec-9633-5e416a2c93fe" />

# Scope of AWS Services

AWS services are broadly classified into **three scopes** based on their availability and accessibility:

1. Global
2. Regional
3. Availability Zone (AZ)

---

# 1. Global Services

Global services are available across the entire AWS infrastructure. Any changes made to these services are reflected across all AWS Regions within the AWS account.

## Examples

### 1.1 Amazon Route 53

- Amazon Route 53 is AWS's DNS (Domain Name System) service.
- If you create or update a DNS record in Route 53, the changes are automatically propagated across all AWS Regions.

### 1.2 IAM (Identity and Access Management)

- IAM is used to create and manage AWS users, groups, roles, and permissions.
- Changes made to IAM resources are available across all AWS Regions because IAM is a global service.

### 1.3 Amazon CloudFront

- Amazon CloudFront is AWS's Content Delivery Network (CDN).
- It is used to distribute static content such as:
  - CSS files
  - JavaScript files
  - Images
  - Videos
  - Documents
- When static content is published, it is automatically replicated across AWS Edge Locations worldwide for faster delivery.

---

# 2. Regional Services

Regional services are available only within the AWS Region where they are created.

Resources created in one Region cannot be accessed directly from another Region unless explicitly configured.

## Examples

### 2.1 Amazon DynamoDB

- A fully managed NoSQL database service.
- Tables are automatically replicated across multiple Availability Zones within the same Region to provide High Availability (HA).

### 2.2 Amazon S3 (Simple Storage Service)

- Objects stored in an S3 bucket remain within the selected AWS Region.
- The bucket and its data are region-specific.

### 2.3 Elastic Load Balancer (ELB)

- Distributes incoming application traffic across multiple EC2 instances.
- Operates only within the Region where it is created.

### 2.4 Amazon Virtual Private Cloud (VPC)

- A logically isolated virtual network within an AWS Region.
- Resources inside the VPC belong to that specific Region.

### Example

Suppose you create an **Elastic Load Balancer (ELB)** in the **ap-south-1 (Mumbai)** Region.

- It can distribute traffic only to EC2 instances that exist within the same Region and VPC.
- It cannot directly load balance EC2 instances in another AWS Region.

---

# 3. Availability Zone (AZ) Services

Availability Zone (AZ) services are limited to a specific Availability Zone within an AWS Region.

These resources physically exist only in the Availability Zone where they are created.

## Examples

### 3.1 Amazon Elastic Block Store (EBS)

- Provides persistent block storage for EC2 instances.
- An EBS volume belongs to a single Availability Zone.

### 3.2 Amazon EC2 (Elastic Compute Cloud)

- An EC2 instance is launched in a specific Availability Zone within a Region.
- It physically resides in that Availability Zone.

### 3.3 Subnet

- A subnet is created within a single Availability Zone.
- Resources deployed in the subnet belong to that Availability Zone.

### Example

When launching an **Amazon EC2** instance, AWS asks you to select:

- An AWS Region
- An Availability Zone

After the instance is created:

- The EC2 instance is provisioned in the selected Availability Zone.
- The associated **Amazon EBS** volume is also created in the **same Availability Zone**.

Since EBS volumes are AZ-specific, they can only be directly attached to EC2 instances within the same Availability Zone.

---

# Summary

| Scope | Description | Example Services |
|--------|-------------|------------------|
| **Global** | Available across all AWS Regions. Changes are reflected throughout the AWS account. | Amazon Route 53, IAM, Amazon CloudFront |
| **Regional** | Available only within the Region where they are created. | Amazon S3, Amazon DynamoDB, Elastic Load Balancer (ELB), Amazon VPC |
| **Availability Zone (AZ)** | Resources exist only within a specific Availability Zone. | Amazon EC2, Amazon EBS, Subnet |
```
