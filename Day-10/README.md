# 👀 Day 10 — AWS CloudWatch + 🏆 YOUR COMPLETION CERTIFICATE!

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

Set up **real monitoring and alerts** on AWS — CPU spike alert, budget alarm, CloudWatch dashboard. Then claim your **completion certificate** and post your entire 10-day journey on LinkedIn with all your screenshots. You did it! 🎉

---

## 📚 Topics Covered

| # | Topic |
|---|-------|
| 👀 | What is Monitoring — Concepts & Importance in IT Systems |
| ❓ | Why Monitoring is Required — Performance, Security & Availability |
| 🛠️ | Other Monitoring Tools — Nagios, Zabbix, Prometheus, Grafana |
| ☁️ | AWS CloudWatch — Real-Time Monitoring & Alerts Setup |

---

## 🛠️ Daily Task — Practical Challenge

### ✔ Practical 10A: EC2 CPU Alert via CloudWatch

**Setup CPU alarm — get email when CPU > 80%:**

```
Step 1: CloudWatch → Alarms → Create Alarm
Step 2: Select metric → EC2 → Per-Instance Metrics
Step 3: Choose your EC2 instance → CPUUtilization
Step 4: Conditions:
  → Threshold type: Static
  → CPUUtilization is: Greater than 80
Step 5: Notification:
  → Create SNS topic: "ec2-cpu-alerts"
  → Email: your@email.com
  → Confirm email subscription (check inbox!)
Step 6: Alarm name: "High-CPU-Alert-Day10"
Step 7: Create Alarm ✅
```

**Test the alarm — simulate high CPU:**
```bash
# SSH into your EC2 and run:
sudo yum install stress -y
stress --cpu 4 --timeout 60s

# Watch CloudWatch graph spike in real time! 📈
# Check email — ALARM notification will arrive!
# When stress ends → OK notification comes back!
```

---

### ✔ Practical 10B: AWS Budget Alarm ₹100

**Never get a surprise AWS bill again:**

```
Step 1: AWS Console → Billing → Budgets → Create Budget
Step 2: Budget type → Cost budget
Step 3: Name → "Monthly-Limit-100-INR"
Step 4: Budget amount → ₹100
Step 5: Alerts:
  → 80% of budget → Email warning (₹80 spent)
  → 100% of budget → Email immediately (₹100 reached!)
Step 6: Email → your@email.com
Step 7: Create Budget ✅
```

> 💡 **FinOps Tip:** Every cloud company sets budget alerts. This practice is called Financial Operations (FinOps) — a growing role in cloud! You now know how to do it.

---

### ✔ Bonus: Build Your CloudWatch Dashboard

```
Step 1: CloudWatch → Dashboards → Create Dashboard
Step 2: Name → "My-AWS-10Day-Dashboard"
Step 3: Add widgets:
  → EC2 CPU Utilization (Line chart)
  → EC2 Network In/Out (Line chart)
  → Billing Estimate (Number widget)
Step 4: Save Dashboard
```

> 📸 Screenshot this dashboard — it looks great as a portfolio proof!

---

## 🏆 How to Get Your Completion Certificate

```
Step 1: Login to your portal
         👉 https://devopswithvikas.com/eud/login/email

Step 2: Go to your bookings page
         👉 https://devopswithvikas.com/eud/bookings

Step 3: Mark all 10 days complete

Step 4: Download your 
         "AWS Cloud Cohort-6 — 10-Day Completion Certificate"

Step 5: Save as PDF + take a screenshot

Step 6: Post on LinkedIn with certificate + 10-day screenshots! 🎉
```

---

## 📸 Collect All 10-Day Screenshots for Final Post

```
📸 Day 1  → EC2 Instance in "Running" state
📸 Day 2  → Your HTML website live via EC2 Public IP
📸 Day 3  → IAM user created + ALB with 2 EC2 targets
📸 Day 4  → Resume/Portfolio live on AWS S3 URL
📸 Day 5  → S3 Lifecycle Rules configured
📸 Day 6  → RDS connected from EC2 terminal
📸 Day 7  → SQL queries running on Aurora/MySQL/PostgreSQL
📸 Day 8  → Custom VPC with subnets and route tables
📸 Day 9  → Private EC2 accessed via Bastion Host
📸 Day 10 → CloudWatch ALARM triggered + email received
🏆 Bonus  → Certificate screenshot!
```

---

## 🎯 Final LinkedIn Profile Update Checklist

```
✅ HEADLINE:
   "[Role] | AWS Cloud | DevOps | [City]"
   Example: "Software Engineer | AWS Cloud | DevOps | Indore"

✅ FEATURED SECTION:
   • Your live S3 portfolio/resume URL
   • CloudWatch Dashboard screenshot
   • Completion Certificate (PDF or image)

✅ ABOUT SECTION — Add this paragraph:
   "Completed the 10-Day AWS Cloud Cohort (CloudDevOpsHub Cohort-6) 
   under Vikas Ratnawat (Ex-Amazon/Microsoft/Google, Microsoft MVP). 
   Built 10+ hands-on projects: EC2 Web Servers, S3 Static Hosting, 
   RDS Databases, VPC Architecture, IAM Security, CloudWatch Monitoring."

✅ SKILLS TO ADD:
   Amazon EC2 | Amazon S3 | AWS IAM | Amazon RDS
   Amazon VPC | Amazon CloudWatch | Linux | Nginx | Cloud Architecture

✅ PROJECTS SECTION:
   Title: "AWS 10-Day Cloud Challenge — CloudDevOpsHub Cohort-6"
   Description: 10 real hands-on projects across EC2, S3, IAM, RDS, VPC, CloudWatch
   Link: [Your GitHub Repo]

✅ CERTIFICATIONS — Next step:
   → AWS Cloud Practitioner (CLF-C02): https://aws.amazon.com/certification/

✅ OPEN TO WORK → Switch it ON! 🟢
```

