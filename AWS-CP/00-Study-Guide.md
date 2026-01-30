# AWS Cloud Practitioner + Solutions Architect Associate Study Notes

## How to use this folder
- Start with 00-Study-Guide.md, then read services by domain.
- Do labs from 11-Hands-On-Labs.md while reading theory.
- Use the question banks weekly: interview set + certification set.

## Exam focus split
- **Cloud Practitioner (CLF-C02):** fundamentals, pricing, billing, shared responsibility, core services.
- **Solutions Architect Associate (SAA-C03):** architecture, HA, resilience, performance, security, cost tradeoffs.

## Study plan (8 weeks)
- **Week 1:** Cloud basics, IAM, global infrastructure, billing.
- **Week 2:** Storage + Database.
- **Week 3:** Compute + Auto Scaling + ELB.
- **Week 4:** Networking + VPC + Route 53.
- **Week 5:** Security + Compliance + Encryption.
- **Week 6:** Integration + Serverless + Eventing.
- **Week 7:** Management + Monitoring + Cost Optimization.
- **Week 8:** Review + practice tests + weak areas.

## Key AWS principles
- **Global infrastructure:** Regions, Availability Zones, edge locations.
- **Shared responsibility:** AWS secures the cloud; you secure in the cloud.
- **Well-Architected pillars:** Operational excellence, security, reliability, performance efficiency, cost optimization, sustainability.

## Core architecture patterns to master
- **Stateless web tier** behind ALB + Auto Scaling.
- **Decouple** with SQS + SNS + EventBridge.
- **Data durability** with multi-AZ/replication.
- **Disaster recovery:** backup & restore, pilot light, warm standby, multi-site.

## Exam tip checklist
- Read the last line of the question first (what is being asked?).
- Look for keywords: "most cost-effective", "least operational overhead", "high availability".
- Eliminate answers with extra services or unnecessary complexity.
- Prefer managed services unless a requirement forbids them.

