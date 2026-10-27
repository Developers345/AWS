# AWS Compute Domain

## Overview

The computing resources required to run software applications are provided as part of the **Compute domain**.

The five services covered in these notes are:

1. **EC2** — Amazon Elastic Compute Cloud
2. **Elastic Load Balancing (ELB)**
3. **AWS Lambda**
4. **Auto Scaling Group (ASG)** — commonly managed through EC2 Auto Scaling
5. **Elastic Beanstalk** — AWS Elastic Beanstalk

> **Terminology note:** The original notes list “elasitc bean stalk (ebs).” The service intended here appears to be **Elastic Beanstalk**. **EBS** usually means **Amazon Elastic Block Store**, which provides block storage for EC2 instances.

## 1. Amazon EC2

**EC2** stands for **Amazon Elastic Compute Cloud**. It provides virtual compute machines (instances) on which we can host and run software applications.

To run an application, a computer or machine needs computing and networking resources, such as:

- CPU
- RAM
- Hard disk / storage (HDD or SSD)
- Operating system
- System utility software
- Network services, including:
  - Network connectivity
  - Security and traffic restrictions
  - Other networking capabilities

The network services and controls are covered as part of the **Networking domain**.

An EC2 instance is launched in a selected AWS Region and Availability Zone, within a VPC and subnet. Network access can be restricted using controls such as security groups and network ACLs.

### 1.1 Choosing a VPC and subnet

To create an EC2 instance, we can either:

- Use a default VPC and its subnet, where available and suitable; or
- Create our own VPC and subnet with the required network configuration and security restrictions.

When launching the instance, choose the Region, VPC, and subnet in which it should run.

### 1.2 Requesting an EC2 instance

We submit a provisioning request to AWS to create an EC2 compute instance and specify the required resources and configuration, including:

- Region and Availability Zone
- VPC and subnet
- Instance type (CPU and memory capacity)
- Storage capacity and type
- Amazon Machine Image (AMI)
- Security group
- SSH key pair, if using SSH key-based access

Then launch the instance.

### 1.3 Instance type (instance “shape”)

AWS offers predefined instance types. An instance type determines the combination of CPU, memory (RAM), networking, and other capabilities allocated to the instance.

Examples from the original notes include:

- `t2.nano`
- `t2.micro`
- `t2.large`

The appropriate type depends on the application's requirements. **Pricing varies by instance type**, and the selected AWS Region can also affect pricing.

### 1.4 Storage

Choose the required storage capacity and type—for example, 10 GB of storage, with an appropriate SSD- or HDD-based option where supported.

EC2 storage can include Amazon EBS volumes and, for supported instance types, instance store. The available choices and pricing depend on the storage option selected.

### 1.5 AMI (Amazon Machine Image)

**AMI** stands for **Amazon Machine Image**.

An AMI is a template used to launch an EC2 instance. It includes the information needed to create an instance, such as the operating system and may include pre-installed system utilities or application software.

AMI choices are available from AMI catalogs and registries, including:

- Linux images
- Unix-related images
- Windows images

Images may be open source or commercial, depending on the operating system, software, and image publisher.

The accompanying diagram illustrates the idea of an EC2 instance as a combination of software and hardware resources, and shows an AMI registry with Linux, Unix, and Windows image categories.

![EC2 instance components and AMI registry diagram](ec2-ami-diagram.png)

> **Technical clarification:** An AMI is not simply an EBS snapshot. An AMI contains launch information and references to the storage snapshots or image data needed to create an instance. An AMI can be backed by Amazon EBS snapshots; instance-store-backed AMIs are also part of EC2's historical terminology. Do not treat “backed in instance store” as a universal description of every AMI.

### 1.6 Security group

Define or assign a **security group** to control the inbound and outbound network traffic allowed for the instance. Security groups act as virtual firewalls for associated resources.

### 1.7 SSH key pair

Associate or select a **key pair** if SSH key-based access is required—for example, to connect to a Linux instance. Access methods depend on the operating system and instance configuration.

### 1.8 Launch the instance

After selecting the Region, VPC, subnet, instance type, storage, AMI, security group, and key pair as needed, launch the EC2 instance.

## 2. EC2 provisioning checklist

The following checklist consolidates the provisioning steps described above:

1. **Choose the Region**, Availability Zone, VPC, and subnet where the instance should be launched.
2. **Choose the instance type** (the instance “shape”), which determines the CPU and RAM combination. Examples: `t2.nano`, `t2.micro`, and `t2.large`.
3. **Choose storage**, including capacity (for example, 10 GB) and an appropriate storage type.
4. **Choose an AMI** (Amazon Machine Image), which supplies the image/template used to provision the instance, including its operating system and any software already included in that image.
5. **Define or assign a security group** to apply the required traffic restrictions.
6. **Associate an SSH key pair** if needed for SSH access.
7. **Launch the instance**.

## 3. Billing considerations

The original notes identify these main factors:

1. **Instance type:** Pricing depends on the selected instance type and its allocated resources.
2. **Region:** Pricing can vary by the AWS Region selected.
3. **Storage:** Storage capacity and storage type can affect the total cost.

Other charges may apply depending on usage and configuration, such as data transfer, public IPv4 addresses, and related AWS services. Check current AWS pricing for the selected Region and resources.

## Pictorial Representation
<img width="1638" height="472" alt="image" src="https://github.com/user-attachments/assets/81fd45a9-10ca-4b17-a013-0aa2e4e36445" />


## 4. Quick summary

An EC2 instance is a virtual compute machine used to run applications. To provision one, select where it will run (Region, Availability Zone, VPC, and subnet), its compute capacity (instance type), its storage, the AMI, and its network-access configuration (security group and, where applicable, key pair). Then launch it.


