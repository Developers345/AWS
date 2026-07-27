# 2. Subnet

A **Virtual Private Cloud (VPC)** is a large, isolated network that is scoped to an AWS Region. By default, all resources within the same VPC can communicate with each other.

However, in real-world applications, not all resources should have the same level of accessibility. Some resources need to be **publicly accessible**, while others should remain **private** for security reasons.

Managing all resources within a single large VPC makes it difficult to enforce different access controls. To solve this problem, AWS provides **Subnets**.

---

## What is a Subnet?

A **Subnet** is a smaller network created within a VPC.

A VPC can be divided into multiple subnets, allowing you to:

- Organize resources into smaller network segments.
- Control network traffic more effectively.
- Separate public and private resources.
- Improve security and network management.

Resources are distributed across these subnets, and traffic restrictions can be enforced at the subnet level.

---

## Features of a Subnet

1. **Subnets are created within a VPC and belong to a specific Availability Zone (AZ).**

2. **A single VPC can have up to 200 subnets by default.**

3. **It is recommended to create at least two subnets in different Availability Zones.**

   This helps distribute resources across multiple Availability Zones, improving:

   - High Availability (HA)
   - Fault Tolerance
   - Reliability

---

# Purpose of a Subnet

Subnets are mainly used for the following purposes:

## 1. Apply Traffic Restrictions

Subnets allow you to control network traffic for groups of resources.

For example, you can create:

- **Public Subnet** – Contains resources that should be accessible from the Internet (such as Web Servers).
- **Private Subnet** – Contains resources that should not be directly accessible from the Internet (such as Databases and Application Servers).

This separation improves the overall security of your AWS infrastructure.

---

## 2. Distribute Resources Across Availability Zones

Subnets help distribute resources across multiple Availability Zones within a Region.

By placing resources in different Availability Zones, applications become more resilient to failures.

For example:

- Web Server → Availability Zone A
- Application Server → Availability Zone B
- Database → Availability Zone B (Private Subnet)

If one Availability Zone becomes unavailable, resources in another Availability Zone can continue serving the application, ensuring **High Availability (HA)**.

---

# CIDR Notation

When creating a **VPC** or a **Subnet**, you must specify a **CIDR (Classless Inter-Domain Routing)** block.

A CIDR block defines the range of IP addresses that can be assigned to resources within the VPC or Subnet.

### Example

| Resource | Example CIDR Block |
|----------|--------------------|
| VPC | `10.0.0.0/16` |
| Public Subnet | `10.0.1.0/24` |
| Private Subnet | `10.0.2.0/24` |

The CIDR block determines:

- The IP address range available.
- The maximum number of resources that can be assigned IP addresses.
- The network boundaries of the VPC or Subnet.

---

# Summary

| Feature | Description |
|---------|-------------|
| **Subnet** | A smaller network created within a VPC |
| **Scope** | Availability Zone (AZ) |
| **Default Limit** | Up to 200 subnets per VPC |
| **Purpose** | Organize resources, improve security, and control network traffic |
| **High Availability** | Recommended to create at least two subnets in different Availability Zones |
| **CIDR Block** | Defines the IP address range assigned to the VPC or Subnet |


# Pictorial representation - Subnet
<img width="3568" height="4172" alt="08-SEP-2021-SUBNET" src="https://github.com/user-attachments/assets/0611011c-d1bc-489a-8d80-b015a703bb8b" />

# CIDR (Classless Inter-Domain Routing)

**CIDR (Classless Inter-Domain Routing)** is a notation used to define how many bits of an IP address are allocated for the **network** portion and how many bits are allocated for the **host** portion.

### Example

```text
192.168.12.10/23
```

In this example:

- **23 bits** are allocated for the **Network ID**.
- **9 bits** are allocated for the **Host ID**.

Since an IPv4 address consists of **32 bits**, the remaining bits after the network portion are used for host addresses.

> **Formula:**  
> **Host Bits = 32 − Network Bits**

For the above example:

```text
32 - 23 = 9 Host Bits
```

---

# CIDR Notation for a VPC

When creating a **Virtual Private Cloud (VPC)**, you must specify a CIDR block.

### Example

```text
10.0.0.0/16
```

This indicates:

- **16 bits** are allocated for the **Network ID**.
- **16 bits** are allocated for the **Host ID**.

As a result, the VPC can assign IP addresses in the following range:

```text
10.0.0.0  →  10.0.255.255
```

All AWS resources created within this VPC receive IP addresses from this range.

---

# Creating Subnets Using CIDR

To divide a VPC into smaller networks, create **Subnets** with smaller CIDR blocks.

### Subnet 1

```text
10.0.2.0/24
```

IP Address Range:

```text
10.0.2.0  →  10.0.2.255
```

All machines within this IP address range belong to **Subnet 1**.

---

### Subnet 2

```text
10.0.3.0/24
```

IP Address Range:

```text
10.0.3.0  →  10.0.3.255
```

All machines within this IP address range belong to **Subnet 2**.

---

# IP Address Allocation

AWS allocates private IP addresses to resources based on the CIDR block assigned to the **VPC** and **Subnet**.

- The **VPC CIDR** defines the overall IP address range available for the VPC.
- The **Subnet CIDR** defines a smaller range of IP addresses within the VPC.
- Resources such as **EC2 instances**, **Load Balancers**, and **RDS databases** receive IP addresses from the subnet in which they are deployed.

---

# Summary

| CIDR Block | Description | IP Address Range |
|------------|-------------|------------------|
| `10.0.0.0/16` | VPC CIDR | `10.0.0.0` – `10.0.255.255` |
| `10.0.2.0/24` | Subnet 1 | `10.0.2.0` – `10.0.2.255` |
| `10.0.3.0/24` | Subnet 2 | `10.0.3.0` – `10.0.3.255` |

> **Key Points**
>
> - CIDR stands for **Classless Inter-Domain Routing**.
> - CIDR notation determines how IP addresses are divided into **Network** and **Host** portions.
> - Every VPC must have a CIDR block.
> - Every subnet must have a CIDR block that falls within the VPC's CIDR range.
> - AWS assigns IP addresses to resources based on the configured VPC and Subnet CIDR blocks.

# Pictorial representation - CIDR
<img width="3568" height="4172" alt="09-SEP-2021-CIDR" src="https://github.com/user-attachments/assets/cfc73f93-e146-4c73-97a5-9e16b3ce093a" />

