# Scalable Web Application with ALB and Auto Scaling

Deploys a highly available web app on AWS using EC2, ALB, Auto Scaling, and RDS.

## Architecture
![Solution Diagram](./architecture-diagram.png)

## AWS Services Used
- **EC2**: Web servers running Apache/PHP
- **ALB**: Distributes traffic to EC2 instances
- **Auto Scaling**: Dynamically adjusts instance count
- **RDS**: Multi-AZ MySQL database (optional)
- **CloudWatch/SNS**: Monitoring and alerts

## Deployment Steps

### 1. Create Security Groups
- **ALB SG**: Allow HTTP/HTTPS from `0.0.0.0/0`
- **EC2 SG**: Allow HTTP from ALB SG + SSH from your IP
- **RDS SG**: Allow port 3306 from EC2 SG

### 2. Launch EC2 Instances via ASG
- **AMI**: Amazon Linux 2023
- **Instance Type**: t3.micro
- **User Data** (Bootstrap script):
  ```bash
  #!/bin/bash
  yum update -y
  yum install -y httpd php mysql
  systemctl start httpd
  systemctl enable httpd
  echo "<?php phpinfo(); ?>" > /var/www/html/index.php