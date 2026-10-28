# Use Case 1: AWS VPC with Public and Private Subnets

## Architecture Overview

**Goal:** Create a VPC with public and private subnets and configure an Internet Gateway (IGW) to allow external network traffic to reach resources in the public subnet.

### Network Components

1. **VPC:** `dailykartvpc` — `172.16.0.0/16`
2. **Public Subnet:** `dailykartpublicsubnet` — `172.16.1.0/24`
3. **Private Subnet:** `dailykartprivatesubnet` — `172.16.2.0/24`
4. **Internet Gateway:** `dailykartig`
5. **EC2 Instance:** An EC2 instance with a public IP address.

---

## Step 1: Create a VPC

Create a VPC with the following configuration:

| Property | Value |
|---|---|
| VPC Name | `dailykartvpc` |
| IPv4 CIDR Block | `172.16.0.0/16` |

The VPC provides an isolated virtual network in which we can create subnets and deploy AWS resources.

## Step 2: Create Two Subnets

Create two subnets inside `dailykartvpc`.

### 2.1 Public Subnet

| Property | Value |
|---|---|
| Subnet Name | `dailykartpublicsubnet` |
| IPv4 CIDR Block | `172.16.1.0/24` |
| VPC | `dailykartvpc` |

### 2.2 Private Subnet

| Property | Value |
|---|---|
| Subnet Name | `dailykartprivatesubnet` |
| IPv4 CIDR Block | `172.16.2.0/24` |
| VPC | `dailykartvpc` |

At this stage, both subnets exist within the VPC. However, creating a subnet does not automatically provide it with internet connectivity.

## Step 3: Create an Internet Gateway

Create an Internet Gateway with the following configuration:

| Property | Value |
|---|---|
| Internet Gateway Name | `dailykartig` |
| Initial State | Detached |

An Internet Gateway (IGW) allows communication between a VPC and the internet when the appropriate routing and network configurations are in place.

## Step 4: Attach the Internet Gateway to the VPC

By default, a newly created Internet Gateway is detached from any VPC.

To attach it:

1. Open the **Internet Gateways** section in the AWS Management Console.
2. Select `dailykartig`.
3. Click **Actions**.
4. Select **Attach to a VPC**.
5. Choose `dailykartvpc`.
6. Confirm the attachment.

The Internet Gateway is now attached to the VPC.

**Important:** Attaching an Internet Gateway to a VPC does not automatically enable internet connectivity for its subnets. We must configure routing rules to direct external traffic through the Internet Gateway.

---

## Step 5: Create a Route Table

To route external network traffic through the Internet Gateway, we need a route table containing the appropriate routing rule.

We have two options:

1. Modify the VPC's existing main route table.
2. Create a new route table and configure it for the public subnet.

**Recommended approach:** Create a separate route table for the public subnet instead of modifying the default route table. This keeps the public and private subnet routing configurations easier to manage.

Create a route table with the following configuration:

| Property | Value |
|---|---|
| Route Table Name | `dailykartpublicrt` |
| VPC | `dailykartvpc` |

## Step 6: Configure the Route Table

### 6.1 Associate the Public Subnet

Associate `dailykartpublicsubnet` with the newly created route table, `dailykartpublicrt`.

To do this:

1. Open the **Route Tables** section.
2. Select `dailykartpublicrt`.
3. Open the **Subnet associations** tab.
4. Click **Edit subnet associations**.
5. Select `dailykartpublicsubnet`.
6. Save the changes.

This association ensures that the public subnet uses the routing rules defined in `dailykartpublicrt`.

### 6.2 Add a Route to the Internet Gateway

Edit the routes in `dailykartpublicrt` and add the following rule:

| Destination | Target | Purpose |
|---|---|---|
| `172.16.0.0/16` | `local` | Allows communication within the VPC |
| `0.0.0.0/0` | `dailykartig` | Routes IPv4 traffic destined for external networks through the Internet Gateway |

The `0.0.0.0/0` destination represents all IPv4 destinations that do not match a more specific route.

The route directs this traffic to the Internet Gateway.

Save the changes.

---

## Step 7: The Subnet Is Now a Public Subnet

After completing the previous steps, `dailykartpublicsubnet` has a route to the Internet Gateway.

The routing configuration looks like this:

```text
dailykartvpc (172.16.0.0/16)
│
├── dailykartpublicsubnet (172.16.1.0/24)
│   │
│   ├── EC2 Instance (with public IPv4 address)
│   │
│   └── dailykartpublicrt
│       ├── 172.16.0.0/16 → local
│       └── 0.0.0.0/0 → dailykartig
│
└── dailykartprivatesubnet (172.16.2.0/24)
    │
    └── Uses its associated route table
        └── No route to the Internet Gateway by default
```

A subnet is considered **public** when its associated route table has a route to an Internet Gateway.

A subnet is considered **private** when it does not have a direct route to an Internet Gateway.

> **Note:** A private subnet can still have internet access through a NAT Gateway if its route table is configured accordingly.

---

## Step 8: Configure the EC2 Instance for Internet Connectivity

Even though the public subnet has a route to the Internet Gateway, an EC2 instance needs additional configuration for direct internet connectivity.

Verify the following:

1. **Public IPv4 address:** The EC2 instance must have a public IPv4 address or an Elastic IP address.
2. **Route table:** The subnet must be associated with a route table containing the route `0.0.0.0/0 → dailykartig`.
3. **Internet Gateway:** `dailykartig` must be attached to `dailykartvpc`.
4. **Security group:** The instance's security group must allow the required inbound and outbound traffic.
5. **Network ACL:** The subnet's network ACL must permit the required traffic, including return traffic.
6. **Operating system firewall:** The instance's operating system must allow the intended traffic.

If these requirements are satisfied, the EC2 instance can communicate with the internet, subject to the configured network and security rules.

---

## Final Summary

The steps to configure a public subnet are:

1. Create `dailykartvpc` with CIDR block `172.16.0.0/16`.
2. Create `dailykartpublicsubnet` with CIDR block `172.16.1.0/24`.
3. Create `dailykartprivatesubnet` with CIDR block `172.16.2.0/24`.
4. Create the Internet Gateway `dailykartig`.
5. Attach `dailykartig` to `dailykartvpc`.
6. Create the route table `dailykartpublicrt`.
7. Associate `dailykartpublicsubnet` with `dailykartpublicrt`.
8. Add the route `0.0.0.0/0 → dailykartig`.
9. Launch or configure an EC2 instance in the public subnet with a public IPv4 address.
10. Verify the required security group, network ACL, and instance firewall rules.

**Key takeaway:** Attaching an Internet Gateway to a VPC makes it available to the VPC, but routing external traffic through it requires an appropriate route table associated with the subnet. An EC2 instance also needs a public IP address and suitable network and security configurations to communicate directly with the internet.

