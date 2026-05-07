# 🔐 Day 3 — AWS IAM (Identity & Access Management)

> **Cohort:** AWS Cloud Cohort-6 by CloudDevOpsHub

---

## 🚀 How to Use This Repo

### 🍴 Step 1 — Fork This Repo
```
1. Click "Fork" at the top-right of this GitHub repo
2. It will be copied to your own GitHub account
3. Clone it locally:
   git clone https://github.com/YOUR_USERNAME/aws-10day-challenge
4. Do the practicals → Update your README with your own screenshots & notes
5. Push your changes:
   git add . && git commit -m "Day X complete ✅" && git push
```
> 💡 Your forked repo becomes your **public portfolio** — share it on LinkedIn!

---

### 🔐 Step 2 — Login & Watch Recorded Sessions
**Portal Login:**
👉 [https://devopswithvikas.com/eud/login/email](https://devopswithvikas.com/eud/login/email)

**Bookmark Your Recordings Page:**
👉 [https://devopswithvikas.com/eud/bookings](https://devopswithvikas.com/eud/bookings)

```
→ Login with your registered email
→ Go to "My Bookings" → Open AWS Cohort-6
→ Watch the Day-wise Recorded Video Session for today
→ Pause. Practice. Repeat.
```
> 📌 Recordings are valid for **8 weeks** — watch at your own pace!

---

### 👥 Step 3 — Invite Your Friends
Know someone who should learn AWS? 🔥

**Share this enrollment link with them:**
👉 [https://devopswithvikas.com/offer/top-10-aws-services-cohort-2026](https://devopswithvikas.com/offer/top-10-aws-services-cohort-2026)

```
✅ Top 10 AWS Services covered
✅ Hands-on practicals every day
✅ WhatsApp group support
✅ Resume-worthy projects
✅ Certificate on completion
✅ Mentored by Vikas Ratnawat (Ex-Amazon | Microsoft | Google | Microsoft MVP)
```

---

## 🎯 Today's Goal

Understand **who can do what on your AWS account**. Set up IAM users, configure permissions, create an account alias — and build a proper load balancer with 2 EC2 instances. Security + Architecture today! 🛡️

---

## 📚 Topics Covered

| # | Topic |
|---|-------|
| 🔐 | AWS IAM Overview — Identity & Access Management Basics |
| 👤 | IAM Users & Service Access Configuration |
| 🧩 | IAM Sections — Users, Groups, Roles, Policies |
| ⚖️ | Load Balancers — Deep Dive & Architecture |
| 🔀 | Types: ALB, NLB, CLB — Use Cases & Comparison |

---

## 🛠️ Daily Task — Practical Challenge

### ✔ Practical 1: Create IAM User with Limited Access

```
Step 1: AWS Console → IAM → Users → Create User
Step 2: Username → "dev-user-01"
Step 3: Access Type → AWS Console Access
Step 4: Set custom password
Step 5: Attach Policy → "AmazonEC2ReadOnlyAccess"
Step 6: Create User → Download credentials CSV
Step 7: Login with new IAM user → Verify limited access
```

> ⚠️ **Rule #1 in AWS Security:** NEVER use root account for daily work. Always use IAM users!

---

### ✔ Practical 2: Create AWS Account Alias (Corporate Login URL)

```
Step 1: IAM → Dashboard → Account Alias → Create
Step 2: Set alias → "clouddevopshub-demo" (or your company name)
Step 3: New login URL becomes:
        https://clouddevopshub-demo.signin.aws.amazon.com/console
Step 4: Share this URL with your IAM users instead of account ID
```

> 💡 **Interview Answer:** "Why Account Alias?" — For professional login URLs instead of 12-digit account IDs. Used in all enterprise AWS setups.

---

### ✔ Practical 3: Create ELB with 2 EC2 Instances

**Architecture:**
```
Internet → ALB (Port 80) → Target Group → EC2-1 (App Server 1)
                                        → EC2-2 (App Server 2)
```

```
Step 1: Launch 2 EC2 instances (Amazon Linux + Nginx)
Step 2: Different HTML on each:
  EC2-1: <h1>Server 1 — Hello from AZ-1a</h1>
  EC2-2: <h1>Server 2 — Hello from AZ-1b</h1>

Step 3: EC2 → Load Balancers → Create ALB
Step 4: Select both Availability Zones
Step 5: Create Target Group → Register both EC2s
Step 6: Attach TG to ALB

Step 7: Test → Keep refreshing ALB DNS URL
        → You'll see Server 1 and Server 2 alternating!
```

---

## 🏆 Today's Challenge

```
🔥 CHALLENGE:
1. Create an IAM User with EC2 Read-Only access
2. Login with that user and try to START an EC2 — it should FAIL (read-only!)
3. Create Account Alias for your AWS account
4. Screenshot the "Access Denied" error — that's proof IAM is working!
5. Post on LinkedIn!
```

---

## 📸 Screenshot Checklist

- [ ] IAM User created with policy attached
- [ ] AWS Account Alias set (custom login URL)
- [ ] "Access Denied" error when IAM user tries restricted action
- [ ] ALB distributing traffic between 2 EC2s (both server responses)

---


## 🔑 IAM Cheat Sheet (Interview Ready)

| Concept | What it does |
|---------|-------------|
| **User** | A person or app that needs AWS access |
| **Group** | Collection of users with shared permissions |
| **Role** | Temporary permissions (used by EC2, Lambda, etc.) |
| **Policy** | JSON document defining what's allowed/denied |
| **Principle of Least Privilege** | Give minimum required access only |

---

## 🔗 Resources

- [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [AWS Policy Generator](https://awspolicygen.s3.amazonaws.com/policygen.html)
> [← Day 2](../Day-02/README.md) | [Back to Main](../README.md) | [Day 4 →](../Day-04/README.md)


---

## 📣 LinkedIn Post — Copy & Customize

```
🔐 Day 3 of My 10-Day AWS Challenge! #CloudDevOpsHub

Today I learned the most IMPORTANT concept in AWS — IAM Security! 🛡️

✅ Created IAM Users with custom permissions
✅ Set up AWS Account Alias (professional login URL)
✅ Configured Elastic Load Balancer with 2 EC2 instances
✅ Verified access control — IAM user got "Access Denied" as expected!

Why this matters in real companies 💡:
→ 100s of employees need AWS access
→ Each gets ONLY the permissions they need (Principle of Least Privilege)
→ Root account is locked away and never used daily
→ Load Balancer ensures no single point of failure

Real scenario: If EC2-1 crashes at 3AM, ALB auto-routes to EC2-2. 
Your users never notice! That's High Availability. 🚀

Day done! Building cloud skills daily with @Vikas Ratnawat | @CloudDevOpsHub Community 💪

#Day3 #AWSChallenge #10DayAWSChallenge #CloudDevOpsHub #AWS #IAM 
#Security #LoadBalancer #CloudComputing #DevOps #LearningInPublic #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
```

> 💬 **LinkedIn Profile Tip:**  
> Update your **About section** to mention you're actively learning AWS with hands-on projects.  
> Write something like: "Currently completing a 10-Day AWS Cloud Challenge — building real projects daily."

---


---

> 🌟 **Star this repo | Fork & Customize | Tag @Vikas Ratnawat + @CloudDevOpsHub**  
> [← Day 2](../Day-02/README.md) | [🏠 Back to Main](../README.md) | [Day 4 →](../Day-04/README.md)
