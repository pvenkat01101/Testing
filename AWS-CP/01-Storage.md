# Storage Services

## S3 (Simple Storage Service)
**What it is:** Object storage for any amount of data.

**Key concepts**
- Buckets are region-scoped; objects are key-value.
- 11 9s durability; availability depends on class.
- Versioning, lifecycle policies, replication (SRR/CRR).
- Event notifications to SNS/SQS/Lambda.

**Storage classes**
- Standard, Intelligent-Tiering, Standard-IA, One Zone-IA, Glacier Instant/Flexible/Deep Archive.

**Security**
- IAM, bucket policies, ACLs (legacy), Block Public Access.
- Encryption: SSE-S3, SSE-KMS, SSE-C, client-side.
- S3 Object Lock for WORM compliance.

**Use cases**
- Static websites, backups, data lakes, log storage.

**Exam tips**
- S3 is regional; cross-region replication is optional.
- For static website hosting use S3 + CloudFront for HTTPS.

**Hands-on**
- Create a bucket, enable versioning, upload objects.
- Set lifecycle to move to Glacier after 30 days.
- Enable access logging to another bucket.

## EBS (Elastic Block Store)
**What it is:** Block storage for EC2, AZ-scoped.

**Types**
- gp3/gp2 (general), io1/io2 (provisioned IOPS), st1 (throughput), sc1 (cold HDD).

**Key points**
- Attached to a single instance at a time (Multi-Attach on io1/io2).
- Snapshots are incremental and stored in S3.
- Encryption at rest with KMS.

**Exam tips**
- EBS is AZ-specific; snapshots can be copied across regions.

## EFS (Elastic File System)
**What it is:** Managed NFS file system, multi-AZ.

**Key points**
- Shared file system for Linux instances.
- Performance modes and throughput modes.

**Exam tips**
- Use EFS for shared web content across EC2.

## FSx
- **FSx for Windows File Server:** SMB, AD integration.
- **FSx for Lustre:** high-performance compute.

## Storage Gateway
- Hybrid storage: file, volume, or tape gateway.

## Snow family
- Snowcone, Snowball, Snowmobile for offline data transfer.

