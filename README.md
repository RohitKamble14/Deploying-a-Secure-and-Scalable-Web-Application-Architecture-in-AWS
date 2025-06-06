Deploying a Secure and Scalable Web Application Architecture in AWS
Project Overview
This project demonstrates how to deploy a secure and scalable web application in AWS using a Virtual Private Cloud (VPC) with public and private subnets, auto-scaling groups, an Application Load Balancer, NAT gateways, and a bastion host for secure access.
Table of Contents

Step-by-Step Guide
Step 1: Create the VPC with Public and Private Subnets
Step 2: Configure Auto-Scaling Group and Launch Template
Step 3: Set Up the Bastion Host
Step 4: Deploy the Application
Step 5: Create the Load Balancer
Step 6: Test the Setup
Step 7: Access Load Balancer Using Public URL (Route 53)


Tools and Services Used
AWS Services
Tools


Author

Step-by-Step Guide
Step 1: Create the VPC with Public and Private Subnets

Navigate to the VPC Dashboard:

Open the AWS Management Console.
Search for and select "VPC."


Create a VPC:

Choose "Create VPC and More."
Configure:
VPC Name: AWS-Project
IPv4 CIDR Block: Use default (e.g., 10.0.0.0/16)
Availability Zones: Select two (e.g., us-east-1a, us-east-1b)
Public Subnets: Two public subnets in each availability zone
Private Subnets: Two private subnets in each availability zone




Set Routing for Subnets:

Attach an Internet Gateway to the VPC.
Associate public subnets with a route table that includes the Internet Gateway.
Create a NAT Gateway in one public subnet and associate private subnets with a route table configured to use the NAT Gateway.



Step 2: Configure Auto-Scaling Group and Launch Template

Create a Launch Template:

Navigate to the EC2 dashboard and select "Launch Templates."
Configure:
Name: Project-Launch-Template
AMI: Choose Ubuntu (or your preferred OS)
Instance Type: t2.micro
Key Pair: Select or create a key pair
Security Group:
Allow SSH (Port 22) and application traffic (e.g., Port 80)
Associate with the VPC






Create an Auto-Scaling Group:

Go to "Auto-Scaling Groups."
Configure:
Attach the launch template
Select the private subnets in both availability zones
Set:
Desired Capacity: 2
Minimum Instances: 1
Maximum Instances: 4







Step 3: Set Up the Bastion Host

Launch an EC2 Instance:

Choose t2.micro instance type with Ubuntu AMI.
Place the instance in a public subnet.
Assign a public IP.


Configure Security Group:

Allow SSH (Port 22) access from your IP address.


SSH into the Bastion Host:

Use the key pair to SSH:ssh -i <Your-Key.pem> ubuntu@<Bastion-Host-Public-IP>




Prepare for Private Instance Access:

Transfer the private key to the Bastion Host:scp -i <Your-Key.pem> <Your-Key.pem> ubuntu@<Bastion-Host-Public-IP>:~/





Step 4: Deploy the Application

SSH into Private Instances via the Bastion Host:

From the Bastion Host:ssh -i <Your-Key.pem> ubuntu@<Private-Instance-Private-IP>




Install a Simple HTTP Server:
sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2


Host a Sample Website:

Place an index.html file in the server’s root directory (e.g., /var/www/html for Apache).



Step 5: Create the Load Balancer

Navigate to the Load Balancer Dashboard:

In the EC2 dashboard, choose "Load Balancers."


Create an Application Load Balancer:

Configure:
Name: AWS-Project-ALB
Type: Internet-facing
Subnets: Choose the public subnets
Security Group: Allow HTTP (Port 80)




Set Up Target Groups:

Create a target group and attach private instances on Port 8000.


Register Targets:

Verify both instances are healthy.



Step 6: Test the Setup

Access the Load Balancer:

Copy the Load Balancer’s DNS name.
Open it in a browser:http://<Load-Balancer-DNS>


You should see the content from the deployed application.


Test Load Balancing:

Deploy a different application on the second instance.
Verify traffic alternates between the two applications.



Step 7: Access Load Balancer Using Public URL (Route 53)

Objective: Map a custom domain to the load balancer using Route 53.
Register a domain name in Hostinger (or use an existing one).
Create a hosted zone for the domain in Route 53.
Add an A record (Alias) in the hosted zone that points to the load balancer’s DNS name.
Verify that the custom domain resolves to the load balancer and loads the web application.



Tools and Services Used
AWS Services

VPC: Isolated network for resources.
Subnets: Public (for Load Balancer, NAT Gateway) and Private (for application servers).
Internet Gateway: Enables internet access for public resources.
NAT Gateway: Allows secure internet access for private resources.
Route Tables: Manages traffic routing.
Security Groups: Firewall rules for resource access.
Bastion Host: Secure bridge to access private instances via SSH.
EC2 Instances: Virtual servers for the application.
Auto-Scaling Group: Automatically adjusts the number of instances.
Application Load Balancer: Distributes incoming traffic across instances.

Tools

AWS Management Console: GUI for managing AWS resources.
SSH Client: Access EC2 instances (e.g., Terminal, PuTTY).
SCP: Secure file transfer between local machine and EC2.
Text Editor: Edit files on EC2 (e.g., Nano, Vim).

Author

Rohit Kamble

