# 🏗️ Day 8 — AWS VPC Overview & Networking Basics

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

Understand **Virtual Private Cloud (VPC)** — your own private network inside AWS. This is where most interviews go deep. VPC = networking in cloud. Get this right and you stand out in any DevOps/Cloud interview. 🔌

---

## 📚 Topics Covered

| # | Topic |
|---|-------|
| 🏗️ | What is VPC — Virtual Private Cloud Explained |
| 🔌 | Basics of Networking — IP, Subnet, Gateway & Routing |
| 🧱 | OSI Model — 7 Layers & Real-World Mapping |
| 🏢 | Types of VPC — Default VPC vs Custom VPC |
| 🧮 | CIDR — IP Addressing & Subnetting Basics |

---

## 🛠️ Daily Task — Practical Challenge

### 📋 Minor Project 2: Design a VPC Architecture for Resume

**Real-World VPC Architecture (3-tier app):**
```
INTERNET
    │
    ▼
Internet Gateway (IGW)
    │
    ▼
┌─────────────────────────────────────────┐
│              VPC: 10.0.0.0/16           │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │    Public Subnet: 10.0.1.0/24   │   │
│  │    (Web Tier — Nginx/ALB)        │   │
│  └─────────────────────────────────┘   │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │    Private Subnet: 10.0.2.0/24  │   │
│  │    (App Tier — Node.js/Spring)   │   │
│  └─────────────────────────────────┘   │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │    Private Subnet: 10.0.3.0/24  │   │
│  │    (DB Tier — RDS MySQL)         │   │
│  └─────────────────────────────────┘   │
│                                         │
└─────────────────────────────────────────┘
```

---

### ✔ Networking Concepts You Must Know

**CIDR Quick Reference:**
```
10.0.0.0/16  → 65,536 IPs (Large VPC)
10.0.0.0/24  → 256 IPs (Subnet)
10.0.0.0/28  → 16 IPs (Small subnet)

Rule: /xx → smaller number = more IPs
        /16 > /24 > /28
```

**Subnetting Your VPC:**
```
VPC CIDR: 10.0.0.0/16

Public Subnet 1 (AZ-1a):  10.0.1.0/24
Public Subnet 2 (AZ-1b):  10.0.2.0/24
Private Subnet 1 (AZ-1a): 10.0.3.0/24
Private Subnet 2 (AZ-1b): 10.0.4.0/24
DB Subnet 1 (AZ-1a):      10.0.5.0/24
DB Subnet 2 (AZ-1b):      10.0.6.0/24
```

---

### ✔ Key Networking Components

**Internet Gateway (IGW):**
```
→ Allows resources in PUBLIC subnets to reach internet
→ 1 IGW per VPC
→ Fully managed, no cost
```

**Route Table:**
```
Public Route Table:
  Destination     │ Target
  10.0.0.0/16     │ local
  0.0.0.0/0       │ igw-xxxx  (internet!)

Private Route Table:
  Destination     │ Target
  10.0.0.0/16     │ local
  0.0.0.0/0       │ nat-xxxx  (NAT Gateway — outbound only)
```

**Security Group (SG) vs NACL:**
```
Security Group:
→ Works at INSTANCE level
→ Stateful (return traffic auto-allowed)
→ ALLOW rules only (no deny)
→ Default: deny all inbound, allow all outbound

NACL:
→ Works at SUBNET level
→ Stateless (must define both inbound & outbound)
→ ALLOW and DENY rules
→ Rule numbers matter (lower = evaluated first)
```

---

## 🏆 Today's Challenge

```
🔥 CHALLENGE:
Design (on paper or draw.io) a VPC architecture for:
"A 3-tier web application: Web + App + Database"

Include:
1. VPC CIDR range
2. 2 Public subnets (web tier, 2 AZs)
3. 2 Private subnets (app tier, 2 AZs)
4. 2 DB subnets (db tier, 2 AZs)
5. IGW for public subnets
6. NAT Gateway for private subnets

Upload the diagram to LinkedIn — architecture diagrams get HIGH engagement!
```

---

## 📸 Screenshot Checklist

- [ ] VPC created with custom CIDR
- [ ] Subnets created (public + private)
- [ ] Route tables configured
- [ ] Security group rules screenshot
- [ ] Architecture diagram (bonus)

---


## 🧱 OSI Model — Quick DevOps Relevance

| Layer | Name | AWS/DevOps Relevance |
|-------|------|---------------------|
| 7 | Application | ALB, API Gateway, Route 53 |
| 4 | Transport | NLB, Security Groups (ports) |
| 3 | Network | VPC, Subnets, Route Tables, NACLs |
| 2 | Data Link | MAC addresses (handled by AWS) |
| 1 | Physical | AWS Data Centers (not your concern) |

---

## 🔗 Resources

- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/)
- [CIDR Calculator](https://cidr.xyz)
- [draw.io for Architecture Diagrams](https://app.diagrams.net)
> [← Day 7](../Day-07/README.md) | [Back to Main](../README.md) | [Day 9 →](../Day-09/README.md)


---

## 📣 LinkedIn Post — Copy & Customize

```
🏗️ Day 8 of My 10-Day AWS Challenge! #CloudDevOpsHub

Today I learned the most asked topic in Cloud interviews — AWS VPC! 🌐

✅ Designed a 3-Tier VPC Architecture from scratch
✅ Learned CIDR notation and IP subnetting
✅ Configured Route Tables, IGW, Security Groups
✅ Understood the difference between Public and Private Subnets

The "aha moment" today 💡:
Before VPC — all your AWS resources were on a SHARED network
With VPC — you get your OWN isolated network in AWS cloud

Think of it like this:
→ VPC = Your apartment building
→ Subnets = Individual floors
→ Security Groups = Door locks on each apartment
→ NACL = Security guard at each floor entrance
→ Internet Gateway = Main building entrance

Interview insight 🎯:
"Why do we put RDS in a private subnet?"
→ DB should NEVER be directly accessible from internet
→ Only the app tier (EC2) should talk to RDS
→ This is called defense-in-depth architecture!

Minor Project 2: Networking Architecture — added to my resume! 📄

#Day8 #AWSChallenge #10DayAWSChallenge #CloudDevOpsHub #AWS #VPC 
#Networking #CloudArchitecture #DevOps #LearningInPublic #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
```

> 💬 **LinkedIn Profile Tip:**  
> Architecture diagrams as images in posts get **5x more impressions** than text-only posts.  
> Draw your VPC diagram on draw.io → export as PNG → attach to post!

---


---

> 🌟 **Star this repo | Fork & Customize | Tag @Vikas Ratnawat + @CloudDevOpsHub**  
> [← Day 7](../Day-07/README.md) | [🏠 Back to Main](../README.md) | [Day 9 →](../Day-09/README.md)
