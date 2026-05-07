# ☁️ Day 1 — AWS Introduction & EC2 Basics

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

Get your AWS account live and launch your **first EC2 virtual machine** in the cloud.  
By end of today — you should have a running server on AWS. That's the mission. 💪

---

## 📚 Topics Covered

| # | Topic |
|---|-------|
| ✅ | History of AWS — How it all started in 2006 |
| ✅ | What is AWS — The world's #1 Cloud Platform |
| ✅ | Traditional IT vs Cloud Services — Why Cloud wins |
| ✅ | AWS Global Infrastructure — Regions, AZs, Edge Locations |
| ✅ | AWS Account Creation — Free Tier walkthrough |
| ✅ | First EC2 Instance Creation — Live Practical |

---

## 🛠️ Daily Task — Practical Challenge

### ✔ Task 1: Create Your AWS Free Tier Account

```
1. Go to → https://aws.amazon.com/free
2. Sign up with your email
3. Add credit/debit card (₹2 verification, not charged)
4. Select "Basic Support – Free"
5. Login to AWS Console → https://console.aws.amazon.com
```

> 💡 **Pro Tip:** Use a dedicated email for AWS. Never share root credentials.

---

### ✔ Task 2: Launch Your First EC2 Instance (Linux VM)

```
Step 1: Go to EC2 → Launch Instance
Step 2: Name it → "my-first-server"
Step 3: AMI → Amazon Linux 2023 (Free Tier Eligible)
Step 4: Instance Type → t2.micro (Free Tier)
Step 5: Key Pair → Create new → Download .pem file (SAVE IT!)
Step 6: Security Group → Allow SSH (port 22) from My IP
Step 7: Launch Instance
Step 8: Connect via EC2 Instance Connect (browser SSH)
```

**Verify it works:**
```bash
whoami
# Output: ec2-user

uname -r
# Shows Linux kernel version
```

---

## 🏆 Today's Challenge

```
🔥 CHALLENGE: 
Launch an EC2 instance and run the following command:
  echo "Hello AWS! I am [Your Name] - Day 1 Complete 🚀"

Screenshot it and post on LinkedIn!
```

---

## 📸 Screenshot Checklist

- [ ] AWS Console Dashboard (logged in)
- [ ] EC2 Instance in "Running" state
- [ ] Terminal showing `echo` output
- [ ] EC2 Instance details page

---


## 🔗 Resources

- [AWS Free Tier](https://aws.amazon.com/free)
- [AWS Global Infrastructure Map](https://aws.amazon.com/about-aws/global-infrastructure/)
- [EC2 Documentation](https://docs.aws.amazon.com/ec2/)

---

## ⏭️ Tomorrow — Day 2

**EC2 Deep Dive** → Windows Server, Nginx Web Server, Load Balancer  
Make sure your EC2 is running — we'll use it tomorrow!
> [← Back to Main](../README.md) | [Day 2 →](../Day-02/README.md)


---

## 📣 LinkedIn Post — Copy & Customize

```
🚀 Day 1 of My 10-Day AWS Challenge! #CloudDevOpsHub

I just completed Day 1 of the AWS Cohort Challenge! 🎉

What I did today:
✅ Created my AWS Free Tier Account
✅ Understood Traditional IT vs Cloud
✅ Explored AWS Global Infrastructure (Regions & Availability Zones)
✅ Launched my First EC2 Instance on AWS!

Key Learning 💡:
Traditional IT = You buy servers, manage them, pay even when idle.
Cloud = Pay only for what you use. Scale in seconds. No hardware headache!

This is just Day 1 — and I already have a live server running in the cloud! 🔥

Attending the 10-Day AWS Cohort by:
👨‍💻 @Vikas Ratnawat (Ex-Amazon | Microsoft | Google | Microsoft MVP)
🌐 @CloudDevOpsHub Community — India's Largest Cloud & DevOps Community (58K+ Members)
🔗 https://linkedin.com/in/vikasratnawat/

#Day1 #AWSChallenge #10DayAWSChallenge #CloudDevOpsHub #AWS #CloudComputing 
#DevOps #LearningInPublic #AWSCloud #CloudEngineer #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
```

> 💬 **Tip to Optimize LinkedIn Profile:**  
> Add "AWS Cloud | Learning in Public" to your headline today.  
> Recruiters search for these keywords. Every post you make = free visibility.

---


---

> 🌟 **Star this repo | Fork & Customize | Tag @Vikas Ratnawat + @CloudDevOpsHub**  
> [← Day 0](../Day-00/README.md) | [🏠 Back to Main](../README.md) | [Day 2 →](../Day-02/README.md)
