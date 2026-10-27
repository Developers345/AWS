# SSH (Secure Shell)

SSH stands for **Secure Shell**. It is a network protocol used to connect to and manage a remote computer securely. Through SSH, we can run shell commands on a remote system and perform tasks such as installing software, changing configuration, and running applications.

## SSH client and SSH server

SSH communication typically involves two components:

1. **SSH server**  
   The remote computer runs an SSH server (commonly `sshd`). It listens for incoming SSH connections and, after authenticating a client, provides access according to the user's permissions.

2. **SSH client**  
   The local computer runs an SSH client. It connects to the remote SSH server and lets the user run commands on the remote system.

## SSH versus Telnet

Before SSH, **Telnet** was commonly used for remote terminal access. Telnet does not encrypt the session by default, so credentials and other data can be sent as readable plain text. Someone able to observe the network traffic may be able to read that information.

SSH addresses this problem by encrypting the connection. This protects the confidentiality and integrity of the session while data travels between the client and server.

## SSH authentication with a key pair

SSH supports several authentication methods, including passwords and public-key authentication. On many cloud virtual machines, such as AWS EC2 Linux instances, key-pair authentication is commonly used.

A key pair consists of:

- **Private key** — kept securely by the user on the local computer. Do not share it.
- **Public key** — installed on the remote server (for example, in the user's `~/.ssh/authorized_keys` file).

During public-key authentication, the client proves that it possesses the private key corresponding to the public key on the server. The private key itself is not sent to the server.

### Why use a key pair for AWS EC2?

For many EC2 Linux launch configurations, AWS lets you select or create a key pair. The public key is made available to the instance, while you keep the private key. This avoids needing AWS to generate and hand you a reusable Linux login password.

Key-pair authentication is not the only SSH authentication method, and it does not mean SSH can never use a username and password. Rather, key-based authentication is a common and recommended option for EC2 Linux access.

## Example connection

```bash
ssh -i /path/to/private-key.pem ec2-user@<public-ip-or-dns>
```

Replace `/path/to/private-key.pem` with the path to your private key and `<public-ip-or-dns>` with the instance's public IP address or DNS name. The login name depends on the operating system image; `ec2-user` is common for Amazon Linux.

For example, to inspect a local text file, you might use:

```bash
cat password.txt
```

Be careful not to expose passwords, private keys, or other secrets in terminal output, screenshots, logs, or a public GitHub repository.

## Diagram

The diagram below illustrates a local workstation using an SSH client and a key pair to connect to a remote EC2 instance in the AWS cloud.

![SSH client, key pair, and remote EC2 instance diagram](23-SEP-2021-SSH.png)
<img width="1480" height="673" alt="Screenshot 2026-10-27 125414" src="https://github.com/user-attachments/assets/0927963a-95d2-4bda-aa49-b00a44f94818" />


## Key takeaways

- SSH means **Secure Shell**.
- An SSH client initiates the connection; an SSH server runs on the remote machine.
- SSH encrypts traffic between the client and server.
- Telnet does not encrypt its session by default.
- With public-key authentication, the private key stays with the client and the corresponding public key is installed on the server.
- Keep private keys and credentials secret. Never commit them to GitHub.
