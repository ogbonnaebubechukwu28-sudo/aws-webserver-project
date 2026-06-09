# AWS Web Server Deployment with VPC, EC2, Nginx and Security Best Practices

**Prepared by:** Ebubechukwu Ogbonna
**Deployment Date:** 9th June 2026

---

## 1. Introduction

This project demonstrates the deployment of a secure, publicly accessible web server on AWS following cloud networking and security best practices. It covers the full process — from designing the network architecture and provisioning AWS resources, to configuring a Linux server, securing it with proper IAM and Security Group rules, and serving a live webpage using Nginx.

The project was completed as part of a Junior Cloud Engineer training exercise, showcasing hands-on experience with core AWS services including VPC, EC2, Security Groups, IAM, and Linux administration.

**Live URL:** http://13.48.6.29/

---

## 2. Architecture

The following diagram illustrates the cloud architecture designed for this project:

```
Internet Users
      |
Internet Gateway
      |
  Route Table
      |
 Public Subnet (10.0.1.0/24)
      |
 Security Group
      |
  EC2 Instance
      |
    Nginx
```

The architecture follows a standard public subnet model where internet traffic flows through the Internet Gateway, is routed via the Route Table into the public subnet, and reaches the EC2 instance through a Security Group that filters allowed traffic.

*(See architecture diagram in the architecture/ folder of the project repository)*

---

## 3. Deployment Steps

### Step 1: Network Infrastructure Setup
- Created a custom VPC with CIDR block **10.0.0.0/16**
- Created a public subnet with CIDR block **10.0.1.0/24**
- Created and attached an Internet Gateway to the VPC
- Configured a Route Table with a **0.0.0.0/0** route pointing to the Internet Gateway
- Associated the Route Table with the public subnet

### Step 2: EC2 Instance Launch
- Launched an Ubuntu Server EC2 instance inside the public subnet
- Enabled auto-assign public IP
- Attached the appropriate Security Group
- Downloaded and stored the key pair (.pem file) for SSH access

### Step 3: Security Group Configuration
- Created a Security Group with the following inbound rules:
  - Port 22 (SSH) — restricted to my IP address only
  - Port 80 (HTTP) — open to all (0.0.0.0/0)
  - Port 443 (HTTPS) — open to all (0.0.0.0/0)

### Step 4: Connecting to the EC2 Instance
- Connected to the EC2 instance via SSH using the command:
```bash
ssh -i "your-key.pem" ubuntu@13.48.6.29
```

### Step 5: Nginx Installation and Configuration
- Updated the server packages
- Installed Nginx
- Started and enabled Nginx to run on boot
- Created a custom HTML webpage with my name and deployment date
- Verified the webpage was accessible via the public IP

---

## 4. Security Measures

### Security Groups
Three inbound rules were configured:

| Port | Protocol | Source | Purpose |
|------|----------|--------|---------|
| 22 | TCP | My IP only | Secure SSH access |
| 80 | TCP | 0.0.0.0/0 | Public HTTP access |
| 443 | TCP | 0.0.0.0/0 | Public HTTPS access |

SSH access was deliberately restricted to my IP address only to prevent unauthorized remote access to the server.

### IAM
An IAM user was created with only the permissions necessary to complete this project, following the principle of least privilege. Root account credentials were not used for day-to-day tasks.

### SSH Restrictions
- A key pair (.pem file) was used for authentication instead of a password
- SSH access was limited to a specific IP address via the Security Group
- The private key file was stored securely and not shared

---

## 5. Challenges and Solutions

*(Pulled from troubleshooting.md in the project repository)*

Please refer to the `troubleshooting.md` file in the repository for a detailed breakdown of all issues encountered, their causes, and solutions.

---

## 6. Lessons Learned

Completing this project significantly improved my understanding of the following concepts:

- **VPC Networking** — I now understand how traffic flows from the internet into a private cloud network through an Internet Gateway and Route Table
- **Subnetting** — Working with CIDR blocks made IP addressing and subnet design much clearer
- **Security Groups** — I learned how to think about security at the network level, only opening ports that are absolutely necessary
- **Linux Administration** — Installing and managing Nginx from the command line reinforced my confidence with Linux
- **SSH Access Management** — Using key pairs and IP restrictions made me appreciate how important access control is in cloud environments
- **Troubleshooting** — Debugging real issues during deployment taught me how to read error messages and trace problems systematically

---

## 7. Repository

All project files, screenshots, architecture diagram, deployment script, and troubleshooting log are available at:

**GitHub:** https://github.com/ogbonnaebubechukwu28-sudo/aws-webserver-project/tree/main

---

*Prepared by Ebubechukwu Ogbonna | June 9, 2026*
