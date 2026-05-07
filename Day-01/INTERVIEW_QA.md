# 🎯 Day 1 — Interview Questions & Answers: AWS Introduction & EC2 Basics

> **CloudDevOpsHub Cohort | Mentor: Vikas Ratnawat**  
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

## Q1. What is AWS and why do companies use it?

**Answer:**

AWS (Amazon Web Services) is the world's leading cloud platform offering 200+ services including compute, storage, databases, networking, and AI.

**Why companies use it:**
- No upfront hardware cost — pay only for what you use
- Scale up or down in minutes (elasticity)
- Global presence across 30+ regions worldwide
- 99.99% availability SLA on most services
- Faster time to market — deploy in minutes vs weeks

**Real Example:** Swiggy, Zomato, HDFC Bank all run on AWS to handle millions of requests daily without managing physical servers.

---

## Q2. What is the difference between Traditional IT and Cloud Computing?

**Answer:**

| Factor | Traditional IT | Cloud (AWS) |
|--------|---------------|-------------|
| Setup time | Weeks/Months | Minutes |
| Cost model | CapEx (upfront) | OpEx (pay-as-you-go) |
| Scaling | Manual, slow | Auto, instant |
| Maintenance | Your team | AWS manages |
| Availability | Limited | Multi-AZ, 99.99% |
| Disaster Recovery | Expensive | Built-in |

**Interview Tip:** Always say "Cloud shifts CapEx to OpEx" — this impresses interviewers.

---

## Q3. What is an EC2 instance? What are instance types?

**Answer:**

EC2 (Elastic Compute Cloud) is a virtual server in AWS cloud. It's like renting a computer in AWS's data center.

**Instance Types:**
- **t2.micro / t3.micro** → Free tier, dev/test workloads
- **m5.large** → General purpose, web servers, app servers
- **c5.xlarge** → Compute optimized, ML inference, batch jobs
- **r5.large** → Memory optimized, in-memory databases (Redis)
- **p3.2xlarge** → GPU instances, AI/ML training

**Naming convention:** `[Family][Generation].[Size]`  
Example: `t3.micro` = T-family, Gen 3, micro size

**Real Use:** Netflix uses c5 instances for video encoding, RDS uses r5 for database memory.

---

## Q4. What is AMI? How is it different from an EC2 Instance?

**Answer:**

**AMI (Amazon Machine Image)** is a template/blueprint that contains the OS, configuration, and software needed to launch an EC2 instance.

**Analogy:** AMI = Photo/Snapshot, EC2 = Running computer from that photo

| AMI | EC2 Instance |
|-----|-------------|
| Template/Image | Running virtual machine |
| Stored in S3 | Runs on physical host |
| Used to launch EC2 | Created from AMI |
| Can be shared/copied | Region-specific running |

**Real Use Case:**  
A company builds a "Golden AMI" with all software pre-installed → launches 100 EC2s from it → consistent, fast deployment. This is used in Auto Scaling Groups!

---

## Q5. What is a Key Pair in EC2? Why is it important?

**Answer:**

A **Key Pair** is a set of cryptographic keys (public + private) used to securely SSH into Linux EC2 instances or decrypt Windows passwords.

- **Public Key** → Stored on EC2 (in `~/.ssh/authorized_keys`)
- **Private Key (.pem)** → You keep it. AWS never stores it.

```bash
# Connect to EC2 using key pair
ssh -i "my-key.pem" ec2-user@<public-ip>
```

**⚠️ Critical Interview Point:**
- If you lose the .pem file → you CANNOT connect to that EC2
- Never commit .pem files to GitHub — it's a massive security risk
- In production, use AWS Systems Manager Session Manager instead of SSH

---

## Q6. What are Security Groups? How do they work?

**Answer:**

A **Security Group** is a virtual firewall that controls inbound and outbound traffic to EC2 instances.

**Key Properties:**
- Stateful — if you allow inbound, return traffic is auto-allowed
- Default: DENY all inbound, ALLOW all outbound
- Works at instance level (not subnet level)
- ALLOW rules only — cannot write DENY rules

**Common Rules:**
```
Inbound:
Port 22  (SSH)   → My IP only (never 0.0.0.0/0 in production!)
Port 80  (HTTP)  → 0.0.0.0/0
Port 443 (HTTPS) → 0.0.0.0/0
Port 3306 (MySQL)→ App server SG only

Outbound:
All traffic → 0.0.0.0/0 (default)
```

