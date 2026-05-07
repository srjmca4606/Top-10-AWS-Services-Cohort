# 🔥 Day 6 — Real-World Scenario Q&A: AWS RDS Deep Dive

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

## Scenario 1: Production RDS database is having connection errors: 'Too many connections'. How do you fix?

**Your Answer (How to Approach This):**

Immediate: Check max_connections: SHOW VARIABLES LIKE 'max_connections'. Check current: SHOW STATUS LIKE 'Threads_connected'. Kill idle connections: SELECT * FROM information_schema.PROCESSLIST WHERE COMMAND='Sleep' AND TIME > 600. Long-term fix: Add RDS Proxy (reduces connections by 87%). Scale up DB instance (larger = more connections). Review app connection pool settings (HikariCP: maximumPoolSize). Set wait_timeout and interactive_timeout to close idle connections faster.

---

## Scenario 2: You need to migrate a 2TB production database from on-premises to RDS with minimal downtime.

**Your Answer (How to Approach This):**

Strategy: Online migration with <5 minutes downtime. Phase 1 (1 week): Setup RDS instance. Use AWS DMS (Database Migration Service). Initial load: DMS full-load migration (runs while prod is live). Phase 2 (ongoing): DMS CDC (Change Data Capture) replication. Ongoing changes from source replicate to RDS in near real-time. Phase 3 (cutover weekend): Verify row counts. Stop writes to source for 2 minutes. Let DMS sync final changes. Update DNS/connection string. Start RDS as primary. Downtime: 2-5 minutes. Rollback: Point DNS back to old DB if issues.

---

## Scenario 3: Your RDS Read Replica has 5-minute lag. What causes this and how do you fix?

**Your Answer (How to Approach This):**

Causes: 1. Heavy write load on primary (replica can't keep up). 2. Long-running write transactions block replication. 3. Replica under-provisioned (smaller instance than primary). 4. Network latency between AZs. Fixes: 1. Scale up replica instance to match primary. 2. Review and optimize long transactions on primary. 3. Parallel replication: set slave_parallel_workers (MySQL 5.7+). 4. If writes > replica capacity → sharding or write to multiple primaries. Monitor: ReplicaLag CloudWatch metric. Alert at >30 seconds.

---

## Scenario 4: How do you perform a major version upgrade of RDS from MySQL 5.7 to MySQL 8.0?

**Your Answer (How to Approach This):**

Step 1: Test in dev/staging first! Run application against MySQL 8.0 RDS in staging. Fix compatibility issues (MySQL 8 removed some functions). Step 2: Take snapshot of production DB. Step 3: Restore snapshot to new test instance with MySQL 8.0. Run full regression test suite. Step 4: Schedule maintenance window (low-traffic time). Step 5: RDS → Modify → DB engine version → 8.0. Select 'Apply immediately'. Downtime: 10-30 minutes for version upgrade. Step 6: Verify application works. If not: restore from pre-upgrade snapshot. This is why we always take snapshot before major changes!

---

## Scenario 5: A developer ran DELETE FROM orders without WHERE clause. How do you recover?

**Your Answer (How to Approach This):**

Immediate stop: Kill the connection if still running. SHOW PROCESSLIST → KILL connection_id. Check binlog (if enabled): Was DELETE committed? Assess damage: SELECT COUNT(*) FROM orders. If 0 rows: full delete confirmed. Recovery options: 1. PITR (Point-in-Time Recovery): RDS → Restore to point in time → 30 seconds before DELETE. Creates new RDS instance. This is the fastest, cleanest recovery. 2. Manual binlog recovery: mysqlbinlog → find DELETE statement → run events before it. Prevention: Require transactions for DML. SQL_SAFE_UPDATES=1. Table-level delete protection via IAM policy.

---

## Scenario 6: How do you implement database connection pooling for a Lambda function connecting to RDS?

**Your Answer (How to Approach This):**

Lambda problem: Each invocation opens a new connection. 1000 concurrent Lambdas = 1000 DB connections. RDS MySQL max: 500. Solution: RDS Proxy. Setup: RDS → Proxies → Create proxy. IAM authentication (no passwords in Lambda code). Lambda → RDS Proxy endpoint (instead of RDS directly). Proxy maintains pool of 50 connections to RDS. 1000 Lambdas → Proxy → 50 DB connections. Lambda code change: Just update connection string to proxy endpoint. Same code, same queries. Proxy handles everything.

---

## Scenario 7: Design a highly available RDS architecture for a banking application.

**Your Answer (How to Approach This):**

Requirements: RPO=0 (no data loss), RTO<1min (near-instant recovery). Architecture: Aurora MySQL Global Database. Primary region: ap-south-1 (Mumbai). Secondary region: ap-southeast-1 (Singapore). Replication: <1 second lag (Global Database). Failover: <1 minute to secondary region. Multi-AZ within Mumbai: writer + 2 readers. Encryption: KMS with customer-managed keys. Deletion protection: enabled. Backup: 35-day retention + manual snapshots before releases. Performance Insights: enabled. Enhanced monitoring: enabled. Audit logs: exported to CloudWatch.

---

## Scenario 8: How do you optimize Aurora for high read throughput?

**Your Answer (How to Approach This):**

1. Use Aurora Reader Endpoint. Automatically load-balances across all reader instances. Connection string: cluster.cluster-ro-xxx.rds.amazonaws.com. 2. Add up to 15 Aurora replicas. Scale during high traffic. 3. Aurora Auto Scaling for replicas. Target metric: CPUUtilization > 70% → add replica. 4. Connection pooling: PgBouncer (PostgreSQL) or ProxySQL (MySQL). 5. Caching: ElastiCache Redis in front of Aurora. Cache hit: 0ms. Cache miss: Aurora replica: 5ms. 6. Query optimization: indexes, query rewrite. 7. Partitioning for large tables.

---

## Scenario 9: What is your RDS backup strategy for a GDPR-compliant application?

**Your Answer (How to Approach This):**

Automated backups: 35-day retention (max). Point-in-time recovery to any second. Manual snapshots: Before every release, monthly long-term. Cross-region copy: Monthly snapshot copied to EU-West (Ireland) for EU data compliance. Encryption: All snapshots encrypted with KMS. Testing: Monthly restore test — verify backup is actually valid. Right-to-erasure: When user requests deletion, scrub their data in DB. Snapshot retention for scrubbed data: separate policy. Document: Evidence of backup testing for GDPR DPA requirements.

---

## Scenario 10: Aurora is costing too much. How do you reduce cost?

**Your Answer (How to Approach This):**

Analysis: AWS Cost Explorer → Aurora line items. Writer cost, Reader cost, I/O, Storage, Backup separately. Optimization: 1. Aurora Serverless v2: Variable load → Serverless saves 30-60%. 2. Right-size readers: Monitor CPU — if <30% avg → downsize. 3. Reduce backup retention: 35 → 7 days if compliance allows. 4. Aurora I/O-Optimized: If I/O costs > 25% of total Aurora cost → switch to I/O-optimized pricing. 5. Reserved Instances: 1-year RI saves 40% on steady writer+readers. 6. Scheduled scaling: Scale down readers at night.

---

## 📣 LinkedIn Post After Completing Scenarios

```
💡 Day 6 Scenario Practice Done! #CloudDevOpsHub

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
> [← Back to Day 6 README](./README.md)
