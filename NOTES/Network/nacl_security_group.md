# Network Access Control List (NACL)

## What are NACL Rules?

**NACL (Network Access Control List)** is a **subnet-level firewall** in AWS. It is used to control the network traffic entering and leaving an entire subnet.

A NACL contains a set of **allow** and **deny** rules that determine which network traffic is permitted or blocked for all resources within the associated subnet.

---

## Purpose of NACL

The primary purpose of a NACL is to:

- Control inbound and outbound traffic at the subnet level.
- Add an additional layer of security for resources in a subnet.
- Allow or deny traffic based on:
  - Protocol (TCP, UDP, ICMP, etc.)
  - Port number
  - Source or destination IP address (CIDR range)

---

## Characteristics of NACL

- Works at the **Subnet Level**.
- Controls traffic for **all resources** within the subnet.
- Supports both **Allow** and **Deny** rules.
- Rules are evaluated in **ascending order** based on their rule numbers.
- The first matching rule is applied, and the remaining rules are ignored.
- Every subnet must be associated with one NACL.
- Every VPC has a **default NACL**.

---

## Inbound and Outbound Rules

NACL rules are divided into two categories:

### Inbound Rules (Ingress)

These rules control traffic entering the subnet.

Example:
- Allow HTTP (TCP Port 80)
- Allow HTTPS (TCP Port 443)
- Deny traffic from a specific IP range

### Outbound Rules (Egress)

These rules control traffic leaving the subnet.

Example:
- Allow internet access
- Allow database communication
- Restrict outbound traffic to certain destinations

---

## Stateless Nature of NACL

NACLs are **Stateless**.

This means they **do not remember** previous network requests.

As a result:

- A request and its response are treated as **two separate network connections**.
- If you allow inbound traffic, you must also explicitly allow the corresponding outbound response.
- Similarly, if outbound traffic is allowed, the inbound response must also be explicitly allowed.

### Example

Suppose you allow incoming HTTP traffic on port **80**.

You must also allow the outgoing response (typically using the ephemeral port range) so that the communication can complete successfully.

---

## Rule Evaluation

NACL rules are processed in numerical order.

Example:

| Rule Number | Action | Protocol | Port | Source |
|-------------|--------|----------|------|--------|
| 100 | Allow | TCP | 80 | `0.0.0.0/0` |
| 110 | Allow | TCP | 443 | `0.0.0.0/0` |
| 120 | Deny | All | All | `192.168.1.0/24` |
| * | Deny | All | All | All Traffic |

The first matching rule is applied.

---

## Default NACL

Every VPC automatically contains a **Default Network ACL**.

By default:

- All inbound traffic is allowed.
- All outbound traffic is allowed.

Custom NACLs can be created to implement more restrictive security policies.

---

# Security Groups

A **Security Group** is a **resource-level (instance-level) virtual firewall** that controls inbound and outbound traffic for individual AWS resources such as:

- EC2 Instances
- RDS Databases
- Elastic Load Balancers (ELB)
- Lambda Functions (within a VPC)
- Amazon ECS Tasks

Unlike NACLs, Security Groups provide security for individual resources rather than an entire subnet.

---

## Purpose of Security Groups

Security Groups are used to:

- Protect individual AWS resources.
- Apply different security rules to different resources.
- Control inbound and outbound traffic based on:
  - Protocol
  - Port
  - Source or destination IP address
  - Other Security Groups

---

## Characteristics of Security Groups

- Operate at the **Resource (Instance) Level**.
- Created within a **VPC**.
- Applied directly to AWS resources.
- Support **Allow rules only** (no explicit Deny rules).
- Multiple Security Groups can be attached to a single resource.
- Security Groups are **Stateful**.

---

## Stateful Nature of Security Groups

Security Groups are **Stateful**.

This means they automatically keep track of network connections.

If an inbound request is allowed:

- The corresponding outbound response is automatically allowed.
- No separate outbound rule is required.

Similarly:

- If outbound traffic is allowed, the corresponding inbound response is automatically permitted.

### Example

If an EC2 instance allows inbound HTTP traffic on **Port 80**, the response to that request is automatically allowed without creating an additional outbound rule.

---

# NACL vs Security Group

| Feature | NACL | Security Group |
|---------|------|----------------|
| Full Form | Network Access Control List | Security Group |
| Security Level | Subnet Level | Resource (Instance) Level |
| Applies To | Entire Subnet | Individual AWS Resources |
| Supports Allow Rules | ✅ Yes | ✅ Yes |
| Supports Deny Rules | ✅ Yes | ❌ No |
| Stateful | ❌ No (Stateless) | ✅ Yes (Stateful) |
| Rule Processing | Ordered (Rule Numbers) | All rules are evaluated together |
| Separate Inbound & Outbound Rules | ✅ Yes | ✅ Yes |
| Automatic Response Allowed | ❌ No | ✅ Yes |
| Default Behavior | Default NACL allows all traffic | Default Security Group allows all outbound traffic and inbound traffic only from resources within the same Security Group |

---

# Summary

- **NACL (Network Access Control List)** is a **subnet-level, stateless firewall** that supports both **Allow** and **Deny** rules. Since it is stateless, inbound and outbound traffic must be explicitly permitted.
- **Security Groups** are **resource-level, stateful firewalls** that support **Allow rules only**. They automatically allow response traffic for established connections, making them simpler to manage than NACLs.

# Pictorial Representation - NACL
<img width="3568" height="4172" alt="14-SEP-2021-NACL" src="https://github.com/user-attachments/assets/83ad000a-171d-4490-bfb0-8b424cc229fc" />

# Pictorial Representation - Security Group
<img width="3568" height="4172" alt="15-SEP-2021-SECURITYGROUP" src="https://github.com/user-attachments/assets/27da197d-e3b9-4d3f-b8ac-aa615a1c5f86" />
