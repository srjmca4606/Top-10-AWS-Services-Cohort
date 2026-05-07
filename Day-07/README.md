# 🐬 Day 7 — RDS Practical (Aurora, MySQL & PostgreSQL)

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

Full hands-on day! Work with **Amazon Aurora, MySQL, and PostgreSQL** — understand each engine's strengths, connect real applications, and solidify your database skills. This is pure practice day. 💪

---

## 📚 Topics Covered

| # | Topic |
|---|-------|
| 🗄️ | Database Creation on AWS — Hands-On Practice |
| ⚡ | Amazon Aurora — High Performance & High Availability |
| 🐬 | MySQL — Setup, Configuration & Use Cases |
| 🐘 | PostgreSQL — Advanced Open-Source Relational DB |

---

## 🛠️ Daily Task — Practical Challenge

### ✔ Practical: Create Amazon Aurora Cluster

```
Step 1: RDS → Create Database → Aurora (MySQL Compatible)
Step 2: Template → Dev/Test (cheaper)
Step 3: Cluster ID → "my-aurora-cluster"
Step 4: Master user → "auroraadmin"
Step 5: Instance → db.t3.medium
Step 6: Create

Aurora gives you:
→ Writer endpoint (for writes)
→ Reader endpoint (for reads — auto load balanced!)
```

> 💡 **Aurora Serverless v2** — Scales from 0.5 to 128 ACUs automatically. Perfect for variable workloads like e-commerce.

---

### ✔ Deep Dive: MySQL vs PostgreSQL — When to Use What

**MySQL:**
```
Best for:
✅ Web applications (WordPress, Drupal)
✅ E-commerce (Shopify uses MySQL)
✅ Read-heavy applications
✅ Simple CRUD operations

Port: 3306
AWS RDS Engine: mysql8.0

Connect:
mysql -h endpoint -u admin -p -e "SHOW DATABASES;"
```

**PostgreSQL:**
```
Best for:
✅ Complex queries with JOINs
✅ JSON/JSONB data (hybrid SQL+NoSQL)
✅ Financial systems
✅ Analytics workloads
✅ GIS/Location data

Port: 5432
AWS RDS Engine: postgres16

Connect:
psql -h endpoint -U admin -d postgres
```

---

### ✔ Hands-On Practice: Full Stack Database Operations

**Create a mini blog database (MySQL):**
```sql
CREATE DATABASE techblog;
USE techblog;

-- Users table
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(50) UNIQUE,
  email VARCHAR(100),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Posts table  
CREATE TABLE posts (
  id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(200),
  content TEXT,
  author_id INT,
  published BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (author_id) REFERENCES users(id)
);

-- Insert sample data
INSERT INTO users (username, email) VALUES 
('vikas_cdh', 'vikas@clouddevopshub.com'),
('aws_learner', 'learner@example.com');

INSERT INTO posts (title, content, author_id, published) VALUES
('My AWS Journey Day 7', 'Today I completed RDS practical...', 1, TRUE),
('EC2 Setup Guide', 'Step by step EC2 setup...', 1, TRUE);

-- Query with JOIN
SELECT 
  p.title,
  u.username,
  p.created_at
FROM posts p
JOIN users u ON p.author_id = u.id
WHERE p.published = TRUE;
```

**PostgreSQL version (same scenario, different syntax):**
```sql
-- PostgreSQL uses SERIAL instead of AUTO_INCREMENT
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50) UNIQUE,
  email VARCHAR(100),
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- PostgreSQL supports JSON natively!
CREATE TABLE app_config (
  key VARCHAR(100) PRIMARY KEY,
  value JSONB
);

INSERT INTO app_config VALUES 
('aws_settings', '{"region": "ap-south-1", "services": ["EC2", "S3", "RDS"]}');

-- Query JSON field!
SELECT value->>'region' AS aws_region FROM app_config WHERE key = 'aws_settings';
```

---

## 🏆 Today's Challenge

```
🔥 CHALLENGE:
Pick ONE of these and complete it:

Option A (Easy): 
→ Connect to your existing RDS MySQL
→ Create 2 tables with a relationship (FOREIGN KEY)
→ Run a JOIN query
→ Screenshot the results

Option B (Medium):
→ Create Aurora cluster
→ Connect to Writer endpoint
→ Create a database
→ Run 5 different SQL queries (SELECT, INSERT, UPDATE, DELETE, JOIN)

Option C (Hard — Resume worthy!):
→ Create both MySQL AND PostgreSQL RDS
→ Load same dataset into both
→ Compare query performance
→ Write a 3-line summary: "MySQL vs PostgreSQL — which was faster for my use case?"
```

---

## 📸 Screenshot Checklist

- [ ] Aurora cluster endpoints (writer + reader)
- [ ] MySQL connection from EC2
- [ ] JOIN query results
- [ ] PostgreSQL connection (bonus)

---


## 🗄️ Database Engine Cheat Sheet

| Feature | MySQL | PostgreSQL | Aurora |
|---------|-------|-----------|--------|
| Port | 3306 | 5432 | 3306/5432 |
| JSON | Limited | Native JSONB | Limited |
| Full-text Search | Yes | Better | Yes |
| Performance | Fast | Complex queries | 5x faster |
| Cost | Low | Low | Higher |
| Best for | Web apps | Analytics/GIS | High traffic |

---

## 🔗 Resources

- [Aurora Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/)
- [PostgreSQL vs MySQL Comparison](https://aws.amazon.com/compare/the-difference-between-mysql-vs-postgresql/)
> [← Day 6](../Day-06/README.md) | [Back to Main](../README.md) | [Day 8 →](../Day-08/README.md)


---

## 📣 LinkedIn Post — Copy & Customize

```
🐬 Day 7 of My 10-Day AWS Challenge! #CloudDevOpsHub

Full hands-on database day! Worked with 3 different AWS database engines 💪

✅ Explored Amazon Aurora (MySQL & PostgreSQL compatible)
✅ Built a full relational database with JOINs and Foreign Keys
✅ Compared MySQL vs PostgreSQL for different use cases
✅ Ran advanced SQL queries on AWS RDS

My finding today 🤔:

MySQL = Great for web apps, faster for simple reads
PostgreSQL = Better for complex analytics, native JSON support

Coolest thing I learned:
→ Aurora has BOTH a Writer and Reader endpoint
→ All READ queries automatically load-balance across replicas
→ That's how Netflix handles millions of DB reads per second!

Building real database skills, not just clicking buttons 🎯

#Day7 #AWSChallenge #10DayAWSChallenge #CloudDevOpsHub #AWS #RDS 
#Aurora #MySQL #PostgreSQL #SQL #Database #LearningInPublic #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
```

> 💬 **LinkedIn Profile Tip:**  
> When you write posts with technical comparisons (MySQL vs PostgreSQL),  
> you attract SENIOR engineers to comment — building your professional network passively!

---


---

> 🌟 **Star this repo | Fork & Customize | Tag @Vikas Ratnawat + @CloudDevOpsHub**  
> [← Day 6](../Day-06/README.md) | [🏠 Back to Main](../README.md) | [Day 8 →](../Day-08/README.md)
