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

# Types of Subnets in AWS

There are **3 types of subnets** in AWS:

1. Private Subnet
2. Public Subnet
3. Hybrid Subnet

---

# 1. Private Subnet

By default, when we create a subnet within a VPC, it acts as a **private subnet**.

### Characteristics

- The resources in a private subnet are isolated from other VPCs and external networks.
- Resources in a private subnet **cannot be accessed directly from the internet**.
- Resources within the same VPC can communicate with the private subnet resources.
- Private subnets are typically used for:
  - Databases
  - Application servers
  - Internal services
  - Backend workloads

---

# 2. Public Subnet

Resources created within a **public subnet** can access the internet/public network. Likewise, resources on the internet can access the resources in the public subnet through their **public IP addresses**, provided the necessary security rules allow it.

### Characteristics

- A subnet is considered a **public subnet** when it has a route to an **Internet Gateway (IGW)**.
- An **Internet Gateway (IGW)** is an AWS-managed networking device that connects a VPC to the public internet.
- To make a subnet public:
  1. Create and attach an Internet Gateway to the VPC.
  2. Update the subnet's route table to route internet-bound traffic (`0.0.0.0/0`) to the Internet Gateway.
  3. Assign public IP addresses (or Elastic IPs) to the resources that need internet access.

### Common Use Cases

- Web servers
- Load Balancers
- Bastion Hosts
- Public APIs

---

# 3. Hybrid Subnet

A **hybrid subnet** is a subnet that is **partially open to the public network**.

### Characteristics

- Resources in the subnet **can access the internet** through a **NAT Gateway**.
- All **outgoing traffic** from the subnet is routed to the internet via the NAT Gateway.
- **Incoming connections from the internet are not allowed** directly to the resources in the subnet.
- This provides internet access for updates, package downloads, and external API calls while keeping the resources protected from direct public access.

### Common Use Cases

- Application servers
- Backend services
- Microservices that require outbound internet connectivity but should remain private

---

# Subnet Comparison

| Feature | Private Subnet | Public Subnet | Hybrid Subnet |
|----------|----------------|---------------|----------------|
| Internet Access (Outgoing) | ❌ No (unless NAT Gateway is used) | ✅ Yes | ✅ Yes (via NAT Gateway) |
| Internet Access (Incoming) | ❌ No | ✅ Yes (using Public IP + IGW) | ❌ No |
| Public IP Required | ❌ No | ✅ Yes | ❌ No |
| Internet Gateway Required | ❌ No | ✅ Yes | ❌ No (uses NAT Gateway) |
| NAT Gateway Required | Optional | ❌ No | ✅ Yes |
| Typical Resources | Databases, Internal Servers | Web Servers, Load Balancers | Application Servers, Backend Services |

---

# Summary

- **Private Subnet:** Resources are isolated and accessible only within the VPC.
- **Public Subnet:** Resources are accessible from the internet through an Internet Gateway and public IP addresses.
- **Hybrid Subnet:** Resources can initiate outbound internet connections through a NAT Gateway but cannot receive direct inbound connections from the internet.

  
# NAT Gateway

A **NAT (Network Address Translation) Gateway** is an **AWS-managed networking service** that allows resources in a **private subnet** to initiate **outbound connections** to the internet or other AWS services while **preventing unsolicited inbound connections** from the internet.

### Characteristics

- AWS provides and manages the NAT Gateway.
- Allows **only outbound traffic** from resources in a private subnet.
- Does **not allow inbound traffic** initiated from the internet to the resources in the subnet.
- Resources in the private subnet can:
  - Download software updates.
  - Install packages.
  - Access external APIs.
  - Connect to AWS public services.
- Since inbound traffic is blocked, it provides a high level of security while still enabling internet access when required.

### How It Works

1. A resource in a private subnet sends a request to the internet.
2. The request is routed to the **NAT Gateway**.
3. The NAT Gateway translates the private IP address to its public IP address.
4. The internet sends the response back to the NAT Gateway.
5. The NAT Gateway forwards the response to the originating resource.
6. Any **unsolicited inbound traffic** from the internet is **blocked**.

### Benefits

- Enables secure outbound internet access.
- Prevents direct inbound access from the internet.
- Fully managed by AWS (highly available within an Availability Zone).
- Improves the security of private subnet resources.
- Eliminates the need to manage your own NAT instance.

### Common Use Cases

- Private EC2 instances downloading OS updates.
- Accessing third-party APIs.
- Pulling Docker images from public container registries.
- Installing application dependencies from the internet.
- Accessing AWS public service endpoints.

> **Note:** A NAT Gateway must be deployed in a **public subnet** and requires an **Elastic IP (EIP)**. Private subnet route tables should direct internet-bound traffic (`0.0.0.0/0`) to the NAT Gateway.

## Summary

A **NAT Gateway** is an AWS-managed networking service that allows **only outbound internet communication** for resources in private subnets while **blocking all unsolicited inbound traffic**. It enables organizations to partially expose private subnet resources to the internet for outbound communication while maintaining a high level of security.

## Pictorial Representation - Types of Subnets
<img width="3568" height="4172" alt="11-SEP-2021-SUBNETTYPES" src="https://github.com/user-attachments/assets/7eb10e6c-3b69-42e8-887f-fe0952c1c739" />

