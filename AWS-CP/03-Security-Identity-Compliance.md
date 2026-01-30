# Security, Identity, Compliance

## IAM
- Users, groups, roles, policies (JSON).
- Best practice: least privilege, MFA, roles for EC2/Lambda.
- IAM policy evaluation: explicit deny > allow > default deny.

## Organizations
- Consolidated billing, SCPs, account hierarchy.

## KMS
- Managed key service; CMK, key policies, rotation.

## Secrets Manager & Parameter Store
- Secrets Manager for rotating secrets.
- Parameter Store for config values; basic secrets with KMS.

## Cognito
- User pools (auth), identity pools (AWS access).

## WAF & Shield
- WAF for L7 filtering; Shield for DDoS protection.

## GuardDuty, Security Hub, Macie
- Threat detection, centralized security posture, data discovery.

## CloudTrail
- API audit logs; store in S3.

## Compliance
- Shared responsibility; data residency; encryption.

**Hands-on**
- Create IAM role for EC2 with S3 read access.
- Enable CloudTrail and send logs to S3.
- Enable KMS encryption for S3 bucket.

