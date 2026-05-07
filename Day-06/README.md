# 🗄️ Day 6 — AWS RDS Deep Dive

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

Set up a **real managed database on AWS (RDS)** — PostgreSQL and MySQL. Connect to it from an EC2 instance. This is something you'll do on Day 1 of any cloud job. 🗄️

---

## 📚 Topics Covered

| # | Topic |
|---|-------|
| 🕰️ | History of Databases — File Systems → Cloud |
| 🛠️ | Installing Databases — On-Premise vs Cloud |
| ☁️ | Amazon RDS — Managed Relational Database Service |
| 🗄️ | Database Creation on AWS — Step-by-Step |
| ⚡ | Amazon Aurora vs RDS — Performance & Cost |

---

## 🛠️ Daily Task — Practical Challenge

### ✔ Practical 6: Create RDS PostgreSQL Database

```
Step 1: RDS → Create Database
Step 2: Engine → PostgreSQL (latest version)
Step 3: Template → Free tier
Step 4: DB Instance ID → "my-postgres-db"
Step 5: Master username → "admin"
Step 6: Master password → Set strong password (save it!)
Step 7: Instance class → db.t3.micro (Free Tier)
Step 8: Storage → 20 GB gp2
Step 9: VPC → Default VPC
Step 10: Public Access → Yes (for learning; No in production!)
Step 11: Security Group → Allow port 5432 from your IP
Step 12: Create Database
```

> ⏱️ RDS takes ~5 minutes to become available. Grab a chai! ☕

---

### ✔ Practical 7: Connect EC2 to RDS MySQL via Endpoint

**Architecture:**
```
Your Laptop / EC2
      │
      │ (MySQL Client)
      ↓
RDS MySQL Endpoint
(my-db.xxxx.ap-south-1.rds.amazonaws.com:3306)
      │
      ↓
Managed Database (Multi-AZ if needed)
```

**Step 1 — Create RDS MySQL:**
```
Engine: MySQL 8.x
Instance: db.t3.micro
DB Name: resumedb
Username: admin
Port: 3306
```

**Step 2 — Connect from EC2:**
```bash
# Install MySQL client on EC2
sudo yum install mysql -y

# Connect to RDS
mysql -h YOUR_RDS_ENDPOINT -u admin -p

# Enter password when prompted
```

**Step 3 — Create tables for Resume Project:**
```sql
CREATE DATABASE resumedb;
USE resumedb;

CREATE TABLE candidates (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    skills TEXT,
    experience_years INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO candidates (name, email, skills, experience_years)
VALUES ('Your Name', 'you@email.com', 'AWS, Linux, DevOps', 2);

SELECT * FROM candidates;
```

---

### 📋 Minor Project 1: Resume Database

Build a mini database project for your resume:

```sql
-- Create your portfolio database
CREATE DATABASE my_portfolio;
USE my_portfolio;

-- Projects table
CREATE TABLE projects (
    id INT AUTO_INCREMENT PRIMARY KEY,
    project_name VARCHAR(200),
    tech_used VARCHAR(200),
    description TEXT,
    completion_date DATE
);

-- Insert your AWS Challenge projects
INSERT INTO projects VALUES 
(1, 'EC2 Web Server', 'AWS EC2, Nginx, Linux', 'Deployed website with ALB', '2026-04-30'),
(2, 'S3 Resume Site', 'AWS S3, HTML/CSS', 'Static portfolio on S3', '2026-05-04'),
(3, 'RDS Resume DB', 'AWS RDS, MySQL', 'Resume data in managed DB', '2026-05-06');

SELECT project_name, tech_used FROM projects;
```

> 💡 **Add to resume:** "Designed and deployed MySQL database on AWS RDS for portfolio application"

---

## 🏆 Today's Challenge

```
🔥 CHALLENGE:
1. Create RDS (MySQL or PostgreSQL) on AWS
2. Connect to it from EC2 using CLI
3. Create a table and insert YOUR data
4. Run SELECT query and screenshot the results
5. This is now your "Minor Project 1" for your resume!
```

---

## 📸 Screenshot Checklist

- [ ] RDS instance in "Available" state
- [ ] EC2 connected to RDS via MySQL/PSQL CLI
- [ ] CREATE TABLE command executed
- [ ] SELECT query showing your data

---


## ⚡ RDS vs Aurora — Quick Comparison

| Feature | RDS MySQL | Amazon Aurora |
|---------|-----------|---------------|
| Performance | Baseline | 5x faster |
| Cost | Lower | Higher |
| Availability | 99.95% | 99.99% |
| Use Case | Standard apps | High-traffic apps |
| Storage | Fixed | Auto-scales |

---

## 🔗 Resources

- [Amazon RDS Documentation](https://docs.aws.amazon.com/rds/)
- [RDS Free Tier Details](https://aws.amazon.com/rds/free/)
> [← Day 5](../Day-05/README.md) | [Back to Main](../README.md) | [Day 7 →](../Day-07/README.md)


---

## 📣 LinkedIn Post — Copy & Customize

```
🗄️ Day 6 of My 10-Day AWS Challenge! #CloudDevOpsHub

Today I set up a REAL production-grade database on AWS! 💪

✅ Created Amazon RDS (PostgreSQL + MySQL)
✅ Connected EC2 instance to RDS via endpoint
✅ Built a Resume Database as Minor Project 1
✅ Learned RDS vs Aurora performance differences

Real World Impact 💡:
→ Self-managed DB on EC2: You handle backups, patches, HA setup
→ AWS RDS: Automated backups, patching, Multi-AZ, monitoring
→ RDS saves 40-60% of your DBA team's time!

The database I built today stores my project portfolio.
It's now a resume project: "Designed MySQL DB on AWS RDS" 🎯

Aurora vs RDS quick take:
• RDS MySQL = Great for most apps
• Aurora = 5x faster, used by Netflix, Airbnb for high-traffic

Day 6 of 10 done! Getting closer to cloud-ready 🚀

#Day6 #AWSChallenge #10DayAWSChallenge #CloudDevOpsHub #AWS #RDS 
#MySQL #PostgreSQL #Database #CloudComputing #DevOps #LearningInPublic #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
```

> 💬 **LinkedIn Profile Tip:**  
> Add "Minor Project 1: Resume Portfolio Database on AWS RDS" to your **Experience or Projects** section.  
> Even a small project with a real AWS service = massive resume value!

---


---

> 🌟 **Star this repo | Fork & Customize | Tag @Vikas Ratnawat + @CloudDevOpsHub**  
> [← Day 5](../Day-05/README.md) | [🏠 Back to Main](../README.md) | [Day 7 →](../Day-07/README.md)
