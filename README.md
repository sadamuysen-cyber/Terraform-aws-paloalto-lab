# Terraform-aws-paloalto-lab
Palo Alto Networks AWS Terraform Lab is an automated hands-on environment designed to test, validate, and deploy Palo Alto VM-Series Virtual Firewalls or Cloud Next-Generation Firewalls (NGFW) inside Amazon Web Services (AWS) using Infrastructure as Code (IaC)

# AWS Palo Alto VM-Series Firewall & Windows Server Deployment using Terraform

## 📖 Overview

This project demonstrates how to deploy a **Palo Alto VM-Series Next-Generation Firewall** and a **Windows Server 2022 EC2 instance** in **Amazon Web Services (AWS)** using **Terraform**.

The infrastructure is deployed using Infrastructure as Code (IaC) best practices, creating networking, routing, security groups, Elastic IPs, firewall interfaces, and compute resources automatically.

---

## 🚀 Features

- Deploy Palo Alto VM-Series NGFW on AWS
- Deploy Windows Server 2022 EC2 Instance
- Infrastructure as Code (Terraform)
- Multi-NIC Firewall Deployment
- VPC with Management, Untrust, and Trust Networks
- Elastic IP Assignment
- Security Groups
- Route Tables
- Modular Terraform Configuration
- Production-Ready Project Structure

---

# 🏗 Architecture

```
                    Internet
                        │
                Internet Gateway
                        │
        ┌───────────────┴───────────────┐
        │                               │
 Management Subnet               Untrust Subnet
     (eth0)                         (eth1)
        │                               │
        └──────── Palo Alto Firewall ───┘
                    Trust (eth2)
                        │
                  Trust Subnet
                        │
              Windows Server 2022
```

---

# 📁 Project Structure

```
terraform-paloalto-aws/
│
├── provider.tf
├── variables.tf
├── terraform.tfvars
├── locals.tf
├── network.tf
├── security-groups.tf
├── route-tables.tf
├── firewall.tf
├── windows.tf
├── outputs.tf
├── versions.tf
├── README.md
└── .gitignore
```

---

# ⚙ Components

## Networking

- Custom VPC
- Internet Gateway
- Management Subnet
- Untrust Subnet
- Trust Subnet
- Route Tables
- Network Interfaces

---

## Palo Alto Firewall

- VM-Series Next-Generation Firewall
- Management Interface
- Untrust Interface
- Trust Interface
- Elastic IP for Management
- Elastic IP for Untrust
- HTTPS Management Access
- SSH Access
- Bootstrap Ready

---

## Windows Server

- Windows Server 2022
- Private EC2 Instance
- RDP Access
- User Data Configuration
- Internal Network Connectivity

---

# 🔒 Security

### Management Interface

| Protocol | Port | Purpose |
|----------|------|----------|
| HTTPS | 443 | Firewall Management |
| SSH | 22 | CLI Management |

---

### Trust Network

- Internal Communication
- Windows Server Access
- Protected by Palo Alto Firewall

---

### Untrust Network

- Internet Access
- Public Connectivity
- NAT Configuration

---

# 🔄 Deployment Workflow

```
Terraform

    │

terraform init

    │

terraform plan

    │

terraform apply

    │

Create VPC

    │

Create Subnets

    │

Deploy Security Groups

    │

Deploy Network Interfaces

    │

Deploy Palo Alto Firewall

    │

Assign Elastic IPs

    │

Deploy Windows Server

    │

Configure Route Tables

    │

Terraform Outputs
```

---

# ✅ Prerequisites

- AWS Account
- Terraform v1.5 or later
- AWS CLI Configured
- EC2 Key Pair
- Palo Alto VM-Series AMI
- Visual Studio Code (Recommended)

---

# 📥 Deployment

## Clone Repository

```bash
git clone https://github.com/<your-github-username>/terraform-paloalto-aws.git

cd terraform-paloalto-aws
```

---

## Configure Variables

Rename:

```
terraform.tfvars.example
```

to

```
terraform.tfvars
```

Update the following values:

- AWS Region
- AWS Access Key
- AWS Secret Key
- EC2 Key Pair
- Palo Alto AMI ID
- Your Public IP

---

## Initialize Terraform

```bash
terraform init
```

---

## Validate Configuration

```bash
terraform validate
```

---

## Preview Deployment

```bash
terraform plan
```

---

## Deploy Infrastructure

```bash
terraform apply
```

Type:

```
yes
```

when prompted.

---

# 📤 Terraform Outputs

After deployment Terraform displays:

- Firewall Management Public IP
- Firewall Untrust Public IP
- Windows Server Private IP

Example:

```
management_public_ip = xx.xx.xx.xx

untrust_public_ip = xx.xx.xx.xx

windows_private_ip = 10.3.2.10
```

---

# 🌐 Access

## Firewall GUI

```
https://<management_public_ip>
```

---

## Firewall SSH

```bash
ssh admin@<management_public_ip>
```

---

## Windows RDP

Use:

- Firewall Untrust Public IP
- Administrator
- Retrieved Windows Password

---

# 🛠 Useful Terraform Commands

Initialize

```bash
terraform init
```

Validate

```bash
terraform validate
```

Plan

```bash
terraform plan
```

Apply

```bash
terraform apply
```

Destroy

```bash
terraform destroy
```

---

# 📚 Skills Demonstrated

- Terraform
- Infrastructure as Code (IaC)
- AWS EC2
- AWS VPC
- AWS Networking
- AWS Route Tables
- AWS Security Groups
- Elastic IP
- Palo Alto VM-Series
- Cloud Security
- Network Security
- Infrastructure Automation

---

# 🚀 Future Enhancements

- Panorama Integration
- GlobalProtect VPN
- AWS Transit Gateway
- High Availability (HA)
- Auto Scaling
- Bootstrap Automation
- GitHub Actions CI/CD
- Remote Terraform State (S3 + DynamoDB)
- CloudWatch Monitoring
- IAM Roles

---

# 🧰 Technologies Used

| Technology | Purpose |
|------------|---------|
| Terraform | Infrastructure as Code |
| AWS | Cloud Platform |
| Palo Alto VM-Series | Next-Generation Firewall |
| Windows Server 2022 | Compute Instance |
| Git | Version Control |
| GitHub | Repository Hosting |
| VS Code | Development Environment |

---

# 📄 License

This project is intended for learning, testing, and demonstration purposes.

Please ensure you have a valid **Palo Alto VM-Series license** before using the firewall in a production environment.

---

# 👤 Author

## Sadam Uysen

**Senior Cyber Security Specialist**

**AWS • Azure • Palo Alto • Prisma Cloud • Terraform • Network Security • Cloud Security • DevSecOps**

---

⭐ If you found this project useful, consider giving it a **Star** on GitHub.
