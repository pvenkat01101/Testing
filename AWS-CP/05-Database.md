# Databases

## RDS
- Managed relational DB (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Aurora).
- Multi-AZ for HA; Read Replicas for scaling.

## Aurora
- MySQL/PostgreSQL compatible; auto-scaling storage.
- Higher performance and multi-AZ by default.

## DynamoDB
- Fully managed NoSQL; single-digit ms latency.
- Partition key + sort key; on-demand or provisioned.
- DAX for in-memory caching.

## ElastiCache
- Redis or Memcached; offload reads.

## Redshift
- Columnar data warehouse; OLAP.

## Neptune / DocumentDB
- Graph and MongoDB-compatible options.

**Exam tips**
- Multi-AZ for HA, Read Replica for read scaling.
- DynamoDB for massive scale, key-value.

**Hands-on**
- Create RDS MySQL with Multi-AZ.
- Create DynamoDB table with GSI.

