# FAANG-Level Interview Questions (30) + Short Answers

1) **Design a highly available web app for 1M users.**
- Route 53 -> ALB -> Auto Scaling in 2+ AZs; stateless app; session in ElastiCache/DynamoDB; RDS Multi-AZ/Aurora; S3 + CloudFront.

2) **How do you implement zero-downtime deployments on AWS?**
- Blue/green with ALB target groups or CodeDeploy; weighted Route 53; or canary via Lambda aliases.

3) **Explain difference between Multi-AZ and Read Replicas in RDS.**
- Multi-AZ = synchronous HA failover; Read Replica = async read scaling and DR.

4) **When choose DynamoDB over RDS?**
- Need massive scale, key-value access, predictable low latency, serverless.

5) **Design a serverless image processing pipeline.**
- S3 upload -> Lambda -> S3 output; use SQS for retries, Step Functions for orchestration.

6) **How to secure S3 buckets for compliance?**
- Block Public Access, bucket policy, encryption (SSE-KMS), versioning + Object Lock, CloudTrail.

7) **How do you reduce latency for global users?**
- CloudFront + edge caching; Route 53 latency routing; multi-region active-active if needed.

8) **Describe DR strategies and when to use each.**
- Backup/restore (low cost); pilot light (core services always on); warm standby; multi-site active-active (highest cost).

9) **How would you design a multi-tenant SaaS on AWS?**
- Separate accounts or shared VPC; isolate data via tenant IDs; IAM role boundaries; use Organizations.

10) **What is a VPC endpoint and when is it used?**
- Private access to AWS services without IGW/NAT; for security and cost.

11) **How does IAM policy evaluation work?**
- Explicit deny wins; then allow; default deny.

12) **Design log analytics for 5TB/day.**
- Kinesis Firehose -> S3 -> Glue catalog -> Athena/EMR; store in partitioned S3.

13) **How would you handle backpressure in event-driven systems?**
- SQS buffer, Lambda concurrency controls, DLQ, batch size.

14) **How to ensure data durability for backups?**
- Use S3 with versioning and CRR; Glacier for long-term.

15) **Compare ALB vs NLB vs API Gateway.**
- ALB for HTTP routing; NLB for TCP/UDP low latency; API Gateway for APIs, auth, throttling.

16) **How do you achieve least privilege for EC2 access to S3?**
- IAM role with scoped policy; use instance profile.

17) **What are the tradeoffs of Spot Instances?**
- Lower cost, but can be interrupted; use for fault-tolerant batch jobs.

18) **Explain how to handle secrets in AWS.**
- Store in Secrets Manager or Parameter Store with KMS; rotate secrets.

19) **How would you design a multi-region failover for a critical API?**
- Active-active via Route 53 health checks + latency routing; replicate data (Aurora Global DB).

20) **How to reduce database load without changing app code?**
- Add ElastiCache; enable RDS read replicas.

21) **What is the shared responsibility model?**
- AWS secures infrastructure; customer secures data, IAM, OS, configs.

22) **How do you isolate environments (dev/prod)?**
- Separate accounts via Organizations; SCPs and billing separation.

23) **Explain eventual consistency in S3 and DynamoDB.**
- S3 is read-after-write for new objects; DynamoDB can be eventually or strongly consistent for reads.

24) **How would you handle large file uploads in a web app?**
- Use pre-signed S3 URLs; multipart upload.

25) **Design an audit system for all AWS API calls.**
- Enable CloudTrail across all accounts; central S3 log bucket.

26) **How to protect against DDoS?**
- Use Shield + WAF; CloudFront and ALB.

27) **How do you choose between ECS and EKS?**
- ECS for simpler managed container orchestrator; EKS if need Kubernetes ecosystem.

28) **What is the purpose of a NAT Gateway?**
- Allow outbound internet access from private subnet without inbound exposure.

29) **How to implement CI/CD on AWS?**
- CodeCommit/CodeBuild/CodeDeploy/CodePipeline; or integrate GitHub with CodePipeline.

30) **How do you monitor microservices in AWS?**
- CloudWatch metrics/logs, X-Ray tracing, centralized dashboards.

