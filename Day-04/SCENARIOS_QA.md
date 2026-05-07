# 🔥 Day 4 — Real-World Scenario Q&A: AWS S3 + Portfolio

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

## Scenario 1: Your S3 bucket is costing $500/month unexpectedly. How do you investigate?

**Your Answer (How to Approach This):**

Check: 1. S3 Storage Lens for visualization. 2. Cost Explorer filter by S3 service. 3. Check S3 access logs or CloudTrail for high transfer. 4. Data transfer OUT is expensive — maybe CloudFront? 5. Check S3 Intelligent Tiering overhead. Common culprit: Large files transferred out to internet repeatedly. Fix: CloudFront in front of S3 for repeated downloads. Lifecycle rules if old data in Standard class.

---

## Scenario 2: A user accidentally deleted important files from S3. How do you recover?

**Your Answer (How to Approach This):**

If versioning enabled: S3 → object → show versions → find deleted version → restore. Delete the delete marker. If no versioning: Check S3 Replication destination bucket. Check backup in Glacier if lifecycle rule moved it. No backup: Data is gone permanently — this is why we enable versioning on all production buckets. Prevention: Enable versioning + MFA Delete on all critical buckets immediately.

---

## Scenario 3: How do you migrate 50TB of data from on-premises to S3?

**Your Answer (How to Approach This):**

Use AWS Snowball Edge (physical device). 1. Order Snowball from console. 2. AWS ships 80TB device to you. 3. Connect to your network, copy data using Snowball client. 4. Ship device back to AWS. 5. AWS loads data to S3. Time: 1 week vs months over internet. Cost: $300/device vs thousands in transfer fees. For smaller data (<1TB): AWS DataSync (online). For ongoing replication: Storage Gateway.

---

## Scenario 4: You need to share S3 files with external partners without making bucket public.

**Your Answer (How to Approach This):**

Use pre-signed URLs. Backend generates URL: aws s3 presign s3://bucket/file.pdf --expires-in 86400. Share URL. Expires in 24 hours. Partner can download, can't modify. For bulk sharing: S3 Access Points with specific permissions. For frequent external access: CloudFront with OAC (Origin Access Control) — partner gets CloudFront URL, S3 remains private.

---

## Scenario 5: Design a data lake on S3 for a retail company.

**Your Answer (How to Approach This):**

Raw zone: s3://datalake/raw/ — ingested as-is from all sources. Processed zone: s3://datalake/processed/ — cleaned, standardized (Parquet format). Curated zone: s3://datalake/curated/ — business-ready, aggregated. Catalog: AWS Glue Data Catalog — schema registry. Query: Amazon Athena — SQL on S3. Visualization: Amazon QuickSight. IAM: Each team accesses only their prefix. Lifecycle: Raw → 90 days Standard-IA → 365 days Glacier.

---

## Scenario 6: How do you implement data governance for S3 at enterprise scale?

**Your Answer (How to Approach This):**

1. AWS Macie: Auto-detect PII, credit cards, secrets in S3. Alerts when sensitive data found in unexpected location. 2. S3 Object Tagging: Tag all objects: DataClassification=PII, Owner=HRTeam. 3. Bucket policies: Deny if object doesn't have classification tag. 4. S3 Inventory: Weekly report of all objects, tags, encryption status. 5. AWS Config rules: s3-bucket-server-side-encryption-enabled, s3-bucket-public-read-prohibited. Auto-remediate violations.

---

## Scenario 7: Your static website on S3 is getting DDoS attacked. How do you protect it?

**Your Answer (How to Approach This):**

1. Put CloudFront in front of S3 — absorbs DDoS at edge. 2. AWS Shield Standard (free) — protects against L3/L4 attacks automatically. 3. AWS WAF on CloudFront: Rate limiting, IP blocking, geo-blocking. 4. S3: Remove direct public access — route all traffic via CloudFront. 5. CloudFront geo-restriction: Block traffic from countries with no legitimate users. Cost: WAF ~$5/month + $0.60/million requests. Much cheaper than downtime.

---

## Scenario 8: How do you ensure S3 data is encrypted at rest and in transit?

**Your Answer (How to Approach This):**

At rest: Enable default encryption on bucket: SSE-S3 (AWS-managed key) or SSE-KMS (your key, full audit trail). Bucket policy: Deny s3:PutObject if header aws:SecureTransport is false. In transit: Force HTTPS only bucket policy: Deny all actions if aws:SecureTransport = false. Verify: aws s3api get-bucket-encryption --bucket my-bucket. Compliance: SSE-KMS + key rotation + CloudTrail + access logs satisfies PCI-DSS and HIPAA.

---

## Scenario 9: A development team pushed secrets to S3 accidentally. What do you do?

**Your Answer (How to Approach This):**

Immediate: Make S3 object private (if it was public). Check S3 access logs: was it accessed? By whom? Rotate the exposed secret immediately (API key, password, whatever it was). Check CloudTrail for any API calls made with that secret. Long-term: Set up Macie to scan S3 for secrets automatically. Set up GitHub secret scanning. Use Secrets Manager — never put secrets in S3 or Git. Add pre-commit hook: git-secrets.

---

## Scenario 10: Design S3 architecture for a video streaming platform (like Hotstar).

**Your Answer (How to Approach This):**

Architecture: Ingest bucket (private) → Lambda trigger → transcode job (MediaConvert) → Output buckets: 360p, 720p, 1080p, 4K (each prefix). CloudFront distribution: serve all qualities. HLS/DASH streaming: playlists + segments in S3. Cache: CloudFront caches hot content. Cold content: S3 Intelligent-Tiering auto-moves old shows to cheaper tiers. DRM: Signed URLs with short expiry. Upload pipeline: Pre-signed URL for creator upload. Metadata: DynamoDB (show info, episode list). Search: ElasticSearch or OpenSearch.

---

## 📣 LinkedIn Post After Completing Scenarios

```
💡 Day 4 Scenario Practice Done! #CloudDevOpsHub

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
> [← Back to Day 4 README](./README.md)