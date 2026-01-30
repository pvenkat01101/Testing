# Architecture Patterns (SAA focus)

## High availability web app
- Route 53 -> ALB -> Auto Scaling EC2 across multiple AZs.
- RDS Multi-AZ or Aurora for data.

## Static + dynamic
- S3 + CloudFront for static.
- API Gateway + Lambda for dynamic.

## Decoupled microservices
- Use SQS queues and EventBridge for events.
- Store state in DynamoDB.

## DR strategies
- Backup & restore (lowest cost).
- Pilot light.
- Warm standby.
- Multi-site active-active (highest cost).

## Data lake
- S3 + Glue catalog + Athena/EMR.

## Hybrid
- VPN or Direct Connect + Storage Gateway.

