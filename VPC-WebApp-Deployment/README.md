# 🛠️ AWS VPC Web App Deployment Project

This project demonstrates how to design and deploy a secure and scalable **web application infrastructure** using **AWS VPC**, **EC2**, **Auto Scaling**, and **Application Load Balancer**. A simple Python-based web app is hosted on private EC2 instances and accessed securely via a public load balancer.

---

## 📊 Architecture Diagram

![Architecture Diagram](architecture-diagram.png)

> *(Make sure this image is added to your GitHub repo with the same file name)*

---

## 🚀 Project Features

- Multi-AZ VPC setup with public and private subnets
- NAT Gateways for internet access from private subnets
- Bastion Host (Jump Server) for secure SSH into private EC2s
- Auto Scaling Group for EC2 instances in private subnets
- Application Load Balancer in public subnet
- Simple Python Web App hosted using `python -m http.server`
- Secure key handling using SCP and MobaXterm (Windows)

---

## 🧱 Step-by-Step Setup

### 1. 🔧 VPC and Network Setup

- Create a **VPC** with CIDR block (e.g., `10.0.0.0/16`)
- Add **2 Availability Zones**
- Create the following subnets:
  - 2 **Public Subnets**
  - 2 **Private Subnets**
- Attach **Internet Gateway** to VPC for outbound access from public subnets
- Set up **NAT Gateway** in each public subnet for private subnet internet access
- Configure **Route Tables** and associate with subnets accordingly
- Create **Security Groups**:
  - Bastion Host SG (Allow SSH from your IP)
  - Private EC2 SG (Allow SSH from Bastion Host SG, HTTP for Python app)
  - ALB SG (Allow HTTP from anywhere)

---

### 2. 🏗️ EC2 + Auto Scaling Group

- Create a **Launch Template**:
  - AMI: Amazon Linux 2
  - Instance type: t2.micro
  - Key pair: Your existing key
  - User data (optional): Install Python or Apache
- Create an **Auto Scaling Group**:
  - Launch Template: Use the one created above
  - Subnets: **Private subnets only**
  - Desired Capacity: `2`
  - Min/Max Capacity: `1` and `3` (or as per need)
  - Attach Load Balancer later

---

### 3. 🔐 Bastion Host Setup (Jump Server)

- Launch an EC2 instance (Amazon Linux 2) in one **public subnet**
- Assign **Elastic IP** for stable access
- Allow inbound **SSH (port 22)** from your IP in its Security Group
- From **MobaXterm (for Windows)**:
  - **Upload your `.pem` key** to Bastion Host:
    ```bash
    scp -i your-key.pem your-key.pem ec2-user@<Bastion_Public_IP>:/home/ec2-user/
    ```
  - SSH into Bastion Host:
    ```bash
    ssh -i your-key.pem ec2-user@<Bastion_Public_IP>
    ```
  - Then from Bastion Host, SSH into Private EC2 using **private IP**:
    ```bash
    ssh -i your-key.pem ec2-user@<Private_EC2_IP>
    ```

---

### 4. 🌐 Deploy Python Web App on Private EC2

Once inside the **Private EC2 Instance**:

#### 🧾 Option 1: Create HTML + Serve Using Python
```bash
echo "<html><body><h1>Hello from my Python Web App!</h1></body></html>" > index.html
python3 -m http.server 8000

### 5. 📌 Application Load Balancer Setup

Now expose your private EC2s to the internet securely using an Application Load Balancer.

🔹 Steps:

➡️ Go to **EC2 → Load Balancers → Create Load Balancer**

➡️ Choose **Application Load Balancer**

- Name: `my-app-lb`
- Scheme: `Internet-facing`
- Listeners: Add `HTTP` on port `80`
- Availability Zones: Select **both public subnets**
- Security Group: Allow port **80 (HTTP)** from **anywhere**

🎯 Create Target Group:

- Name: `my-app-target-group`
- Target type: `Instance`
- Protocol: `HTTP`
- Port: `8000`

➡️ Register your EC2 instances (launched via Terraform) to this target group

➡️ Once created, copy the **DNS name of the ALB** and paste it into your browser — your Python app should be accessible now!



Copy
Edit
http://<Your-ALB-DNS-Name>
You should see your Python web app running successfully!
