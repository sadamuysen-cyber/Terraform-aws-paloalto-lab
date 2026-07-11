# Terraform-aws-paloalto-lab
Palo Alto Networks AWS Terraform Lab is an automated hands-on environment designed to test, validate, and deploy Palo Alto VM-Series Virtual Firewalls or Cloud Next-Generation Firewalls (NGFW) inside Amazon Web Services (AWS) using Infrastructure as Code (IaC)

# AWS Palo Alto VM-Series Firewall & Windows Server Deployment using Terraform

## 📖 Overview

This project demonstrates how to deploy a **Palo Alto VM-Series Next-Generation Firewall** and a **Windows Server 2022 EC2 instance** in **Amazon Web Services (AWS)** using **Terraform**.

Deploys a Palo Alto VM-Series firewall and a Windows Server 2022 VM in Amazon Web Services (AWS) with management access for the firewall and RDP/internet access for the Windows VM.

The infrastructure is deployed using Infrastructure as Code (IaC) best practices, creating networking, routing, security groups, Elastic IPs, firewall interfaces, and compute resources automatically.

---
## Files

The project is split into multiple files to illustrate modularity and keep separate constructs distinct, making it easier to manage and understand.

- **main.tf**: Terraform provider block (hashicorp/aws) and version requirements.
- **awsprovider.tf**: AWS provider config with access_key, secret_key, and region.
- **variables.tf**: Variables for region, firewall AMI, instance types, etc.
- **terraform.tfvars.template**: Template for sensitive/custom values; rename to **terraform.tfvars** and add your credentials.
- **locals.tf**: Local variables for naming conventions.
- **aws-networking.tf**: VPC, management, untrust, and trust subnets, and internet gateway.
- **securitygroup.tf**: Security groups for firewall management (HTTPS/SSH), untrust (all traffic), and trust (all traffic).
- **routing-static.tf**: Route tables for internet access (management/untrust) and internal routing via firewall trust NIC.
- **firewall.tf**: Palo Alto firewall with management, untrust, and trust NICs, Elastic IPs, and public IP outputs.
- **windows.tf**: Windows VM, outputs private IP.

---

## How It Works

- **Networking:** VPC with management, untrust, and trust subnets provides connectivity. The management and untrust subnets use a public route table for internet access, while the trust subnet routes traffic through the firewall’s trust NIC.
- **Security:** Firewall management security group allows HTTPS (443) and SSH (22) from your specified IP. Untrust and trust security groups allow all traffic.
- **Firewall:** Palo Alto VM-Series instance with three NICs:
  - Management (10.3.0.10) for HTTPS/SSH access
  - Untrust (10.3.1.10) for external traffic
  - Trust (10.3.2.10) for internal traffic

  Elastic IPs are assigned to the management and untrust NICs for public access.

- **Instance:** Windows Server 2022 VM in the trust subnet with no public IP. The firewall is disabled via user data for internal access.

---

## Prerequisites

- An AWS account with a key pair for EC2 instances.
- AWS credentials including **access_key**, **secret_key**, and **region**.
- A valid Palo Alto VM-Series AMI ID for your region (specified in **firewall_ami**).
- Terraform installed on your machine.
- Examples are demonstrated using Visual Studio Code (VS Code).

---

## Deployment Steps

1. Update **terraform.tfvars** with AWS credentials, key pair name in **key_name**, your public IP in **my_public_ip**, and a valid Palo Alto AMI ID in **firewall_ami** if different from the default.

2. Run:
   ```bash
   terraform init
   terraform plan
   terraform apply
   ```
   Type **yes** when prompted.

3. Get the firewall management public IP from the **management_public_ip** output shown after deployment, or run:

   ```bash
   terraform output management_public_ip
   ```

4. Use SSH to connect to the firewall management interface using the public IP and your key pair. Change the admin password by entering:

   ```
   configure
   set mgt-config users admin password
   commit
   ```

5. Use HTTPS to connect to the firewall management interface:

   ```
   https://<management_public_ip>
   ```

6. Update **MY-PUBLIC-IP** in **firewall-config.xml**:
   - Replace **5.5.5.5/32** with your actual public IP (same as **my_public_ip** in **terraform.tfvars**).
   - Save the XML file.

7. Import the XML configuration via the GUI:
   - Log in using:
     - Username: **fwadmin**
     - Password: **firewall_admin_password** (from terraform.tfvars)
   - Navigate to:
     ```
     Device > Setup > Operations
     ```
   - Import Named Configuration Snapshot.
   - Upload **firewall-config.xml**.
   - Load the imported configuration.
   - Commit the configuration.

   **Note:** After the commit, the admin password will be reset to:

   ```
   2Plus2cabbage!
   ```

8. Retrieve the Windows VM Administrator password:

   ```
   AWS Console
   → EC2
   → Instances
   → Select Windows VM
   → Actions
   → Security
   → Get Windows Password
   ```

9. Use Remote Desktop to connect to the firewall's untrust public IP:

   ```
   <untrust_public_ip>
   ```

   Login with:
   - Username: **Administrator**
   - Password: Retrieved from AWS

10. To remove all resources:

```bash
terraform destroy
```

Type **yes** when prompted.

---

## Potential Costs and Licensing

- The resources deployed using this Terraform configuration should generally incur minimal to no costs, provided they are terminated promptly after creation.
- The Palo Alto VM-Series firewall requires a valid license from Palo Alto Networks, which may incur additional costs. Ensure you have the appropriate licensing for the VM-Series AMI used in **firewall_ami**.
- It is important to fully understand your cloud provider's billing structure, trial periods, and any potential costs associated with deploying resources in public cloud environments.
- You are responsible for any applicable software licensing or other charges that may arise from deploying and using these resources.
  
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