---

## 🗺️ Your 30-Day Next Roadmap

```
Week 1-2: AWS Advanced
→ Route 53 | CloudFront | Lambda | ECS/EKS

Week 3-4: DevOps Core
→ Docker + Kubernetes on AWS
→ CI/CD: GitHub Actions + AWS CodePipeline
→ Terraform: Infrastructure as Code
→ Helm Charts for K8s

🏆 Certification Path:
1. AWS Cloud Practitioner (CLF-C02)   ← Register NOW
2. AWS Solutions Architect Associate
3. AWS DevOps Professional
4. CKA (Certified Kubernetes Administrator)
```

---

## 🔗 Important Links

| Resource | Link |
|----------|------|
| 🔗 Connect with Vikas | [linkedin.com/in/vikasratnawat](https://www.linkedin.com/in/vikasratnawat/) |
| 🌐 CloudDevOpsHub | [linkedin.com/in/clouddevopshub](https://www.linkedin.com/in/clouddevopshub/) |
| 📸 Instagram | [@clouddevopshub](https://www.instagram.com/clouddevopshub/) |
| 📍 Portal | [devopswithvikas.com](https://devopswithvikas.com/eud/login/email) |
| 📺 YouTube | CloudDevOpsHub |
| 🎓 AWS Certification | [aws.amazon.com/certification](https://aws.amazon.com/certification/) |
| ☁️ CloudWatch Docs | [docs.aws.amazon.com/cloudwatch](https://docs.aws.amazon.com/cloudwatch/) |

---

## 🏆 Final Words from Vikas

> *"You started this challenge as a learner.*  
> *You're finishing it as someone who has BUILT real things on AWS.*  
> *That's the CloudDevOpsHub way.*  
> *Ab interview mein confidence ke saath jaao — tumne real kaam kiya hai!*  
> *Proud of every single one of you."* 🔥  
>
> — **Vikas Ratnawat**  
> *Ex-Amazon | Microsoft | Google | PwC | Microsoft MVP*  
> *Founder, CloudDevOpsHub — 58,000+ Members | 10,000+ Students*
> [← Day 9](../Day-09/README.md) | [🏠 Back to Main](../README.md)

---

*Built with ❤️ by Vikas Ratnawat | CloudDevOpsHub*  
*🌐 devopswithvikas.com | 🔗 linkedin.com/in/vikasratnawat*  
*58,000+ Members | 43+ Cohorts | 10,000+ Students Trained*

> [← Day 9](../Day-09/README.md) | [🏠 Back to Main](../README.md)

---

## 📣 LinkedIn Post — Copy & Customize

```
🏆 CERTIFICATE EARNED! I completed the 10-Day AWS Cloud Challenge!
@Vikas Ratnawat | @CloudDevOpsHub | AWS Cohort-6 ✅

10 days. 10 practicals. 10 challenges.
Real AWS. Real projects. Real confidence. 💪

My complete journey 👇

☁️ Day 0: Joined cohort. AWS Account created. Challenge accepted! 🔥
🖥️ Day 1: Launched my first EC2 VM on AWS Cloud
🌐 Day 2: Deployed a LIVE website — Nginx + Application Load Balancer
🔐 Day 3: Configured IAM users, policies & ELB with 2 EC2 instances
📄 Day 4: Hosted my Resume/Portfolio LIVE on AWS S3 — accessible worldwide!
💰 Day 5: Saved storage costs with S3 Storage Classes + Lifecycle Rules
🗄️ Day 6: Built RDS PostgreSQL + MySQL database on AWS Cloud
🐬 Day 7: Mastered Aurora, MySQL & PostgreSQL with real SQL queries
🏗️ Day 8: Designed a 3-Tier VPC Architecture (Web+App+DB) from scratch
🔗 Day 9: Connected two VPCs with VPC Peering + accessed Private EC2 via Bastion
👀 Day 10: Set up CloudWatch CPU monitoring + ₹100 Budget Alert 🚨
🏆 CERTIFICATE: 10-Day AWS Cloud Cohort Completion — EARNED!

10 days ago I was just reading about AWS.
Today I have REAL projects on my resume. That's the difference. 🎯

Massive thanks to my mentor:
👨‍💻 @Vikas Ratnawat
(Ex-Amazon | Microsoft | Google | PwC | Microsoft MVP)
🔗 https://www.linkedin.com/in/vikasratnawat/

And the entire @CloudDevOpsHub Community 🙏
India's Largest Cloud & DevOps Community — 58,000+ Members!
🌐 https://www.linkedin.com/in/clouddevopshub/

To anyone on the fence — START. Build in public. Post daily.
Every post, every project, every day counts. 🚀

My next target: AWS Solutions Architect Certification 🏅

#10DayAWSChallenge #AWSChallenge #CloudDevOpsHub #CertificateEarned #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
#AWS #CloudComputing #DevOps #CloudEngineer #LearningInPublic  #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
#BuildInPublic #AWSCloud #CareerGrowth #HighPackage #VikasRatnawat #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
#Cohort6 #AWSSolutionsArchitect #CloudCertification #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
```

---

---

> 🌟 **Star this repo | Fork & Customize | Tag @Vikas Ratnawat + @CloudDevOpsHub**  
> [← Day 9](../Day-09/README.md) | [🏠 Back to Main](../README.md)
