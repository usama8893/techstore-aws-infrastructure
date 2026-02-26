# TechStore AWS Infrastructure

Production-grade auto-scaling web application infrastructure on AWS, built following AWS Well-Architected Framework principles.

![Architecture Diagram](architecture/architecture-diagram.png)

## 🏗️ Architecture Overview

This project implements a highly available, auto-scaling web application on AWS with:

- **Multi-AZ Deployment** across 2 Availability Zones (us-east-1a, us-east-1b)
- **Application Load Balancer** for traffic distribution and health checking
- **Auto Scaling Group** (2-6 EC2 instances) with CPU-based scaling policies
- **RDS MySQL Database** with automated backups and Multi-AZ capability
- **S3 Buckets** with lifecycle policies and versioning
- **CloudWatch Monitoring** with alarms and dashboards
- **SNS Notifications** for infrastructure alerts

## 🎯 Key Features

### High Availability
- Multi-AZ deployment survives datacenter failures
- Auto Scaling maintains minimum 2 instances
- RDS Multi-AZ with automatic failover (60 seconds)
- Load balancer health checks remove unhealthy instances

### Auto Scaling
- Scales 2-6 instances based on CPU utilization (target: 50%)
- Handles traffic spikes automatically
- Cost-optimized (scales down during low traffic)
- Self-healing (replaces failed instances)

### Security
- VPC isolation with public/private subnets
- Security groups follow least privilege principle
- IAM roles (no hardcoded credentials)
- Encryption at rest and in transit
- Database in private subnet (not internet-accessible)

### Monitoring
- CloudWatch metrics for all resources
- 5 CloudWatch alarms for proactive alerting
- Centralized dashboard for visibility
- SNS email notifications

## 💰 Cost Estimate

**Monthly costs (us-east-1):**

| Resource | Cost (Free Tier) | Cost (Standard) |
|----------|------------------|-----------------|
| EC2 (2 × t3.micro) | $0 | $15 |
| Application Load Balancer | $16 | $16 |
| RDS (db.t3.micro) | $0 | $12.50 |
| NAT Gateway | $32 | $32 |
| S3 (100 GB) | $2.30 | $2.30 |
| CloudWatch | $1 | $1 |
| **Total** | **~$51/month** | **~$79/month** |

*At maximum scale (6 instances): ~$110/month*

## 🚀 Technologies Used

- **Compute:** EC2 (Auto Scaling), Application Load Balancer
- **Database:** RDS MySQL 8.0
- **Storage:** S3 with lifecycle policies
- **Network:** VPC, Subnets, Internet Gateway, NAT Gateway
- **Security:** Security Groups, IAM Roles, Encryption
- **Monitoring:** CloudWatch Metrics, Alarms, Dashboards
- **Notifications:** SNS (Simple Notification Service)
- **Infrastructure:** AWS Console, Launch Templates

## 📋 Architecture Components

### Network Layer (VPC)
```
VPC: 10.0.0.0/16
├── AZ-1 (us-east-1a)
│   ├── Public Subnet: 10.0.1.0/24
│   └── Private Subnet: 10.0.2.0/24
└── AZ-2 (us-east-1b)
    ├── Public Subnet: 10.0.3.0/24
    └── Private Subnet: 10.0.4.0/24
```

### Compute Layer
- Application Load Balancer (internet-facing)
- Auto Scaling Group (2-6 t3.micro instances)
- Launch Template with user data (Apache installation)

### Database Layer
- RDS MySQL 8.0 (db.t3.micro)
- Multi-AZ deployment
- Automated backups (7-day retention)
- Encryption at rest (AES-256)

### Storage Layer
- S3 buckets for product images, user uploads, backups
- Lifecycle policies (Standard → IA → Glacier)
- Versioning enabled
- Server-side encryption (SSE-S3)

## 🔧 Setup Instructions

See [SETUP.md](docs/SETUP.md) for detailed deployment instructions.

**Quick start:**
1. Configure AWS credentials
2. Create VPC and subnets
3. Deploy security groups
4. Launch RDS database
5. Create launch template
6. Deploy load balancer
7. Configure Auto Scaling Group
8. Set up CloudWatch monitoring

## 📊 Monitoring & Alerts

**CloudWatch Alarms:**
- High CPU (>80% for 10 minutes)
- Unhealthy hosts in load balancer
- Database CPU (>75% for 15 minutes)
- Low disk space (<2 GB free)
- High 5XX error rate (>10 per 5 minutes)

**Dashboard includes:**
- CPU utilization (EC2 & RDS)
- Request count and response time
- Healthy/Unhealthy host counts
- Database connections
- Alarm status overview

## 🔒 Security Best Practices Implemented

- ✅ VPC isolation with public/private subnets
- ✅ Security groups with least privilege
- ✅ IAM roles (no access keys on instances)
- ✅ Database in private subnet
- ✅ Encryption at rest (EBS, RDS, S3)
- ✅ Encryption in transit (HTTPS, TLS)
- ✅ IMDSv2 enforced (instance metadata security)
- ✅ Automated backups enabled
- ✅ CloudTrail logging (audit trail)
- ✅ MFA delete on S3 (optional)

## 📈 Performance Characteristics

**Capacity:**
- Normal load: 2 instances handle ~1,000 concurrent users
- Maximum load: 6 instances handle ~3,000 concurrent users
- Database: db.t3.micro supports ~50 connections

**Latency:**
- Load balancer: <10ms overhead
- Target response time: <100ms (alarm at >500ms)
- Database queries: <50ms average

**Availability:**
- Target: 99.9% uptime (43 minutes downtime/month)
- RDS failover: ~60 seconds
- Instance replacement: ~5 minutes

## 🧪 Testing Performed

- ✅ Load balancer distributes traffic evenly
- ✅ Auto Scaling launches instances on high CPU
- ✅ Auto Scaling terminates instances on low CPU
- ✅ Health checks remove unhealthy instances
- ✅ Database failover tested (Multi-AZ)
- ✅ Instance termination triggers replacement
- ✅ CloudWatch alarms trigger correctly
- ✅ SNS notifications delivered

## 🎓 Learning Outcomes

This project demonstrates:
- Multi-tier architecture design
- High availability patterns
- Auto-scaling strategies
- Security best practices
- Cost optimization techniques
- Infrastructure automation
- Monitoring and alerting
- Troubleshooting skills

## 🤝 Contributing

This is a learning project, but suggestions are welcome! Feel free to:
- Open issues for questions
- Submit PRs for improvements
- Share your implementations

## 📄 License

MIT License - See [LICENSE](LICENSE) file

## 👤 Author

**[Your Name]**
- GitHub: [@usama8893](https://github.com/usama8893)
- LinkedIn: [Muhammad Usama]([https://linkedin.com/in/your-profile](https://www.linkedin.com/in/muhammad-usama-689428232/))

## 🙏 Acknowledgments

Built as part of AWS Cloud Engineering learning curriculum.
- AWS Well-Architected Framework
- AWS Solutions Architect certification materials
- AWS documentation and best practices
