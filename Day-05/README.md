# ☁️ Day 5 — AWS S3 Storage Classes & RDS Overview

> **Cohort:** AWS Cloud Cohort by CloudDevOpsHub Community

---

## 🎯 Today's Goal

Master **S3 Storage Classes and Lifecycle Rules** — then get introduced to databases on AWS. This day is about cost optimization (huge in real jobs!) and understanding when to use object storage vs databases. 💰

---

## 📚 Topics Covered

| # | Topic |
|---|-------|
| ☁️ | S3 Storage Classes — When to Use Which |
| ❓ | Why Databases Are Required in Real Applications |
| 📘 | Database Basics — Tables, Records & Indexing |
| 🗂️ | Types of Databases — Relational vs NoSQL |
| 🧮 | SQL/MySQL — Relational Database Concepts |
| 🍃 | MongoDB — NoSQL Overview & Use Cases |

---

## 🛠️ Daily Task — Practical Challenge

### ✔ Practical 5: Configure S3 Storage Classes + Lifecycle Rules

**Real-World Scenario: IT Log File Management for a Bank**
```
Problem: A bank stores 10TB of logs/month
- Active logs → Need fast access (first 30 days)
- Old logs → Rarely accessed (30-90 days)
- Archived logs → Compliance only (90+ days)
- Expired logs → Delete after 1 year

Solution: S3 Lifecycle Rules!
```

**Step 1 — Understanding Storage Classes:**

| Storage Class | Use Case | Cost |
|--------------|----------|------|
| S3 Standard | Active data, frequent access | 💲💲💲 |
| S3 Standard-IA | Monthly access (backups) | 💲💲 |
| S3 One Zone-IA | Non-critical, infrequent | 💲 |
| S3 Glacier Instant | Archives, retrieved in ms | 💲 |
| S3 Glacier Flexible | Long archives, mins-hours | 💲 |
| S3 Glacier Deep Archive | 7-10yr compliance archives | 💲 (cheapest) |
| S3 Intelligent-Tiering | Unknown access patterns | Auto |

**Step 2 — Create Lifecycle Rule:**
```
1. Go to your S3 bucket → Management tab
2. Create Lifecycle Rule → Name: "log-archiving-policy"
3. Apply to: All objects (or prefix "logs/")
4. Transitions:
   → After 30 days → Move to S3 Standard-IA
   → After 90 days → Move to S3 Glacier Flexible Retrieval
   → After 365 days → Expire (delete)
5. Save rule
```

**Step 3 — Enable Versioning:**
```
1. Bucket → Properties → Versioning → Enable
2. Upload same file twice with different content
3. Check "Show versions" → See both versions!
4. Restore to previous version (1 click)
```

> 💡 **Real Scenario (Banking):** HDFC, ICICI banks use S3 Intelligent-Tiering for transaction logs — saves 40-60% on storage costs vs keeping everything in Standard!

---

## 🏆 Today's Challenge

```
🔥 CHALLENGE:
1. Create a lifecycle rule on your S3 bucket:
   - Day 0: Standard
   - Day 30: Transition to Standard-IA
   - Day 90: Transition to Glacier
   - Day 365: Delete
2. Screenshot the lifecycle rule configuration
3. Answer this in your LinkedIn post:
   "What storage class would you use for logs you access once a year?"
```

---

## 📸 Screenshot Checklist

- [ ] S3 bucket with versioning enabled
- [ ] Lifecycle rule created (showing all transitions)
- [ ] Multiple versions of a file visible
- [ ] Storage class configuration visible

---


## 🗄️ Database Quick Intro (Preview for Day 6)

```
Why do apps need databases?
→ Your app has millions of users
→ Each user has data (name, email, orders, etc.)
→ You can't store this in S3 files!
→ You need a database — queryable, structured, fast

S3 vs RDS:
│ S3                    │ RDS                      │
│ Files/Objects/Blobs   │ Tables, Rows, Columns     │
│ "Store my photo"      │ "Find all orders > ₹500"  │
│ Static assets         │ Application data          │
```

---

## 🔗 Resources

- [S3 Storage Classes Comparison](https://aws.amazon.com/s3/storage-classes/)
- [S3 Lifecycle Rules Docs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)
> [← Day 4](../Day-04/README.md) | [Back to Main](../README.md) | [Day 6 →](../Day-06/README.md)


---

## 📣 LinkedIn Post — Copy & Customize

```
💰 Day 5 of My 10-Day AWS Challenge! #CloudDevOpsHub

Today I learned how companies SAVE LAKHS on AWS storage costs! 💡

✅ Configured AWS S3 Storage Classes
✅ Set up Lifecycle Rules for automatic data tiering
✅ Enabled S3 Versioning for data protection
✅ Got introduced to AWS RDS (Databases in Cloud)

Real Scenario I practiced today 🏦:
→ A bank stores 10TB of logs monthly
→ Active logs → S3 Standard (₹₹₹)
→ 30-day-old logs → S3 Standard-IA (₹₹)  
→ 90-day-old logs → S3 Glacier (₹)
→ 1-year-old logs → Auto-delete

Result: 60% cost reduction! This is literally what cloud engineers do in companies!

Quiz 🤔: What S3 storage class would YOU use for data accessed once a year?
Comment below 👇

Continuing the 10-Day AWS Journey with @Vikas Ratnawat | @CloudDevOpsHub Community 🚀

#Day5 #AWSChallenge #10DayAWSChallenge #CloudDevOpsHub #AWS #S3 
#CostOptimization #CloudArchitecture #DevOps #LearningInPublic #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
```

> 💬 **LinkedIn Profile Tip:**  
> Asking a question in your LinkedIn post gets **3x more comments**.  
> Comments = algorithm boost = more profile views = more recruiter eyes! 👀

---


---

> 🌟 **Star this repo | Fork & Customize | Tag @Vikas Ratnawat + @CloudDevOpsHub**  
> [← Day 4](../Day-04/README.md) | [🏠 Back to Main](../README.md) | [Day 6 →](../Day-06/README.md)
