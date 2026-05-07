# 🎯 Day 6 — Interview Questions & Answers: AWS RDS Deep Dive

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

## Q1. Production RDS database is having connection errors: 'Too many connections'. How do you fix?

**Answer:**

Immediate: Check max_connections: SHOW VARIABLES LIKE 'max_connections'. Check current: SHOW STATUS LIKE 'Threads_connected'. Kill idle connections: SELECT * FROM information_schema.PROCESSLIST WHERE COMMAND='Sleep' AND TIME > 600. Long-term fix: Add RDS Proxy (reduces connections by 87%). Scale up DB instance (larger = more connections). Review app connection pool settings (HikariCP: maximumPoolSize). Set wait_timeout and interactive_timeout to close idle connections faster.

---

## Q2. You need to migrate a 2TB production database from on-premises to RDS with minimal downtime.

**Answer:**

Strategy: Online migration with <5 minutes downtime. Phase 1 (1 week): Setup RDS instance. Use AWS DMS (Database Migration Service). Initial load: DMS full-load migration (runs while prod is live). Phase 2 (ongoing): DMS CDC (Change Data Capture) replication. Ongoing changes from source replicate to RDS in near real-time. Phase 3 (cutover weekend): Verify row counts. Stop writes to source for 2 minutes. Let DMS sync final changes. Update DNS/connection string. Start RDS as primary. Downtime: 2-5 minutes. Rollback: Point DNS back to old DB if issues.

---

## Q3. Your RDS Read Replica has 5-minute lag. What causes this and how do you fix?

**Answer:**

Causes: 1. Heavy write load on primary (replica can't keep up). 2. Long-running write transactions block replication. 3. Replica under-provisioned (smaller instance than primary). 4. Network latency between AZs. Fixes: 1. Scale up replica instance to match primary. 2. Review and optimize long transactions on primary. 3. Parallel replication: set slave_parallel_workers (MySQL 5.7+). 4. If writes > replica capacity → sharding or write to multiple primaries. Monitor: ReplicaLag CloudWatch metric. Alert at >30 seconds.

---

## Q4. How do you perform a major version upgrade of RDS from MySQL 5.7 to MySQL 8.0?

**Answer:**

Step 1: Test in dev/staging first! Run application against MySQL 8.0 RDS in staging. Fix compatibility issues (MySQL 8 removed some functions). Step 2: Take snapshot of production DB. Step 3: Restore snapshot to new test instance with MySQL 8.0. Run full regression test suite. Step 4: Schedule maintenance window (low-traffic time). Step 5: RDS → Modify → DB engine version → 8.0. Select 'Apply immediately'. Downtime: 10-30 minutes for version upgrade. Step 6: Verify application works. If not: restore from pre-upgrade snapshot. This is why we always take snapshot before major changes!

---

## Q5. A developer ran DELETE FROM orders without WHERE clause. How do you recover?

**Answer:**

Immediate stop: Kill the connection if still running. SHOW PROCESSLIST → KILL connection_id. Check binlog (if enabled): Was DELETE committed? Assess damage: SELECT COUNT(*) FROM orders. If 0 rows: full delete confirmed. Recovery options: 1. PITR (Point-in-Time Recovery): RDS → Restore to point in time → 30 seconds before DELETE. Creates new RDS instance. This is the fastest, cleanest recovery. 2. Manual binlog recovery: mysqlbinlog → find DELETE statement → run events before it. Prevention: Require transactions for DML. SQL_SAFE_UPDATES=1. Table-level delete protection via IAM policy.

---

## 📣 LinkedIn Post After Practicing These

```
🎯 Day 6 Interview Prep Done! #AWSRDSDeepDive #CloudDevOpsHub

Practiced 5 real-world AWS interview questions today!

Most important thing I learned:
[Write your key takeaway from AWS RDS Deep Dive]

These aren't textbook answers — they're real scenarios.
Thanks to @Vikas Ratnawat | @CloudDevOpsHub for the depth! 🙏

#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHub
#CloudDevOpsHubCommunity #VikasRatnawat #AWS #InterviewPrep
```

---

> 🌟 **Fork → Customize → Push → Share on LinkedIn**  
> [← Back to Day 6 README](./README.md)