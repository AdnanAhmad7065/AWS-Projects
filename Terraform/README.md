# 🌩️ AWS Infrastructure Automation using Terraform

This project automates the setup of a complete AWS infrastructure using **Terraform**. It provisions a **custom VPC**, **public subnets**, **EC2 instances**, and an **Application Load Balancer (ALB)** using infrastructure-as-code principles.

---

## ✅ Step-by-Step Practical Setup

### 🔹 Step 1: Configure AWS CLI on Windows using MobaXterm
1. Open **MobaXterm** terminal.
2. Run the command:
    aws configure

Enter the following when prompted:

AWS Access Key ID

AWS Secret Access Key

Default Region (e.g., us-east-1)

Default Output Format (e.g., json)

This sets up your local credentials to allow Terraform to interact with AWS.

### Step 2: Install and Set Up Terraform (Windows)
Download Terraform binary from: https://developer.hashicorp.com/terraform/downloads

Extract the ZIP and place terraform.exe in a dedicated folder (e.g., C:\Terraform).

Add this folder to your System Environment Variables (PATH) so it can be accessed globally.

Verify installation:

terraform -version

### Step 3: Write Terraform Code in VS Code
Open VS Code and create a new folder (e.g., aws-terraform-infra).

Create the following files:

main.tf – Main infrastructure (VPC, Subnets, EC2, ALB)

provider.tf – AWS provider configuration

variables.tf – Input variables

userdata.sh – Bootstraps EC2 with Apache and HTML

userdata1.sh – Same as above, but with different welcome text

All these files are available in this repo with detailed examples.

### Step 4: Initialize, Plan, and Apply with Terraform
In VS Code terminal (or MobaXterm, inside your project directory):

Initialize Terraform (downloads AWS provider):

terraform init
Preview infrastructure:

terraform plan
Deploy infrastructure:

terraform apply
Type yes when prompted.

(Optional) Destroy everything:

terraform destroy

📦 What This Project Creates:-

✅ A custom VPC

✅ Two public subnets (in different AZs)

✅ Internet Gateway and Route Tables

✅ Security Group with HTTP/SSH access

✅ Two EC2 Instances auto-configured with Apache

✅ An Application Load Balancer (ALB)

✅ A Target Group and HTTP Listener

✅ An S3 Bucket (placeholder for future use)

📁 Terraform Files Explained

File	                            Purpose
provider.tf  	 Connects Terraform to your AWS account using the configured region.
main.tf       Core of the project – defines VPC, EC2 instances, ALB, security, and more.
variables.tf	 Allows dynamic inputs (e.g., VPC CIDR block) instead of hardcoding.
userdata.sh  	 Automatically configures web server on EC2 (Welcome page + Apache).
userdata1.sh	 Same as above, with a slightly different message for 2nd instance.

🧠 Why User Data Scripts?

These scripts make your EC2 instances "ready-to-serve" immediately after boot:

Install Apache2 web server

Enable it to start on boot

Create a simple animated HTML page with the instance ID

(Optionally) Pull images from S3 (commented out in the script)

⚙️ Prerequisites

AWS Account (with access keys)

Terraform installed on Windows

AWS CLI configured

VS Code or any text editor

MobaXterm (for CLI configuration if you’re a Windows user)

### Result
After running terraform apply, Terraform provisions and configures:

Fully working VPC with two web servers

Load-balanced environment

Web page hosted on each EC2 instance

Access via Load Balancer DNS name

🎯 Why Use Terraform?
✅ Infrastructure as Code (IaC) – automated, repeatable infra creation

✅ Version-controlled – easily track changes

✅ Reusable – easy to modify and reapply

✅ Safe – preview changes before applying

📌 Future Enhancements
Add Auto Scaling Group (ASG)

Integrate RDS database

Serve images from S3

Setup monitoring with CloudWatch

Feel free to fork or clone the project, and connect with me on LinkedIn if you're also learning Terraform or DevOps! 🚀

















