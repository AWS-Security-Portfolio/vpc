## AWS Virtual Private Cloud Lab

Engineered segmented VPC networking with public/private subnets, NAT, and bastion host for secure, isolated AWS infrastructure.

---

## Table of Contents

- [Overview](#overview)
- [Real-World Risk](#real-world-risk)
- [What I Built](#what-i-built)
- [Diagram](#diagram)
- [Objectives](#objectives)
- [Steps Performed](#steps-performed)
  - [1. Terraform Initialization and Deployment]
  - [2. VPC and Subnet Configuration]
  - [3. Internet Gateway and NAT Gateway Setup]
  - [4. Route Table Configuration]
  - [5. EC2 Deployment and Security]
  - [6. Connectivity Testing]
  - [7. Cleaunp]
- [Screenshots](#screenshots)
- [Lessons Learned](#lessons-learned)
- [References](#references)
- [Contact](#contact)

---

## Overview

This project demonstrates the process of designing and deploying a secure Virtual Private Cloud (VPC) in AWS using Terraform. The lab focuses on network segmentation, least-privilege access, and secure connectivity through public/private subnets, a NAT gateway, and a bastion host.

---

## Real-World Risk

In real-world AWS environments, placing all cloud resources in a flat, public network dramatically increases the risk of unauthorized access, data breaches and lateral movement by attackers. Without proper network segmentation, a compromised instance with a public IP could be used as a pivot point to attack other assets, escalate privileges or exfiltrate sensitive data. Misconfigured security groups, open SSH ports and a lack of egress controls further expose your environment to brute-force attacks, malware and unmonitored outbound connections. Effective VPC design and segmentation, using bastion hosts and NAT gateways, are critical controls to contain and limit potential threats, enforcing the principle of least privilege at the network level.

---

## What I Built

- A custom AWS VPC with both public and private subnets, enabling strict network segmentation.
- Internet Gateway and NAT Gateway setup for safe outbound access while keeping private instances non-public.
- Bastion host deployed in the public subnet, secured for SSH access from a trusted IP.
- Application EC2 instance launched in the private subnet with no public IP, accessible only via the bastion.
- Security groups enforcing least-privilege rules for all access points.
- Automated all infrastructure deployment and teardown using Terraform.
- Documentation and screenshots validating each security control and design decision.

---

## Diagram

![Architecture Diagram](diagram.png)

---

## Objectives

- Create a custom VPC with public and private subnets.
- Deploy a bastion host for secure SSH access.
- Deploy an application EC2 instance in a private subnet (no public IP).
- Configure a NAT gateway to provide outbound internet for private resources.
- Apply security groups and route tables to enforce network isolation and least-privilege principles.
- Document all resources and security controls with clear evidence.

---

## Steps Performed

**1. Terraform Initialization and Deployment**
   - Initialized the Terraform project to prepare for AWS infrastructure deployment *(Screenshot: `terraforminit-success.png`)*
   - Ran terraform apply and confirmed successful provisioning of all AWS resources *(Screenshot: `terraformapply-success.png`)*

**2. VPC and Subnet Configuration**
   - Created a custom VPC (10.0.0.0/16) for network segmentation *(Screenshot: `vpc-success.png`)*
   - Deployed a public subnet (10.0.1.0/24) and a private subnet (10.0.2.0/24), each configured in the correct availability zone *(Screenshots: `subnets-public-success.png` & `subnets-private-success.png`)*

**3. Internet Gateway and NAT Gateway Setup**
- Attached an Internet Gateway (IGW) to the VPC for public subnet internet connectivity *(Screenshot: `igw-success.png`)*
- Allocated an Elastic IP address and created a NAT Gateway in the public subnet for private subnet outbound Internet access *(Screenshots: `natgateway-success.png` & `ec2-elasticip-success.png`)*

**4. Route Table Configuration**
- Configured a public route table to send all outbound traffic (0.0.0.0/0) through the IGW and associated it with the public subnet *(Screenshots: `publicroutetable-success.png` & `publicroutetable-association-success.png`)*
- Configured a private route table to send all outbound traffic (0.0.0.0/0) through the NAT Gateway and associated it with the private subnet *(Screenshots: `privateroutetable-success.png` & `privateroutetable-association-success.png`)*

**5. EC2 Instance Deployment and Security**
- Launched a bastion host in the public subnet with a public IP, secured for SSH access from a trusted IP.
- Launched a private EC2 instance in the private subnet (no public IP), only accessible via SSH from the bastion host *(Screenshot: `3c2-instances-success.png`)*

**6. Connectivity Testing**
- Connected via SSH to the bastion host in the public subnet *(`Screenshot: ssh-bastion-connection.png`)*
- From the bastion host, established SSH access to the private EC2 instance, demonstrating enforced network isolation and bastion use *(Screenshot: `ssh-bastion-to-private-ec2.png`)*

**7. Cleanup**
- Used terraform destroy to delete all lab resources and prevent ongoing AWS charges.
   
---

## Screenshots

*All screenshots are included in the screenshots/ folder of this repository.*

| Order | File Name                                 | What it Shows                                          |
| ----- | ----------------------------------------- | ------------------------------------------------------ |
| 1     | terraforminit-success.png                 | Successful initialization of Terraform project         |
| 2     | terraformapply-success.png                | Terraform apply run with resources created             |
| 3     | vpc-success.png                           | Custom VPC created with correct CIDR                   |
| 4     | subnets-public-success.png                | Public subnet created and configured                   |
| 5     | subnets-private-success.png               | Private subnet created and configured                  |
| 6     | igw-success.png                           | Internet Gateway attached to VPC                       |
| 7     | natgateway-success.png                    | NAT Gateway provisioned and available                  |
| 8     | ec2-elasticip-success.png                 | Elastic IP assigned for NAT Gateway                    |
| 9     | publicroutetable-success.png              | Public route table with IGW route                      |
| 10    | publicroutetable-association-success.png  | Public subnet associated with public route table       |
| 11    | privateroutetable-success.png             | Private route table with NAT Gateway route             |
| 12    | privateroutetable-association-success.png | Private subnet associated with private route table     |
| 13    | 3c2-instances-success.png                 | Bastion and private EC2 instances deployed and running |
| 14    | ssh-bastion-connection.png                | SSH connection established to bastion host             |
| 15    | ssh-bastion-to-private-ec2.png            | SSH connection from bastion to private EC2 instance    |

---

## References

- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Terraform AWS Provider Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS Security Best Practices](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-standards-fsbp.html)

---

## Contact

Sebastian Silva C. – July, 2025 – Berlin, Germany.  
[LinkedIn](https://www.linkedin.com/in/sebastiansilc) | [GitHub](https://github.com/SebaSilC) | [sebastian@playbookvisualarts.com](mailto:sebastian@playbookvisualarts.com)
