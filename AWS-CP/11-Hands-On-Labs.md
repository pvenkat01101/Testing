# Hands-On Labs (Beginner friendly)

1) **S3 + CloudFront static site**
- Create S3 bucket, enable static website hosting.
- Upload HTML/CSS and configure public access.
- Add CloudFront distribution.

2) **IAM least privilege**
- Create IAM user + group.
- Attach a policy for read-only S3.

3) **EC2 + ALB + Auto Scaling**
- Launch EC2 with user data to install web server.
- Create target group and ALB.
- Set Auto Scaling group min=2 across AZs.

4) **RDS + EC2 app**
- Create RDS MySQL Multi-AZ.
- Connect from EC2 using security groups.

5) **Lambda + S3 event**
- Create Lambda to resize image.
- Trigger on S3 upload event.

6) **VPC with public/private subnets**
- Public subnet with IGW.
- Private subnet with NAT GW.
- Test outbound access.

7) **CloudWatch alarms**
- Create alarm for CPU > 70%.

8) **SQS decoupling**
- Create queue, send/receive messages.

9) **Kinesis Firehose**
- Stream logs to S3.

10) **CloudFormation**
- Deploy a simple VPC stack.

