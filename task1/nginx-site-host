1. Overview

This document outlines the steps to create a Virtual Private Cloud (VPC) in AWS with one public subnet,
launch an Ubuntu EC2 instance, install Nginx, configure SSL termination using Let's Encrypt, set up a document root
with an EBS volume, and create a snapshot and restore process.

2. Prerequisites
- AWS Account with appropriate permissions.
- SSH access to EC2 instances.
- A key pair for SSH access to EC2.
- Access to AWS Management Console.

3. Step-by-Step Setup

3.1. Create a VPC with a Public Subnet
1. Navigate to the VPC Dashboard:
   - Go to VPC > Create VPC.
   - Configure the following:
     - CIDR block: 10.0.0.0/16
     - Name tag: MyVPC
     - Tenancy: Default.
2. Create Subnet:
   - Go to Subnets > Create subnet.
   - Select your VPC (MyVPC).
   - Provide a Subnet Name (e.g., MyPublicSubnet).
   - Choose an Availability Zone (e.g., us-east-1a).
   - Set CIDR Block to 10.0.1.0/24.

3.2. Create and Attach an Internet Gateway
1. Navigate to the Internet Gateway section:
   - Go to VPC > Internet Gateways > Create Internet Gateway.
   - Provide a name and click Create.
2. Attach the Internet Gateway to Your VPC:
   - After creating the IGW, select it and click Attach to VPC.
   - Select MyVPC and click Attach.

3.3. Update Route Table
1. Modify the Route Table:
   - Go to Route Tables and select the default route table.
   - Under Routes, click Edit Routes and add a route:
     - Destination: 0.0.0.0/0
     - Target: Select your Internet Gateway.
2. Associate Route Table with Subnet:
   - In the Subnet Associations tab, associate the route table with MyPublicSubnet.

3.4. Launch EC2 Instance
1. Navigate to EC2:
   - Go to EC2 Dashboard > Launch Instance.
2. Select Ubuntu AMI:
   - Choose the Ubuntu AMI.
3. Select Instance Type:
   - Select t2.micro or another appropriate instance type.
4. Configure Instance:
   - Choose the VPC and subnet (MyPublicSubnet).
5. Launch the Instance:
   - Complete the setup by selecting an existing key pair or creating a new one.

3.5. Install Nginx on EC2
1. SSH into the EC2 Instance:
   - Open a terminal and connect:
     ssh -i "your-key.pem" ubuntu@<Public-IP>
2. Install Nginx:
   - Run the following commands:
     sudo apt update
     sudo apt install nginx -y
   - Start Nginx:
     sudo systemctl start nginx

3.6. Configure SSL Termination with Lets Encrypt
1. Install Certbot:
   - Run the following commands:
     sudo apt install certbot python3-certbot-nginx -y
2. Obtain and Install SSL Certificate:
   - Run:
     sudo certbot --nginx
   - Follow the prompts to configure SSL for your site.

3.8. Create an EBS Volume and Attach to EC2
1. Navigate to EBS Volumes:
   - Go to EC2 > Volumes > Create Volume.
   - Configure the volume size and type (e.g., 10GB, SSD).
2. Attach the Volume:
   - After creating the volume, click Actions > Attach Volume and select the EC2 instance.
3. SSH into the Instance and Mount the Volume:
   - Connect to your instance and run:
     sudo mkdir /mnt/data
     sudo mount /dev/xvdf /mnt/data
4. Update Nginx to Use New Document Root:
   - Edit the Nginx configuration:
     sudo nano /etc/nginx/sites-available/default
   - Change the root directive to:
     root /mnt/data;

3.9. Create Snapshot of the EBS Volume
1. Take Snapshot:
   - In the EC2 Dashboard, go to Volumes, select the volume, and click Actions > Create Snapshot.
   - Provide a description for the snapshot.

3.10. Unmount and Restore EBS Volume
1. Unmount the Volume:
   - Run:
     sudo umount /mnt/data
2. Restore from Snapshot:
   - In EC2 Dashboard, go to Snapshots, select the snapshot, and click Actions > Create Volume.
   - Choose the same Availability Zone as your EC2 instance.
3. Attach the New Volume:
   - Attach the newly created volume to the EC2 instance.
4. Mount the Volume:
   - SSH into the instance and run:
     sudo mount /dev/xvdf /mnt/data
