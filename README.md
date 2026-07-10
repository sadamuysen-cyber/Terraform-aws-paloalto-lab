# Terraform-aws-paloalto-lab
Palo Alto Networks AWS Terraform Lab is an automated hands-on environment designed to test, validate, and deploy Palo Alto VM-Series Virtual Firewalls or Cloud Next-Generation Firewalls (NGFW) inside Amazon Web Services (AWS) using Infrastructure as Code (IaC)

AWS Palo Alto VM-Series Firewall & Windows Server Deployment using Terraform
Project Overview

This project automates the deployment of a secure AWS infrastructure using Terraform, including a Palo Alto VM-Series Next-Generation Firewall and a Windows Server 2022 EC2 instance.

The deployment demonstrates Infrastructure as Code (IaC) principles by automatically provisioning networking, security groups, routing, firewall interfaces, Elastic IPs, and compute resources.

# Features
Automated AWS Infrastructure Deployment
Palo Alto VM-Series NGFW
Windows Server 2022 Instance
Infrastructure as Code (Terraform)
Multi-NIC Firewall Deployment
Secure Management Access
Elastic IP Assignment
Custom Route Tables
Modular Terraform Code
Production-Ready Project Structure

# Architecture
                Internet
                    |
          +-------------------+
          | Internet Gateway  |
          +-------------------+
                    |
        --------------------------
        |                        |
 Management Subnet         Untrust Subnet
     (eth0)                   (eth1)
        |                        |
        +------ Palo Alto -------+
                 Firewall
                   |
              Trust Interface
                   |
             Trust Subnet
                   |
         Windows Server 2022
Project Structure
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
└── README.md
Components
Networking
Custom VPC
Internet Gateway
Three Subnets
Management
Untrust
Trust
Route Tables
Network Interfaces
Palo Alto Firewall
Management Interface
Untrust Interface
Trust Interface
Elastic IP for Management
Elastic IP for Untrust
SSH Access
HTTPS Management
Bootstrap Ready
Windows Server
Windows Server 2022
Private Deployment
RDP through Firewall
User Data Configuration
Internal Communication
Security

Management Interface

Protocol	Port	Purpose
HTTPS	443	Firewall Management
SSH	22	CLI Management

Trust Network

Internal Communication
Windows Server Access
Firewall Protected

Untrust Network

Internet Connectivity
Public Services
NAT Configuration
Deployment Workflow
Terraform

↓

Create VPC

↓

Create Subnets

↓

Deploy Security Groups

↓

Deploy Network Interfaces

↓

Deploy Palo Alto Firewall

↓

Assign Elastic IPs

↓

Deploy Windows Server

↓

Configure Routing

↓

Output Management IP
Prerequisites
AWS Account
Terraform 1.5+
AWS CLI Configured
EC2 Key Pair
Palo Alto VM-Series AMI
Visual Studio Code (Optional)
Deployment

Initialize Terraform

terraform init

Validate

terraform validate

Plan

terraform plan

Deploy

terraform apply

Destroy

terraform destroy
Outputs

Terraform displays:

Firewall Management Public IP
Firewall Untrust Public IP
Windows Private IP
Skills Demonstrated
Terraform
AWS EC2
AWS VPC
AWS Route Tables
AWS Security Groups
Elastic IP
Palo Alto VM-Series
Infrastructure as Code
Network Security
Cloud Networking
Future Enhancements
Panorama Integration
GlobalProtect VPN
AWS Transit Gateway
Auto Scaling
High Availability Firewall
Bootstrap Automation
CloudWatch Monitoring
IAM Role-Based Access
GitHub Actions CI/CD
Terraform Modules
Remote Terraform State (S3 + DynamoDB)
Technologies Used
Terraform
AWS
Palo Alto VM-Series
Windows Server 2022
Visual Studio Code
Git
GitHub
Author


To make this repository clearly your own, I also recommend:
Rename files (provider.tf instead of awsprovider.tf, network.tf instead of aws-networking.tf).
Add an architecture diagram (draw.io or Visio).
Include screenshots of the deployed AWS resources and Palo Alto GUI.
Add a LICENSE file and a .gitignore.
Add sample Terraform outputs and a troubleshooting section.
Use your own comments and formatting in the Terraform files rather than copying another repository verbatim.

These changes help demonstrate your own implementation and presentation of the project rather than simply mirroring an existing repository.


Sadam Uysen
Senior Cyber Security Specialist
Cloud | AWS | Azure | Palo Alto | Prisma Cloud | Terraform | Network Security | DevSecOps
