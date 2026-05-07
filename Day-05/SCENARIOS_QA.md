# 🔥 Day 5 — Real-World Scenario Q&A: S3 Storage Classes & RDS Intro

> **AWS Cloud Cohort by CloudDevOpsHub Community**  
> 🔗 [linkedin.com/in/vikasratnawat](https://www.linkedin.com/in/vikasratnawat/) | [clouddevopshub](https://www.linkedin.com/in/clouddevopshub/)

---

## 🎬 These Scenarios can be discussed in a real-time interview

```
📺 Watch the session first: https://devopswithvikas.com/eud/bookings
📌 Then practice each scenario answer
🎤 Speak the answer out loud — don't just read it
💼 In interview: Use these as story-based answers
```

---

## Scenario 1: A database query that used to take 2 seconds now takes 45 seconds. How do you debug?

**Your Answer (How to Approach This):**

1. RDS Performance Insights: Top SQL by load. 2. EXPLAIN on slow query: check for full table scans. 3. Check table growth: SHOW TABLE STATUS. 4. Check index usage: was an index dropped? 5. Check statistics: ANALYZE TABLE. 6. Check data skew: histogram statistics. 7. CloudWatch: CPUUtilization, ReadIOPS — is hardware maxed? Most likely cause: Table grew 10x and execution plan changed. Missing index or outdated statistics. Fix: Add index + ANALYZE TABLE.

---

## Scenario 2: Your company's storage costs are ₹10 lakhs/month on S3. How do you reduce it?

**Your Answer (How to Approach This):**

Audit: S3 Storage Lens → find top-cost buckets and prefixes. Actions: 1. Enable Intelligent-Tiering on unknown-access buckets (immediate). 2. Lifecycle rules on log buckets → Glacier after 30 days. 3. Delete incomplete multipart uploads (often forgotten): aws s3api list-multipart-uploads. 4. Delete old versions if versioning is on and versions are not needed. 5. Compress data before upload (gzip JSON/CSV files = 80% size reduction). Typical result: 40-60% cost reduction. Document and automate all changes.

---

## Scenario 3: How do you choose between RDS MySQL, PostgreSQL, and Aurora?

**Your Answer (How to Approach This):**

MySQL: Default choice for web apps, WordPress, existing MySQL. Huge ecosystem. Aurora MySQL: When you need MySQL + 5x performance + 15 read replicas + global. PostgreSQL: Complex queries, JSON, GIS, analytics, strict SQL compliance. Aurora PostgreSQL: PostgreSQL + 3x performance + cloud-native. Decision framework: New project? Aurora MySQL unless budget is tight → RDS MySQL. Complex analytics? PostgreSQL. Existing system? Stay on current engine for Aurora migration later.

---

## Scenario 4: Explain how you'd implement a data archival strategy for a 100TB database.

**Your Answer (How to Approach This):**

Step 1: Identify cold data. Queries unused in last 1 year = candidate for archive. Step 2: Export to S3. Use AWS DMS or mysqldump + S3 sync. Step 3: Glacier lifecycle on S3 data. Standard → Glacier after 90 days. Step 4: Partition main database table. Partition by date. Drop old partitions after archive confirmed. Step 5: AWS Athena. Query archived S3 data when needed for compliance. Result: DB shrinks from 100TB to 10TB active data. Query performance improves dramatically.

---

## Scenario 5: A database is running at 95% CPU. What do you do right now vs long term?

**Your Answer (How to Approach This):**

Right now: Kill top CPU-consuming queries. SHOW PROCESSLIST (MySQL) or pg_stat_activity (PostgreSQL). Kill runaway queries. Scale up instance temporarily (RDS → Modify → larger instance). Long term: Performance Insights → identify recurring expensive queries. Add indexes for top SQL. Consider adding read replica for reporting queries. Enable ElastiCache for frequent reads. Review application for N+1 queries. Schedule heavy reports for off-peak hours.

---

## Scenario 6: How do you implement S3 as a data lake for real-time analytics?

**Your Answer (How to Approach This):**

Ingest: Kinesis Data Firehose → S3 (buffers stream data, writes every 60s). Format: Convert to Parquet with Glue ETL (10x cheaper to query vs JSON). Catalog: Glue Crawler → updates Glue Data Catalog automatically. Query: Athena → SQL on S3 Parquet. Cache hot results: ElastiCache or materialized views. BI Tool: QuickSight → connects to Athena. Cost: Athena $5/TB scanned. Parquet compression → scan 1/10th the data → $0.50/TB effective.

---

## Scenario 7: Your RDS free storage is running out. What steps do you take?

**Your Answer (How to Approach This):**

Immediate: RDS → Modify → increase storage. AWS does this online (no downtime for gp2/gp3 volumes). Set up CloudWatch alarm: FreeStorageSpace < 20%. Enable RDS Storage Autoscaling! (RDS → Modify → Enable Autoscaling). Set max threshold. AWS will auto-expand storage when < 10% free. Root cause: Check table growth. Large log tables? Archive old data. Large binary/blob storage? Move to S3. Binary logging (MySQL): Purge old binlogs: CALL mysql.rds_set_configuration('binlog retention hours', 24).

---

## Scenario 8: How do you design a multi-tenant database architecture on RDS?

**Your Answer (How to Approach This):**

Option 1: One DB per tenant (most isolation, expensive at scale). RDS + separate database per tenant. Good for: Enterprise clients, strict data isolation requirements. Option 2: Shared DB, separate schemas. PostgreSQL schemas per tenant. schema_name = tenant_id. Row-level security. More efficient. Good for: Mid-size SaaS. Option 3: Shared DB, shared schema, tenant_id column. Row-level security on all tables. Most efficient but hardest to implement. Good for: High-volume SaaS, many small tenants. Recommendation: Mix — large clients get dedicated DB, small clients share.

---

## Scenario 9: S3 Replication is not working. How do you troubleshoot?

**Your Answer (How to Approach This):**

Check: 1. Versioning enabled on BOTH source and destination? Required. 2. IAM role has s3:ReplicateObject permission on destination? 3. Destination bucket policy allows source account? (cross-account). 4. S3 Replication Time Control (RTC) enabled? Check SLA. 5. Check replication metrics: ReplicationLatency, BytesPendingReplication. 6. CloudTrail: check for permission errors. 7. Object created BEFORE replication rule? Those don't replicate. Use S3 Batch Replication for existing objects.

---

## Scenario 10: Design database strategy for a FinTech app handling 10,000 transactions/second.

**Your Answer (How to Approach This):**

Write path: Aurora MySQL Multi-Master OR PostgreSQL with Citus (horizontal sharding). Shard by account_id. Read path: 15 Aurora read replicas. Read queries distributed via Aurora endpoint auto-routing. Caching: ElastiCache Redis. Account balances cached, invalidated on transaction. Queue: SQS FIFO for transaction ordering guarantees. Audit: DynamoDB Streams → Lambda → Audit table (immutable). Compliance: Aurora with deletion protection, point-in-time recovery, encryption. Monitoring: CloudWatch + custom metrics: transactions/second, error rate, p99 latency.

---

## 📣 LinkedIn Post After Completing Scenarios

```
💡 Day 5 Scenario Practice Done! #CloudDevOpsHub

Today I practiced 10 real AWS scenario-based interview questions.

My favorite scenario today:
[Share your favorite scenario and your approach to solving it]

This is the kind of preparation that makes the difference between
'I know AWS' and 'I can DO AWS in production.' 🎯

Attending AWS Cohort-6 by @Vikas Ratnawat | @CloudDevOpsHub 🔥
#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHubCommunity
#VikasRatnawat #AWS #CloudEngineer #InterviewReady
```

---

> 🌟 **Fork → Customize with your experience → Share on LinkedIn**  
> [← Back to Day 5 README](./README.md)
