# 🎯 Day 5 — Interview Questions & Answers: S3 Storage Classes & RDS Intro

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

## Q1. A database query that used to take 2 seconds now takes 45 seconds. How do you debug?

**Answer:**

1. RDS Performance Insights: Top SQL by load. 2. EXPLAIN on slow query: check for full table scans. 3. Check table growth: SHOW TABLE STATUS. 4. Check index usage: was an index dropped? 5. Check statistics: ANALYZE TABLE. 6. Check data skew: histogram statistics. 7. CloudWatch: CPUUtilization, ReadIOPS — is hardware maxed? Most likely cause: Table grew 10x and execution plan changed. Missing index or outdated statistics. Fix: Add index + ANALYZE TABLE.

---

## Q2. Your company's storage costs are ₹10 lakhs/month on S3. How do you reduce it?

**Answer:**

Audit: S3 Storage Lens → find top-cost buckets and prefixes. Actions: 1. Enable Intelligent-Tiering on unknown-access buckets (immediate). 2. Lifecycle rules on log buckets → Glacier after 30 days. 3. Delete incomplete multipart uploads (often forgotten): aws s3api list-multipart-uploads. 4. Delete old versions if versioning is on and versions are not needed. 5. Compress data before upload (gzip JSON/CSV files = 80% size reduction). Typical result: 40-60% cost reduction. Document and automate all changes.

---

## Q3. How do you choose between RDS MySQL, PostgreSQL, and Aurora?

**Answer:**

MySQL: Default choice for web apps, WordPress, existing MySQL. Huge ecosystem. Aurora MySQL: When you need MySQL + 5x performance + 15 read replicas + global. PostgreSQL: Complex queries, JSON, GIS, analytics, strict SQL compliance. Aurora PostgreSQL: PostgreSQL + 3x performance + cloud-native. Decision framework: New project? Aurora MySQL unless budget is tight → RDS MySQL. Complex analytics? PostgreSQL. Existing system? Stay on current engine for Aurora migration later.

---

## Q4. Explain how you'd implement a data archival strategy for a 100TB database.

**Answer:**

Step 1: Identify cold data. Queries unused in last 1 year = candidate for archive. Step 2: Export to S3. Use AWS DMS or mysqldump + S3 sync. Step 3: Glacier lifecycle on S3 data. Standard → Glacier after 90 days. Step 4: Partition main database table. Partition by date. Drop old partitions after archive confirmed. Step 5: AWS Athena. Query archived S3 data when needed for compliance. Result: DB shrinks from 100TB to 10TB active data. Query performance improves dramatically.

---

## Q5. A database is running at 95% CPU. What do you do right now vs long term?

**Answer:**

Right now: Kill top CPU-consuming queries. SHOW PROCESSLIST (MySQL) or pg_stat_activity (PostgreSQL). Kill runaway queries. Scale up instance temporarily (RDS → Modify → larger instance). Long term: Performance Insights → identify recurring expensive queries. Add indexes for top SQL. Consider adding read replica for reporting queries. Enable ElastiCache for frequent reads. Review application for N+1 queries. Schedule heavy reports for off-peak hours.

---

## 📣 LinkedIn Post After Practicing These

```
🎯 Day 5 Interview Prep Done! #S3StorageClasses&RDSIntro #CloudDevOpsHub

Practiced 5 real-world AWS interview questions today!

Most important thing I learned:
[Write your key takeaway from S3 Storage Classes & RDS Intro]

These aren't textbook answers — they're real scenarios.
Thanks to @Vikas Ratnawat | @CloudDevOpsHub for the depth! 🙏

#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHub
#CloudDevOpsHubCommunity #VikasRatnawat #AWS #InterviewPrep
```

---

> 🌟 **Fork → Customize → Push → Share on LinkedIn**  
> [← Back to Day 5 README](./README.md)