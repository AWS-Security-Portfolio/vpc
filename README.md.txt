## Table of Contents

- Overview
- Architecture Diagram
- Lab Objectives
- Terraform Deployment
- AWS Resource Validation
- Connectivity Testing
- Security Justifications
- Lessons Learned
- Cleanup
- Screenshots

---

## Overview

This lab demonstrates secure network segmentation in AWS by deploying a VPC with public and private subnets, a NAT Gateway, and a bastion host for controlled SSH access. All infrastructure was automated and documented using Terraform.
The workflow closely follows real-world cloud security best practices.

---

## Architecture Diagram

The architecture diagram can be found as 'diagram.png' in this repository.

---

##Lab Objectives

- Provision an isolated VPC with public/private subnets.
- Deploy a bastion host (public subnet)
- Deploy a private EC2 instance (private subnet, no public IP)
- Configure a NAT Gateway for secure outbound traffic.
- Restrict SSH access: only via bastion, only from my IP.

---

## Terraform Deployment

- Initialize Terraform:
I ran 'terraform init' in CLI to download the AWS provider and initialize the working directory.

- Apply Configuration:
After init was finished, I ran 'terraform apply' to provision all AWS resources defined in main.tf. I confirmed the application plan when prompted and waited for completion.

---

## AWS Resource Validation

- VPC Created:
Verified creation of the custom VPC (10.0.0.0/16) in the AWS Console.

- Subnets:
Confirmed that both public (10.0.1.0/24) and private (10.0.2.0/24) subnets exist,
each in the correct availability zone and with proper public IP settings.

- Internet Gateway:
I verified Internet Gateway is created and attached to the custom VPC, enabling public internet access for the public subnet.

- NAT Gateway & Elastic IP:
I confirmed that a NAT Gateway was provisioned in the public subnet, with an Elastic IP allocated and associated, allowing outbound internet access from the private subnet.

- EC2 Instances:
Ensured both the bastion host (in the public subnet, with a public IP) and the application EC2 (in the private subnet, private IP only) are running and correctly placed.

- Public Route Tables:
Validated the public route table is associated with the public subnet and contains a route to the Internet Gateway (0.0.0.0/0).
- Subnet Association:
Checked that the public subnet is associated with the public route table.

- Private Route Tables:
Verified the private route table is associated with the private subnet and has a
default route to the NAT Gateway for outbound internet.
- Subnet Association:
Checked that the private subnet is associated with the private route table.

---

## Connectivity Testing

- SSH into Bastion Host:
Used the generated SSH private key (lab-bastion-key.pem) to connect to the
bastion host’s public IP.

- SSH from Bastion to Private EC2:
From the bastion host, it used the same SSH key to connect to the EC2 instance’s
private IP, confirming network isolation and secure access.

---

## Security Justifications

- Network Segmentation: Isolates workloads and reduces attack surface.

- Subnets: Public subnet for entry (bastion host); private subnet for resources (app EC2).

- NAT Gateway: Allows private instances to reach the internet for updates/patches
without inbound exposure.

- Bastion Host: Only resource with SSH from outside; limits attack surface.

- Security Groups: Applied least-privilege access (SSH only from my IP, private EC2 only accessible via bastion).

- No Public IP for Private EC2: Prevents direct exposure of sensitive resources.

---

## Lessons Learned

- Terraform made repeatable, consistent deployment easy.
- Gained hands-on understanding of AWS networking components (VPC, subnets, NAT, IGW)
- I saw in practice how network isolation and SSH hopping enforces real security.
- Appreciated importance of resource cleanup to avoid charges.

---

## Cleanup

To avoid AWS charges, resources were destroyed with 'terraform destroy' command in CLI (important)

I then also double-checked the AWS Console manually to ensure:
- All EC2 instances terminated.
- NAT Gateway and Elastic IP were deleted/released.
- Custom VPC, IGW, security groups and key pairs were removed.

---

## Screenshots

They are included in the 'screenshots/' folder, named according to each step.

---

## Acknowledgments

This project was completed as part of my self-guided AWS security learning journey. I’d like to acknowledge the usefulness of official AWS documentation, open-source Terraform modules, and the broader AWS and DevOps community whose shared knowledge accelerated my understanding of cloud networking best practices. Special thanks to interactive guidance and online resources that provided real-time troubleshooting and practical insights during the lab.