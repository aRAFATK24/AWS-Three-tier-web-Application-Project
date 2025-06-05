# AWS Three-Tier Web Application Architecture

## Project Overview
This project implements a highly available, scalable three-tier web application on AWS, featuring:

- **Presentation Tier**: Public-facing web servers with load balancing
- **Application Tier**: PHP/Apache servers with phpMyAdmin
- **Data Tier**: Multi-AZ MySQL RDS instance

## Key Components
| AWS Service       | Purpose                                                                 |
|-------------------|-------------------------------------------------------------------------|
| VPC              | Isolated network environment (CIDR: 20.0.0.0/20)                        |
| EC2              | Web servers (public) + App servers (private)                            |
| RDS MySQL        | Managed database with high availability                                 |
| Application LB   | Distributes traffic across app servers                                  |
| Route 53         | DNS management                                                          |
| NAT Gateway      | Outbound internet access for private subnets                            |

## Implementation Steps

### 1. Network Infrastructure
bash
VPC: awsProject-vpc (20.0.0.0/20)
Public Subnets: web-pub-sub1 (20.0.1.0/24), web-pub-sub2 (20.0.2.0/24)
Private Subnets: app-pvt-sub[1-2], db-pvt-sub[1-2]
NAT Gateway + Elastic IP
Internet Gateway + Route Tables


### 2. EC2 Deployment
- **Jump Server**: Bastion host in public subnet (SSH access)
- **App Servers**: 2 instances in private subnets (PHP 8.2 + Apache)
- **Security Groups**: Strict access controls between tiers

### 3. Database Setup
bash
RDS MySQL (Free Tier)
DB Subnet Group: db-pvt-sub[1-2]
Custom Security Group: Restricted to app tier


### 4. Load Balancing
bash
ALB Configuration:
- Listener: HTTP:80
- Target Group: app-tg (EC2 instances)
- Health Checks: HTTP /
- Enabled Stickiness


## Access Instructions
1. Connect to jump server via SSH
2. Access application via ALB DNS:
   
   http://<ALB-DNS>/
   http://<ALB-DNS>/phpMyAdmin
   
3. Database credentials:
   
   Username: admin
   Password: admin
   

## Best Practices Implemented
✔ Multi-AZ deployment for high availability  
✔ Least-privilege security group rules  
✔ Private database subnet with no public access  
✔ Encrypted key pair for SSH access  
✔ Load balancer stickiness for session persistence  

## Future Enhancements
- Implement Auto Scaling for app tier
- Add CloudFront CDN for static content
- Set up RDS read replicas
- Configure monitoring with CloudWatch
