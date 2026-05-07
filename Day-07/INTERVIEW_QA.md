# 🎯 Day 7 — Interview Questions & Answers: RDS Practical

> **CloudDevOpsHub Cohort-6 | Mentor: Vikas Ratnawat**  
> 🔗 [linkedin.com/in/vikasratnawat](https://www.linkedin.com/in/vikasratnawat/) | [clouddevopshub](https://www.linkedin.com/in/clouddevopshub/)

---

## 📋 How to Use This File

```
✅ Read each answer once — understand the concept
✅ Close file — explain it out loud in your own words
✅ Record yourself answering — review your confidence level
✅ In interview — use STAR format: Situation → Task → Action → Result
```

> 🎓 Watch the Day recorded session first:
> 👉 [https://devopswithvikas.com/eud/bookings](https://devopswithvikas.com/eud/bookings)

---

## Q1. How do you perform schema migrations in production databases with zero downtime?

**Answer:**

Use expand-contract pattern. Phase 1 Expand: Add new columns (backward compatible). Code deploys to both old and new schema. Phase 2 Migrate: Background job fills new columns from old data. Phase 3 Contract: Remove old columns once all code uses new schema. Tools: Flyway, Liquibase — version-controlled schema migrations. Never: Rename columns directly (breaks running app). Never: Delete columns in same deploy as code change. Test migrations on replica first. Run migrations before app deployment (not during).

---

## Q2. A stored procedure is causing CPU spikes every hour. How do you find and fix it?

**Answer:**

Find it: CloudWatch — CPUUtilization spikes every hour. Performance Insights — Top SQL at spike time. Show scheduled events: SHOW EVENTS. Check Event Scheduler: SELECT * FROM information_schema.EVENTS. EXPLAIN the stored procedure queries. Find the expensive operations. Fix: Add missing indexes to queries inside procedure. Rewrite inefficient cursor loops as set-based operations. Break into smaller chunks. Run during low-traffic window. Move to read replica if it's reporting query.

---

## Q3. How do you implement database audit logging for compliance?

**Answer:**

RDS → Parameter Group → Enable: general_log, audit_log (MySQL). Enable Database Activity Streams (Aurora) — real-time, tamper-proof stream. Export to CloudWatch Logs or S3. CloudWatch Insights query: All queries by specific user. Filter for sensitive tables: SELECT * FROM WHERE table LIKE '%payment%'. Integration with SIEM: CloudWatch → Kinesis → Splunk/Datadog. Retention: 7 years for SOX, 5 years for PCI-DSS. Cost: Audit logging adds ~20% overhead. Enable selectively on sensitive tables only for performance.

---

## Q4. Your PostgreSQL database has bloat causing slow queries. How do you fix it?

**Answer:**

Diagnose: SELECT schemaname, tablename, pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) FROM pg_tables ORDER BY pg_total_relation_size DESC. Check bloat: SELECT * FROM pgstattuple('orders'). Fix: VACUUM ANALYZE table; (reclaims dead rows, updates statistics). VACUUM FULL table; (rewrites table — locks table! Use pg_repack for online). pg_repack: Online bloat removal, no lock. Schedule AUTOVACUUM tuning: autovacuum_vacuum_scale_factor = 0.01 for high-traffic tables. Prevention: Regular VACUUM schedule, proper autovacuum settings.

---

## Q5. How do you test disaster recovery for RDS?

**Answer:**

Monthly DR drill process: 1. Restore snapshot to test instance (different region). 2. Verify row counts and data integrity. 3. Run application smoke tests against restored instance. 4. Measure RTO: Time from 'restore clicked' to 'application working'. Document: take screenshots, time each step. Fix gaps found. 5. Test PITR: Restore to specific timestamp. Verify correct data. 6. Test Aurora failover: aws rds failover-db-cluster. Measure actual downtime. 7. Update runbook with actual steps and timings. No untested backup is a real backup.

---

## 📣 LinkedIn Post After Practicing These

```
🎯 Day 7 Interview Prep Done! #RDSPractical #CloudDevOpsHub

Practiced 5 real-world AWS interview questions today!

Most important thing I learned:
[Write your key takeaway from RDS Practical]

These aren't textbook answers — they're real scenarios.
Thanks to @Vikas Ratnawat | @CloudDevOpsHub for the depth! 🙏

#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHub
#CloudDevOpsHubCommunity #VikasRatnawat #AWS #InterviewPrep
```

---

> 🌟 **Fork → Customize → Push → Share on LinkedIn**  
> [← Back to Day 7 README](./README.md)