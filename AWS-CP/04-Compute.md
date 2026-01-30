# Compute

## EC2
- Instance families: general (t), compute (c), memory (r), storage (i), GPU (g).
- Pricing: On-Demand, Reserved, Savings Plans, Spot, Dedicated Hosts.
- AMI, Security Groups, User Data.

## Auto Scaling
- Target tracking, step scaling, scheduled scaling.

## Elastic Load Balancing
- ALB/NLB/GWLB; health checks and target groups.

## Lambda (Serverless)
- Event-driven compute; pay per invocation + duration.
- Limits: memory, timeout, concurrency.

## ECS / EKS / Fargate
- ECS: managed containers; Fargate = serverless compute.
- EKS: managed Kubernetes.

**Exam tips**
- For minimal ops: Lambda or Fargate.
- For long-running or custom OS: EC2.

**Hands-on**
- Launch EC2 with IAM role + user data.
- Create ALB + Auto Scaling group.
- Deploy a Lambda function triggered by S3.

