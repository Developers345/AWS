# AWS 2nd Usecase
one public subnet(interget-gateway) and one private subnet

# 3rd Usecase AWS VPC and Subnet Setup

## How to Set Up a Hybrid Subnet

### 1. Create a VPC

Create a VPC in an AWS Region with the following CIDR block:

- **VPC name:** `quickbytesvpc`
- **CIDR block:** `172.19.0.0/16`

### 2. Create Three Subnets

Create one public subnet and two private subnets:

| Subnet name | Type | CIDR block |
|---|---|---|
| `quickbytespublicsubnet1` | Public | `172.19.1.0/24` |
| `quickbytesprivatesubnet1` | Private | `172.19.2.0/24` |
| `quickbytesprivatesubnet2` | Private | `172.19.3.0/24` |

To make the first subnet public, enable **Auto-assign public IPv4 address** in its subnet settings.

### 3. Create an Internet Gateway

Create an Internet Gateway named:

- `quickbytesig`

### 4. Attach the Internet Gateway to the VPC

Attach `quickbytesig` to the VPC:

- `quickbytesvpc`

### 5. Create a Route Table

Create a route table under the VPC:

- **Route table name:** `quickbytesigrt`

### 6. Associate the Public Subnet

Open the route table's **Subnet associations** settings and associate:

- `quickbytespublicsubnet1`

### 7. Add a Route for External Traffic

Edit the route table and add a route that sends external traffic through the Internet Gateway:

| Destination | Target |
|---|---|
| `0.0.0.0/0` | `quickbytesig` |

> **Note:** Associate this route table with the public subnet only. The private subnets should not have a direct route to the Internet Gateway. For private-subnet outbound internet access, use a NAT Gateway in a public subnet and route `0.0.0.0/0` through the NAT Gateway from the private route table.

---
## Diagram - 3rd Usecase
<img width="1879" height="572" alt="Screenshot 2026-10-29 093738" src="https://github.com/user-attachments/assets/b7f9f0bc-0cb2-4123-b8eb-5d4970e073a8" />


## Copy a File from a Local Machine to a Remote Computer Using SCP

### General command

```bash
scp -i <key-file> <local-file-location> ubuntu@<ip-address>:/<remote-path>
```

### Example from the screenshot

```bash
scp -i .ssh\quickbyteskp.pem .ssh\quickbyteskp.pem ubuntu@13.127.192.3:/home/ubuntu/.ssh
```

> **Note:** The example uses Windows-style backslashes in the local key path. In PowerShell, quote paths if needed. The remote destination `/home/ubuntu/.ssh` is a directory; ensure it exists and that the remote user has permission to write to it.
