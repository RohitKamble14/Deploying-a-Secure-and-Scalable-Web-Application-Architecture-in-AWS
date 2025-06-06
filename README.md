# 🚀 Deploying a Secure and Scalable Web Application Architecture in AWS

This project demonstrates how to design and deploy a secure, high-availability, and scalable web application architecture on **Amazon Web Services (AWS)** using a combination of core infrastructure services such as VPC, EC2, Load Balancer, NAT Gateway, Bastion Host, Auto Scaling, and more.

---

## 🧠 Project Overview

The architecture consists of:

- A **custom VPC** with both public and private subnets.
- **Internet Gateway** for internet access.
- **NAT Gateway** for secure access to the internet from private subnets.
- **Bastion Host** for secure SSH access to private instances.
- **Application Load Balancer** to distribute traffic.
- **Auto Scaling Group (ASG)** for scalability and high availability.

This setup helps meet the demands of secure, scalable, and production-grade infrastructure in the cloud.

---

## 🧩 Architecture Diagram

![VPC Architecture](VPC.jpeg)

---

## 📋 Steps to Deploy

### 🔹 Step 1: Create the VPC with Public and Private Subnets
- Go to the **VPC Dashboard** in AWS.
- Choose **"Create VPC and more"**.
- Configure:
  - VPC Name: `AWS-Project`
  - CIDR: `10.0.0.0/16`
  - Select two Availability Zones (e.g., `us-east-1a`, `us-east-1b`)
  - Create:
    - Two **public subnets**
    - Two **private subnets**
- Set up routing:
  - Attach an **Internet Gateway**
  - Associate route tables:
    - Public Subnet ➝ IGW
    - Private Subnet ➝ NAT Gateway (created in one public subnet)

### 🔹 Step 2: Launch EC2 Instances
- Create:
  - A **Bastion Host** in the public subnet.
  - Web/App servers in private subnets.
- Configure security groups:
  - Bastion Host: allow SSH from your IP.
  - Web servers: allow access from Load Balancer only.

### 🔹 Step 3: Setup Application Load Balancer (ALB)
- Create an **ALB** in public subnets.
- Register EC2 instances in private subnets as targets.
- Configure:
  - Target group
  - Health checks
  - Listener rules (HTTP/HTTPS)

### 🔹 Step 4: Configure Auto Scaling
- Attach EC2 instances to an **Auto Scaling Group (ASG)**.
- Define:
  - Launch template/configuration
  - Scaling policies (based on CPU/memory)
  - Min/Max/Desired instance count

---

## 🔐 Security Best Practices Implemented

- Use of **NAT Gateway** for secure outbound traffic.
- Restriction of SSH access to private instances using a **Bastion Host**.
- Segregation of resources via **public and private subnets**.
- Controlled access using **Security Groups and NACLs**.
- Monitoring via **CloudWatch** (optional for enhancements).

---

## 📦 Technologies Used

| Service         | Purpose                          |
|----------------|----------------------------------|
| VPC            | Isolated network environment     |
| Subnets        | Logical separation of resources  |
| EC2            | Compute resources (instances)    |
| ALB            | Load balancing across instances  |
| ASG            | High availability & scaling      |
| NAT Gateway    | Secure outbound internet access  |
| Internet GW    | Internet access for public subnet|
| Bastion Host   | Secure SSH to private instances  |

---

## 📸 Screenshots

*Add relevant screenshots here for:*
- VPC Setup
- EC2 instances
- Load Balancer config
- ASG scaling configuration
- NAT Gateway
- Security Groups

---

## 🛠️ Improvements (Optional)

- Add **RDS** for managed database setup.
- Use **CloudFormation or Terraform** for IaC.
- Configure **CloudWatch Alarms & SNS** for monitoring.
- Implement **WAF** & **Shield** for extra security.
- Store app content in **S3 + CloudFront** (for static content).

---

## 🧑‍💻 Author

**Rohit Kamble**  
DevOps & Cloud Enthusiast  
GitHub: [Your GitHub Username]  
LinkedIn: [Your LinkedIn Profile]
