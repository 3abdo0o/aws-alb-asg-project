# 🚀 Scalable Web Application on AWS (ALB + Auto Scaling)

## 📌 Project Overview
Deploy a simple, scalable, and highly available web application on AWS using:
- EC2 (web servers)
- Application Load Balancer (ALB)
- Auto Scaling Group (ASG)
- (Optional) Amazon RDS for the database
- CloudWatch + SNS for monitoring

This project is done **manually from AWS Console (no Terraform)** to practice AWS fundamentals.

---

## 🛠 Architecture
![Architecture Diagram](images/architecture.png)

---

## 1️⃣ Create VPC & Networking
- Create a new VPC (e.g., `10.0.0.0/16`).
- Create **2 public subnets** in different AZs.
- Create **Internet Gateway** and attach to VPC.
- Add **Route Table** to allow internet access.


---

## 2️⃣ Security Groups
- `web-sg`: Allow **HTTP (80)** from `0.0.0.0/0`, **SSH (22)** only from your IP.
- `alb-sg`: Allow HTTP (80) from `0.0.0.0/0`, forward to `web-sg`.

📷 Screenshot:  
aws-alb-asg-project\Auto Scaling Group\1.png
aws-alb-asg-project\Auto Scaling Group\2.png
aws-alb-asg-project\Auto Scaling Group\3.png
aws-alb-asg-project\Auto Scaling Group\4.png
aws-alb-asg-project\Auto Scaling Group\5.png
aws-alb-asg-project\Auto Scaling Group\6.png
aws-alb-asg-project\Auto Scaling Group\7.png
aws-alb-asg-project\Auto Scaling Group\8.png
aws-alb-asg-project\Auto Scaling Group\9.png
aws-alb-asg-project\Auto Scaling Group\10.png
aws-alb-asg-project\Auto Scaling Group\11.png

---

## 3️⃣ Launch Template
- AMI: **Amazon Linux 2023**  
- Instance type: `t2.micro`  
- User data script:

```bash
#!/bin/bash
yum update -y
amazon-linux-extras enable nginx1
yum install -y nginx
systemctl start nginx
systemctl enable nginx
echo "<h1>Hello from $(hostname)</h1>" > /usr/share/nginx/html/index.html
