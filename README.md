# 🚀 Scalable Web Application on AWS (ALB + Auto Scaling)

## 📌 Project Overview
Deploy a simple, scalable, and highly available web application on AWS using:
- EC2 (web servers)
- Application Load Balancer (ALB)
- Auto Scaling Group (ASG)
- Amazon RDS for the database
- CloudWatch + SNS for monitoring

This project is done **manually from AWS Console (no Terraform)** to practice AWS fundamentals.

---

## 🛠 Architecture
Architecture Diagram

https://i.postimg.cc/SRCDb2Rd/1-8-Fwhw-NQc7977-FCg-W7-TLwo-Q.webp
 ![image alt]([image_url](https://i.postimg.cc/SRCDb2Rd/1-8-Fwhw-NQc7977-FCg-W7-TLwo-Q.webp
)) 

![Image](https://github.com/user-attachments/assets/aa184ebe-f10b-4282-b0d8-a140c9f3e064)

---

Create VPC & Networking
- Create a new VPC (e.g., `10.0.0.0/16`).
- Create **2 public subnets** in different AZs.
- Create **Internet Gateway** and attach to VPC.
- Add **Route Table** to allow internet access.


---
## Launch EC2
https://i.postimg.cc/L681pFrF/1.png

https://i.postimg.cc/33bg3W8S/2.png

https://i.postimg.cc/c007mVkm/3.png

https://i.postimg.cc/VzNqyX80/4.png

https://i.postimg.cc/rwdRDCVh/5.png

https://i.postimg.cc/mD4PpvQq/6.png

https://i.postimg.cc/x8TcnLp3/7.png

https://i.postimg.cc/76NfFK9L/8.png

https://i.postimg.cc/2yc363PR/9.png

https://i.postimg.cc/kMf2whD1/10.png

---


Security Groups
- `web-sg`: Allow **HTTP (80)** from `0.0.0.0/0`, **SSH (22)** only from your IP.
- `alb-sg`: Allow HTTP (80) from `0.0.0.0/0`, forward to `web-sg`.
- 
https://i.postimg.cc/3Jf30sW-h/1.png

https://i.postimg.cc/YSY7VjR2/2.png

https://i.postimg.cc/Xq70pLbX/3.png

https://i.postimg.cc/KzbbyFpP/4.png

https://i.postimg.cc/fLvNzF16/5.png

https://i.postimg.cc/6qjxRZWc/6.png

https://i.postimg.cc/QMrwFmgs/7.png

https://i.postimg.cc/zGh4D6g0/8.png

https://i.postimg.cc/vBYKzmSN/9.png

https://i.postimg.cc/5tqGLzpz/10.png

https://i.postimg.cc/Zn0DGMmP/11.png

---

Launch Template
- AMI: **Amazon Linux 2023**  
- Instance type: `t2.micro`  
- User data script:


---
Application Load Balancer
Console path: EC2 → Load Balancers → Create load balancer → Application Load Balancer

Name: proj1-alb

Scheme: Internet-facing

IP address type: IPv4

VPC: proj1-vpc

Mappings (Subnets): 2 Public subnets in 2 AZs 

Security groups: proj1-alb-sg

Listeners and routing:

Listener: HTTP : 80 → Default action: Forward to proj1-tg

https://i.postimg.cc/CSXCbz83/1.png

https://i.postimg.cc/6WWCBRsv/2.png

https://i.postimg.cc/FK9kst64/3.png

https://i.postimg.cc/R0X6Wsrn/4.png

https://i.postimg.cc/nrQCwnXv/5.png
---
Auto Scaling Group
Console path: EC2 → Auto Scaling Groups → Create Auto Scaling group

Name: proj1-asg

Launch template: proj1-ec2-lt

VPC: proj1-vpc

Subnets: 2 Public subnets 

Load balancing:

Attach to an existing load balancer → Application Load Balancer proj1-alb

Target groups: proj1-tg

Health checks: Enable ELB health checks → Health check grace period: 60 sec

Group size:

Desired capacity: 2

Minimum capacity: 2

Maximum capacity: 4

Scaling policies:

Choose Target tracking scaling policy

Metric type: Average CPU utilization

Target value: 50%

Instance warmup: 180 sec

Monitoring: Enable CloudWatch group metrics collection

https://i.postimg.cc/3Jf30sW-h/1.png

https://i.postimg.cc/YSY7VjR2/2.png

https://i.postimg.cc/Xq70pLbX/3.png

https://i.postimg.cc/KzbbyFpP/4.png

https://i.postimg.cc/fLvNzF16/5.png

https://i.postimg.cc/6qjxRZWc/6.png

https://i.postimg.cc/QMrwFmgs/7.png

https://i.postimg.cc/zGh4D6g0/8.png

https://i.postimg.cc/vBYKzmSN/9.png

https://i.postimg.cc/JR9Lwckp/10.png

https://i.postimg.cc/Zn0DGMmP/11.png

```
RDS
https://i.postimg.cc/FRS2z689/1.png

https://i.postimg.cc/1XZbqH1D/2.png

https://i.postimg.cc/tgs0QJzn/3.png

https://i.postimg.cc/1z2k9SW-C/4.png

https://i.postimg.cc/GLZRRFVL/5.png

https://i.postimg.cc/BZxrGd2p/6.png

https://i.postimg.cc/fTC1Vm20/7.png

https://i.postimg.cc/cL5jmZNx/8.png

https://i.postimg.cc/3JYPrnBT/9.png

https://i.postimg.cc/wjBZLMZ4/10.png



```
CloudWatch + SNS 

https://i.postimg.cc/PqTLSGb7/1.png

https://i.postimg.cc/zBfy1ncS/2.png

https://i.postimg.cc/W1RqJqdT/3.png

https://i.postimg.cc/5tgXGPn2/4.png

https://i.postimg.cc/sX0vfMFF/5.png

https://i.postimg.cc/yxgWD7BD/6.png

https://i.postimg.cc/WzBzrxbX/7.png

https://i.postimg.cc/xjhcWNyM/8.png

https://i.postimg.cc/BQFXsZMy/9.png


```
bash
#!/bin/bash
yum update -y
amazon-linux-extras enable nginx1
yum install -y nginx
systemctl start nginx
systemctl enable nginx
echo "<h1>Hello from $(hostname)</h1>" > /usr/share/nginx/html/index.html
