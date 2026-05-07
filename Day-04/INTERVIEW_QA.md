# 🎯 Day 4 — Interview Questions & Answers: AWS S3 + Portfolio

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

## Q1. Your S3 bucket is costing $500/month unexpectedly. How do you investigate?

**Answer:**

Check: 1. S3 Storage Lens for visualization. 2. Cost Explorer filter by S3 service. 3. Check S3 access logs or CloudTrail for high transfer. 4. Data transfer OUT is expensive — maybe CloudFront? 5. Check S3 Intelligent Tiering overhead. Common culprit: Large files transferred out to internet repeatedly. Fix: CloudFront in front of S3 for repeated downloads. Lifecycle rules if old data in Standard class.

---

## Q2. A user accidentally deleted important files from S3. How do you recover?

**Answer:**

If versioning enabled: S3 → object → show versions → find deleted version → restore. Delete the delete marker. If no versioning: Check S3 Replication destination bucket. Check backup in Glacier if lifecycle rule moved it. No backup: Data is gone permanently — this is why we enable versioning on all production buckets. Prevention: Enable versioning + MFA Delete on all critical buckets immediately.

---

## Q3. How do you migrate 50TB of data from on-premises to S3?

**Answer:**

Use AWS Snowball Edge (physical device). 1. Order Snowball from console. 2. AWS ships 80TB device to you. 3. Connect to your network, copy data using Snowball client. 4. Ship device back to AWS. 5. AWS loads data to S3. Time: 1 week vs months over internet. Cost: $300/device vs thousands in transfer fees. For smaller data (<1TB): AWS DataSync (online). For ongoing replication: Storage Gateway.

---

## Q4. You need to share S3 files with external partners without making bucket public.

**Answer:**

Use pre-signed URLs. Backend generates URL: aws s3 presign s3://bucket/file.pdf --expires-in 86400. Share URL. Expires in 24 hours. Partner can download, can't modify. For bulk sharing: S3 Access Points with specific permissions. For frequent external access: CloudFront with OAC (Origin Access Control) — partner gets CloudFront URL, S3 remains private.

---

## Q5. Design a data lake on S3 for a retail company.

**Answer:**

Raw zone: s3://datalake/raw/ — ingested as-is from all sources. Processed zone: s3://datalake/processed/ — cleaned, standardized (Parquet format). Curated zone: s3://datalake/curated/ — business-ready, aggregated. Catalog: AWS Glue Data Catalog — schema registry. Query: Amazon Athena — SQL on S3. Visualization: Amazon QuickSight. IAM: Each team accesses only their prefix. Lifecycle: Raw → 90 days Standard-IA → 365 days Glacier.

---

## 📣 LinkedIn Post After Practicing These

```
🎯 Day 4 Interview Prep Done! #AWSS3+Portfolio #CloudDevOpsHub

Practiced 5 real-world AWS interview questions today!

Most important thing I learned:
[Write your key takeaway from AWS S3 + Portfolio]

These aren't textbook answers — they're real scenarios.
Thanks to @Vikas Ratnawat | @CloudDevOpsHub for the depth! 🙏

#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHub
#CloudDevOpsHubCommunity #VikasRatnawat #AWS #InterviewPrep
```

---

> 🌟 **Fork → Customize → Push → Share on LinkedIn**  
> [← Back to Day 4 README](./README.md)