**Interview Trap:** "Can Security Groups DENY traffic?"  
→ **No!** Use NACLs for deny rules.

---

## Q7. What is the AWS Global Infrastructure? Explain Regions, AZs, and Edge Locations.

**Answer:**

**Region:** A geographic area with multiple data centers.  
Example: ap-south-1 = Mumbai, us-east-1 = N. Virginia

**Availability Zone (AZ):** Isolated data center(s) within a Region.  
Mumbai region has 3 AZs: ap-south-1a, ap-south-1b, ap-south-1c  
Each AZ = physically separate building, separate power/network

**Edge Location:** Used by CloudFront CDN to cache content closer to users.  
India has edge locations in Mumbai, Chennai, Delhi, Hyderabad

**Architecture Rule:**
- Deploy across 2+ AZs = High Availability
- Deploy across 2+ Regions = Disaster Recovery

**Real Example:** If ap-south-1a has a power failure → ap-south-1b takes over. User never notices.

---

## Q8. What is the difference between Stop, Reboot, and Terminate for EC2?

**Answer:**

| Action | Effect | Data on EBS | Public IP | Billing |
|--------|--------|-------------|-----------|---------|
| **Stop** | Powers off, can restart | Preserved ✅ | Released ❌ | No instance charge |
| **Reboot** | Restarts OS | Preserved ✅ | Same ✅ | Continues |
| **Terminate** | Permanently deleted | Deleted ❌ (by default) | Released ❌ | Stops |

**⚠️ Interview Tip:**  
- Stopped EC2 still charges for EBS storage
- Use Elastic IP if you need static IP that survives Stop/Start
- Instance store data (not EBS) is LOST on stop/terminate

---

## Q9. What is Elastic IP? When should you use it?

**Answer:**

**Elastic IP (EIP)** is a static, persistent public IPv4 address that you own in AWS.

**Why needed:** Normal EC2 public IPs change every time you stop/start.

**When to use:**
- DNS record pointing to your server (can't change IP)
- Whitelisting on client firewalls
- Failover — remap EIP to a new instance instantly

**Cost Note:**  
- FREE when attached to a running instance
- Charged ($0.005/hr) when NOT attached (wasted resource!)
- Each account gets 5 EIPs per region by default

**Interview Answer:** "We use EIP when we need a stable IP for DNS or firewall whitelisting, otherwise we prefer Load Balancers with DNS names for flexibility." 

---

## Q10. What is the EC2 pricing model? Explain On-Demand vs Reserved vs Spot.

**Answer:**

| Model | Cost | Use Case | Savings |
|-------|------|----------|---------|
| **On-Demand** | Highest | Dev, testing, unpredictable | 0% |
| **Reserved (1yr)** | Medium | Production, stable workloads | ~40% |
| **Reserved (3yr)** | Low | Long-term committed workloads | ~60% |
| **Spot** | Lowest | Batch jobs, ML training, CI/CD | ~70-90% |
| **Savings Plans** | Medium | Flexible, commits to $/hr | ~40-60% |

**Real Scenario (Interview Gold):**
"Our company runs a web app. We use:
- Reserved instances for base load (always running web servers)
- On-Demand for traffic spikes
- Spot instances for nightly data processing jobs
This combination saves 45% vs running everything On-Demand."

**Spot Instance Risk:** AWS can terminate with 2-minute warning. Never use for critical stateful workloads.

---

## 📣 LinkedIn Post After Practicing These

```
🎯 Day 1 Interview Prep Done! #AWSIntroduction&EC2Basics #CloudDevOpsHub

Practiced 10 real-world AWS interview questions today!

Most important thing I learned:
[Write your key takeaway from AWS Introduction & EC2 Basics]

These aren't textbook answers — they're real scenarios.
Thanks to @Vikas Ratnawat | @CloudDevOpsHub for the depth! 🙏

#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHub
#CloudDevOpsHubCommunity #VikasRatnawat #AWS #InterviewPrep
```

---

> 🌟 **Fork → Customize → Push → Share on LinkedIn**  
> [← Back to Day 1 README](./README.md)