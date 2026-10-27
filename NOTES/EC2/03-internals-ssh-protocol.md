# SSH Authentication with an AWS EC2 Instance

## How to connect to a remote machine using an SSH client

When we run:

``` bash
ssh user@ipaddress
```

the SSH client software tries to connect to the remote machine using the
supplied IP address and username. In password-based SSH access, the
client prompts the user to enter a password to log in.

## Why AWS EC2 uses an SSH key pair

When we submit a provisioning request, AWS installs and configures the
Linux operating system on the EC2 instance. It creates a default user
for the operating system. For SSH access, AWS commonly configures
key-based authentication so that we do not need to enter an account
password.

AWS creates an SSH key pair that is used to authenticate access to the
remote computer. During instance provisioning, we download and keep the
private key on our workstation. The public key is configured for use by
the remote instance.

> **Important:** The private key must remain private. The public key can
> be placed on the server. Never share or upload the private key to the
> EC2 instance or other people.

## Where to store the key pair

After downloading the SSH key pair, store the private key under the
`.ssh` directory in your workstation's home directory:

``` text
~/.ssh/
```

The key should be owned by your user and have restrictive permissions.
For a typical private key file, set permissions to `600`:

``` bash
chmod 600 ~/.ssh/<private-key-file>
```

## SSH connection command

The diagram shows an example where SSH uses port `2222`:

``` bash
ssh -p 2222 -i ~/.ssh/<private-key-file> user@ipaddress
```

-   `-p 2222` specifies the SSH port. The default SSH port is `22`; use
    `2222` only if the server is configured to listen on that port.
-   `-i ~/.ssh/<private-key-file>` specifies the private key file.
-   `user@ipaddress` specifies the remote username and the instance's
    public IP address.

## SSH key-exchange and authentication flow

The image illustrates these main steps:

1.  **Download the key pair:** The user downloads the SSH key pair
    during EC2 provisioning and stores the private key securely in
    `~/.ssh/` on the workstation.
2.  **Server sends a challenge:** During SSH connection setup, the
    server can send authentication data or a challenge that the client
    must answer. The exact protocol is more sophisticated than simply
    encrypting a random text with the public key.
3.  **Client responds:** The SSH client uses the private key to prove
    possession of the key, typically by signing protocol data. The
    private key is not sent to the server.
4.  **Encrypted session is established:** After authentication and key
    exchange, the client and server establish session keys. SSH traffic
    is encrypted in transit; the client and server encrypt/decrypt data
    as it is sent and received. 

### Note
After authentication completed from client side then ssh server will generate encrypted key using public key and send to client for communication.

### Simplified flow diagram

``` mermaid
flowchart LR
    A[Workstation<br/>SSH client and private key<br/>~/.ssh/] -->|SSH connection| B[EC2 instance<br/>SSH server and public key]
    B --> C[Authentication challenge]
    C --> D[Client proves possession<br/>of private key]
    D --> E[Authentication succeeds]
    E --> F[Encrypted SSH session]
```

## Diagram
<img width="1800" height="885" alt="Screenshot 2026-10-27 130442" src="https://github.com/user-attachments/assets/e25c5dc8-bc47-47d1-aa46-7d4773cf8f66" />

## Key points

-   SSH supports both password-based and public-key authentication.
-   In public-key authentication, the server stores or trusts the
    **public key**; the client keeps the matching **private key**.
-   The private key should not be transmitted to the server.
-   Protect the private key with restrictive file permissions, commonly
    `chmod 600`.
-   Ensure the EC2 security group, network ACLs, and server firewall
    allow the configured SSH port.
-   Use the correct default username for the AMI. For example, many
    Amazon Linux images use `ec2-user`, while Ubuntu images commonly use
    `ubuntu`.
