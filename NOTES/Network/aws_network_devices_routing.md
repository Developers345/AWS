# VPN Gateway (Virtual Private Gateway)

A **VPN Gateway (Virtual Private Gateway - VGW)** is an **AWS-managed networking device** that enables secure communication between an **AWS VPC** and an **on-premises (corporate) network** over the internet using an encrypted **VPN (Virtual Private Network)** connection.

## Characteristics

- AWS provides and manages the VPN Gateway.
- Enables secure connectivity between AWS resources and on-premises resources.
- Uses encrypted IPSec VPN tunnels to securely transfer data over the public internet.
- Allows organizations to extend their corporate network into AWS.
- Commonly used in hybrid cloud architectures.

## How It Works

1. A **Virtual Private Gateway (VGW)** is attached to the AWS VPC.
2. A **Customer Gateway (CGW)** is configured on the on-premises network.
3. An **IPSec VPN connection** is established between the VGW and the CGW.
4. Resources in the VPC and the on-premises network communicate securely through the encrypted tunnel.

## Common Use Cases

- Connecting corporate data centers to AWS.
- Hybrid cloud deployments.
- Secure access to on-premises databases and applications.
- Disaster recovery and backup solutions.
- Migrating workloads from on-premises to AWS.

> **Note:** For higher bandwidth and more consistent network performance, AWS also provides **AWS Direct Connect**, which establishes a dedicated private connection between your on-premises network and AWS.

---

# Route Table

When a **VPC** is created, AWS automatically creates a **default route table**, also known as the **main (primary) route table**, and associates it with the VPC.

A **Route Table** is a collection of routing rules that determines how network traffic is routed between subnets, gateways, and external networks.

## Characteristics

- Every VPC contains at least one route table (the main route table).
- Each subnet must be associated with one route table.
- A route table contains **routes**, where each route consists of:
  - **Destination** (CIDR block)
  - **Target** (VPC, Internet Gateway, NAT Gateway, VPN Gateway, etc.)

## Default Route

When a VPC is created, AWS automatically adds a default route similar to:

| Destination | Target |
|-------------|---------|
| `10.0.0.0/16` (Example VPC CIDR) | Local |

The **Local** target indicates that all resources within the VPC can communicate with each other across all associated subnets.

For example, if the VPC CIDR is `10.0.0.0/16`:

| Destination | Target | Purpose |
|-------------|---------|----------|
| `10.0.0.0/16` | Local | Enables communication between all subnets within the VPC |

This default route allows network traffic to flow freely between resources in different subnets of the same VPC.

## Additional Routes

Depending on the networking requirements, additional routes can be added to the route table.

| Destination | Target | Purpose |
|-------------|---------|----------|
| `0.0.0.0/0` | Internet Gateway (IGW) | Internet access for public subnets |
| `0.0.0.0/0` | NAT Gateway | Outbound internet access for private subnets |
| On-Premises CIDR | VPN Gateway (VGW) | Secure communication with on-premises networks |
| Peered VPC CIDR | VPC Peering Connection | Communication between VPCs |

## Summary

A **Route Table** is responsible for directing network traffic within a VPC and to external networks. Every VPC has a **main (primary) route table** with a default **Local** route that allows communication between all subnets in the VPC. Additional routes can be configured to enable connectivity through an **Internet Gateway**, **NAT Gateway**, **VPN Gateway**, or other networking components.

## Pictorial Representation - VPN, Route Table
<img width="3568" height="4172" alt="13-SEP-2021-SUBNET" src="https://github.com/user-attachments/assets/abfaf603-ac1c-45d1-9b46-ec3b71938fce" />

## Pictorial Representation - Internet Gateway
<img width="3568" height="4172" alt="13-SEP-2021-IG-RT" src="https://github.com/user-attachments/assets/6b1c9913-13e8-4a15-84ab-dab46de52698" />

