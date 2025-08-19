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
## Launch EC2 & Create VPC & Networking

- Create a new VPC (e.g., `10.0.0.0/16`).
- Create **2 public subnets** in different AZs.
- Create **Internet Gateway** and attach to VPC.
- Add **Route Table** to allow internet access.
- Launch EC2

<img width="1680" height="862" alt="Image" src="https://github.com/user-attachments/assets/33d26508-7fb2-44a4-9e3a-c9aa26aeae3d" />

<img width="1680" height="897" alt="Image" src="https://github.com/user-attachments/assets/540e868b-d1bd-42e0-bd2e-ad11e5b8ce3a" />

<img width="1680" height="897" alt="Image" src="https://github.com/user-attachments/assets/722bf740-cd2d-4340-bba1-116467ee0d3f" />

<img width="1680" height="897" alt="Image" src="https://github.com/user-attachments/assets/f9a9d7c7-e66c-4893-b6fc-dbe7c53de290" />

<img width="874" height="192" alt="Image" src="https://github.com/user-attachments/assets/c9fd9e84-fc9f-4f5f-946e-4d477c9b9519" />

<img width="856" height="747" alt="Image" src="https://github.com/user-attachments/assets/4f8f2547-87bd-412c-9606-16602ed4b5bc" />

<img width="880" height="341" alt="Image" src="https://github.com/user-attachments/assets/96344bce-81bf-493d-96f5-4586df16a8e6" />

<img width="874" height="315" alt="Image" src="https://github.com/user-attachments/assets/48355501-0c45-4f12-ab55-c5b1d01c03a5" />

<img width="424" height="508" alt="Image" src="https://github.com/user-attachments/assets/216822e8-45ae-4590-8f4c-e2e9b26f59e1" />

<img width="1374" height="412" alt="Image" src="https://github.com/user-attachments/assets/75b1e686-2e0a-40fb-b329-0e6b74250699" />

---


Security Groups
- `web-sg`: Allow **HTTP (80)** from `0.0.0.0/0`, **SSH (22)** only from your IP.
- `alb-sg`: Allow HTTP (80) from `0.0.0.0/0`, forward to `web-sg`.
--------
Auto Scaling Group

<img width="1391" height="334" alt="Image" src="https://github.com/user-attachments/assets/6b0131ea-ecff-4bec-85f4-725ac0bc48df" />

<img width="1356" height="744" alt="Image" src="https://github.com/user-attachments/assets/7f065896-afc1-4328-95c4-27317d7505a6" />

<img width="1680" height="771" alt="Image" src="https://github.com/user-attachments/assets/1a618ee4-1047-431f-8e48-6b98858a0b4f" />

<img width="1680" height="771" alt="Image" src="https://github.com/user-attachments/assets/d963e564-cd03-4ae8-b498-069d8a7fb7db" />

<img width="1620" height="719" alt="Image" src="https://github.com/user-attachments/assets/f14da0c8-4a19-494d-9b41-8d4a68d3a4cb" />

<img width="1680" height="771" alt="Image" src="https://github.com/user-attachments/assets/eb631e8a-e730-4bf6-a5a7-4593c3dd96d5" />

<img width="1680" height="771" alt="Image" src="https://github.com/user-attachments/assets/6c0f1cb3-a0bb-4974-863e-6f0771fc15fa" />

<img width="1680" height="771" alt="Image" src="https://github.com/user-attachments/assets/5ceae015-4813-4d14-8fda-9bdaa7f75e3f" />

<img width="1680" height="771" alt="Image" src="https://github.com/user-attachments/assets/323e5b54-05ca-42a1-b543-7b0ecb023365" />

<img width="1680" height="771" alt="Image" src="https://github.com/user-attachments/assets/0ac149d4-1f5d-4d9a-b00f-8a31716abc87" />

<img width="1680" height="771" alt="Image" src="https://github.com/user-attachments/assets/20e6ab09-0138-456a-a847-81e0ad71f65c" />

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

<img width="1663" height="412" alt="Image" src="https://github.com/user-attachments/assets/a45945c1-c4a4-4091-bfc6-513826b70bec" />
<img width="1613" height="585" alt="Image" src="https://github.com/user-attachments/assets/62d2c507-2385-413c-8506-8307ff17cd64" />
<img width="1424" height="766" alt="Image" src="https://github.com/user-attachments/assets/479292a3-4cea-4a4d-8f31-9c6b0b5d8957" />
<img width="1632" height="714" alt="Image" src="https://github.com/user-attachments/assets/167ce2ab-3f93-4910-a305-446db950d0e9" />
<img width="1680" height="897" alt="Image" src="https://github.com/user-attachments/assets/fbb44085-083e-457c-af9b-40553bd4ff81" />
<img width="1680" height="766" alt="Image" src="https://github.com/user-attachments/assets/2840e20a-948d-4bd2-8a65-ec057c5a8ddb" />
<img width="1680" height="766" alt="Image" src="https://github.com/user-attachments/assets/c4ffccd1-478b-42c7-9ae7-d9cd7aad0199" />
<img width="1680" height="766" alt="Image" src="https://github.com/user-attachments/assets/2d2e9890-0d56-4c91-bbb3-ddb89f3845d5" />
<img width="1663" height="853" alt="Image" src="https://github.com/user-attachments/assets/f34ffa0f-4ea7-4980-a454-573c30668bc6" />

```
bash
#!/bin/bash
yum update -y
amazon-linux-extras enable nginx1
yum install -y nginx
systemctl start nginx
systemctl enable nginx
echo "<h1>Hello from $(hostname)</h1>" > /usr/share/nginx/html/index.html
