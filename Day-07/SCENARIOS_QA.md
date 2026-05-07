# 🔥 Day 7 — Real-World Scenario Q&A: RDS Practical

> **CloudDevOpsHub Cohort-6 | Mentor: Vikas Ratnawat**  
> 🔗 [linkedin.com/in/vikasratnawat](https://www.linkedin.com/in/vikasratnawat/) | [clouddevopshub](https://www.linkedin.com/in/clouddevopshub/)

---

## 🎬 These Scenarios Were Discussed in the Day's Recording

```
📺 Watch the session first: https://devopswithvikas.com/eud/bookings
📌 Then practice each scenario answer
🎤 Speak the answer out loud — don't just read it
💼 In interview: Use these as story-based answers
```

---

## Scenario 1: How do you perform schema migrations in production databases with zero downtime?

**Your Answer (How to Approach This):**

Use expand-contract pattern. Phase 1 Expand: Add new columns (backward compatible). Code deploys to both old and new schema. Phase 2 Migrate: Background job fills new columns from old data. Phase 3 Contract: Remove old columns once all code uses new schema. Tools: Flyway, Liquibase — version-controlled schema migrations. Never: Rename columns directly (breaks running app). Never: Delete columns in same deploy as code change. Test migrations on replica first. Run migrations before app deployment (not during).

---

## Scenario 2: A stored procedure is causing CPU spikes every hour. How do you find and fix it?

**Your Answer (How to Approach This):**

Find it: CloudWatch — CPUUtilization spikes every hour. Performance Insights — Top SQL at spike time. Show scheduled events: SHOW EVENTS. Check Event Scheduler: SELECT * FROM information_schema.EVENTS. EXPLAIN the stored procedure queries. Find the expensive operations. Fix: Add missing indexes to queries inside procedure. Rewrite inefficient cursor loops as set-based operations. Break into smaller chunks. Run during low-traffic window. Move to read replica if it's reporting query.

---

## Scenario 3: How do you implement database audit logging for compliance?

**Your Answer (How to Approach This):**

RDS → Parameter Group → Enable: general_log, audit_log (MySQL). Enable Database Activity Streams (Aurora) — real-time, tamper-proof stream. Export to CloudWatch Logs or S3. CloudWatch Insights query: All queries by specific user. Filter for sensitive tables: SELECT * FROM WHERE table LIKE '%payment%'. Integration with SIEM: CloudWatch → Kinesis → Splunk/Datadog. Retention: 7 years for SOX, 5 years for PCI-DSS. Cost: Audit logging adds ~20% overhead. Enable selectively on sensitive tables only for performance.

---

## Scenario 4: Your PostgreSQL database has bloat causing slow queries. How do you fix it?

**Your Answer (How to Approach This):**

Diagnose: SELECT schemaname, tablename, pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) FROM pg_tables ORDER BY pg_total_relation_size DESC. Check bloat: SELECT * FROM pgstattuple('orders'). Fix: VACUUM ANALYZE table; (reclaims dead rows, updates statistics). VACUUM FULL table; (rewrites table — locks table! Use pg_repack for online). pg_repack: Online bloat removal, no lock. Schedule AUTOVACUUM tuning: autovacuum_vacuum_scale_factor = 0.01 for high-traffic tables. Prevention: Regular VACUUM schedule, proper autovacuum settings.

---

## Scenario 5: How do you test disaster recovery for RDS?

**Your Answer (How to Approach This):**

Monthly DR drill process: 1. Restore snapshot to test instance (different region). 2. Verify row counts and data integrity. 3. Run application smoke tests against restored instance. 4. Measure RTO: Time from 'restore clicked' to 'application working'. Document: take screenshots, time each step. Fix gaps found. 5. Test PITR: Restore to specific timestamp. Verify correct data. 6. Test Aurora failover: aws rds failover-db-cluster. Measure actual downtime. 7. Update runbook with actual steps and timings. No untested backup is a real backup.

---

## Scenario 6: How do you handle database secrets rotation in production?

**Your Answer (How to Approach This):**

AWS Secrets Manager: Store RDS credentials. Enable automatic rotation: 30-day cycle. Rotation Lambda: Creates new password in RDS, updates Secret, tests connection, deprecates old password. Application: Uses Secrets Manager SDK, not hardcoded connection string. sdk.get_secret_value(SecretId='prod/rds/main') — always fetches latest. Connection pool: Catches authentication errors → refreshes credentials → reconnects. Zero-downtime rotation: New password active, old password valid for 5 minutes overlap. CloudTrail: Log every secret access.

---

## Scenario 7: Design a read-heavy product catalog database on Aurora.

**Your Answer (How to Approach This):**

Traffic: 1M reads/day, 100 writes/day (product updates). Architecture: Aurora MySQL writer: handles all writes (product updates). 5 Aurora readers: handle all product reads, load-balanced via reader endpoint. ElastiCache Redis: Cache product details (TTL 1 hour). Cache hit rate target: 95%. Cache invalidation: Product update → invalidate specific product key. Query design: Product by ID (PK lookup, fastest). Product search → Elasticsearch (not RDS). Product categories → denormalized in Redis. CloudFront: Cache product page HTML at edge. RDS almost never hit for popular products.

---

## Scenario 8: A long-running ETL job is locking tables in production. How do you fix this?

**Your Answer (How to Approach This):**

Immediate: SHOW PROCESSLIST (MySQL) → find ETL connection. KILL the connection if causing critical lock. Check locks: SHOW ENGINE INNODB STATUS. Long term: Move ETL to read replica — point ETL to replica endpoint only. Use SELECT ... FOR UPDATE SKIP LOCKED (MySQL 8+) for concurrent processing. Partition the ETL job — process 10,000 rows at a time with COMMIT between batches. Schedule during low-traffic window (2-4AM). Use Aurora clone for ETL — zero-cost, instant clone, no production impact.

---

## Scenario 9: How do you implement multi-region PostgreSQL for global reads?

**Your Answer (How to Approach This):**

Aurora Global Database: 1 primary cluster (Mumbai — writes). Up to 5 secondary clusters (Singapore, Frankfurt, Oregon — reads). Replication: <1 second typically. Each region has its own reader endpoint. Application: Route reads to local Aurora endpoint. Writes always to primary endpoint. Failover: Promote secondary to primary in <1 minute (managed process). Cost: Additional per-region replication costs. Great for: Global SaaS, reads distributed close to users.

---

## Scenario 10: How do you detect and prevent SQL injection in AWS RDS?

**Your Answer (How to Approach This):**

Prevention (application layer): Use parameterized queries / prepared statements. NEVER string concatenation for SQL. ORMs (SQLAlchemy, Hibernate) — use ORM query builders. Detection (AWS layer): WAF on API Gateway/ALB: SQL injection rules enabled. RDS audit logs → CloudWatch Insights: detect UNION, DROP, OR 1=1 patterns. GuardDuty: Detects unusual query patterns. Macie: Detects if data exfiltration via SQL injection succeeded. Database layer: Create least-privilege user for app — can only SELECT/INSERT/UPDATE needed tables. Can't DROP, can't access system tables. Defense in depth: App-level + WAF + monitoring + least privilege.

---

## 📣 LinkedIn Post After Completing Scenarios

```
💡 Day 7 Scenario Practice Done! #CloudDevOpsHub

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
> [← Back to Day 7 README](./README.md)