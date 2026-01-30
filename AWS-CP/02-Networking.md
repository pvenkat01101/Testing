# Networking & Content Delivery

## VPC (Virtual Private Cloud)
**Core pieces**
- Subnets (public/private), route tables, IGW, NAT GW, NACL, security groups.
- VPC peering, Transit Gateway, VPN, Direct Connect.

**Public vs private**
- Public subnet has route to IGW.
- Private subnet uses NAT GW for outbound only.

**Security**
- Security groups: stateful, instance level.
- NACLs: stateless, subnet level.

**Exam tips**
- Use NAT GW for private subnet internet access.
- Use VPC endpoints to access AWS services privately.

## Elastic Load Balancing (ELB)
- **ALB:** L7, path-based routing, host-based routing.
- **NLB:** L4, high performance, static IP.
- **GWLB:** for virtual appliances.

## Route 53
- DNS, routing policies: simple, weighted, latency, failover, geolocation, geoproximity, multivalue.
- Health checks, private hosted zones.

## CloudFront
- CDN with edge locations, integrates with S3/ALB.
- Signed URLs/cookies for private content.

## API Gateway
- Managed API layer for REST/HTTP/WebSocket.
- Throttling, caching, auth with IAM/Cognito.

## Global Accelerator
- Improves latency using Anycast IPs.

**Hands-on**
- Create VPC with public/private subnets and NAT GW.
- Configure ALB with target group.
- Create Route 53 hosted zone and record.

