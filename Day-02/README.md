# 🖥️ Day 2 — AWS EC2 Deep Dive

> **Cohort:** AWS Cloud Cohort by CloudDevOpsHub

---

## 🎯 Today's Goal

Go deeper into EC2 — launch a **Windows Server**, set up **Nginx web server**, deploy an HTML website, and configure an **Application Load Balancer (ALB)**. By end of today — you'll have a live website hosted on AWS! 🌐

---

## 📚 Topics Covered

| # | Topic |
|---|-------|
| 🖥️ | EC2 Instance Creation — Detailed Explanation & Use Cases |
| 🪟 | EC2 Connection via Remote Desktop & Windows Server 2025 Setup |
| 🔐 | EC2 Access using MobaXterm — Configuration & Connection |
| 🌐 | Nginx Web Server Setup & HTML Website Deployment (Live Project) |
| ⚖️ | Application Load Balancer (ALB) — Traffic Distribution & High Availability |

---

## 🛠️ Daily Task — Practical Challenge

### ✔ Practical 1: Launch Windows Server 2025 EC2 Instance

```
Step 1: EC2 → Launch Instance
Step 2: AMI → Windows Server 2025 Base
Step 3: Instance Type → t2.micro or t3.micro
Step 4: Key Pair → Use existing or create new
Step 5: Security Group → Allow RDP (port 3389) from My IP
Step 6: Launch → Get Password (using .pem key)
Step 7: Connect via RDP (Remote Desktop Protocol)
```

> 💡 **Pro Tip:** In interviews, always mention RDP = port 3389 (Windows), SSH = port 22 (Linux)

---

### ✔ Practical 2: Nginx Web Server on Linux EC2 via MobaXterm

**Step 1 — Connect using MobaXterm:**
```
1. Download MobaXterm → https://mobaxterm.mobatek.net
2. New Session → SSH
3. Remote Host = EC2 Public IP
4. Use private key (.pem file)
5. Connect!
```

**Step 2 — Install Nginx:**
```bash
sudo yum update -y
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

**Step 3 — Deploy HTML Website:**
```bash
cd /usr/share/nginx/html
sudo nano index.html
```

```html
<!DOCTYPE html>
<html>
<head><title>My AWS Website - Day 2</title></head>
<body>
  <h1>🚀 Hello from AWS EC2!</h1>
  <p>Deployed by [Your Name] — Day 2 of AWS Challenge</p>
  <p>Powered by Nginx on Amazon Linux | CloudDevOpsHub</p>
</body>
</html>
```

```bash
# Open port 80 in Security Group first, then:
curl http://localhost
# You should see your HTML!
```

Access via browser: `http://YOUR_EC2_PUBLIC_IP`

---

### ✔ Bonus: Application Load Balancer (ALB) Setup

```
Step 1: Launch 2 EC2 instances (same Nginx setup)
Step 2: EC2 → Load Balancers → Create Load Balancer
Step 3: Type → Application Load Balancer
Step 4: Scheme → Internet-facing
Step 5: Create Target Group → Add both EC2s
Step 6: Create ALB → Copy DNS name
Step 7: Access via ALB DNS → Traffic splits between 2 servers!
```

> 💡 **Real World Use:** Every big company (Amazon, Flipkart, Zomato) uses ALB to handle millions of requests. One server dies → ALB routes to healthy servers automatically!

---

## 🏆 Today's Challenge

```
🔥 CHALLENGE:
1. Deploy an HTML page on Nginx showing your name + "Day 2 Complete"
2. Access it via Public IP
3. Screenshot your website live on AWS
4. Post on LinkedIn!
```

---

## 📸 Screenshot Checklist

- [ ] Windows Server 2025 running via RDP
- [ ] MobaXterm connected to Linux EC2
- [ ] Nginx status showing "active (running)"
- [ ] Website live in browser with your EC2 Public IP

---


## 🔗 Resources

- [MobaXterm Download](https://mobaxterm.mobatek.net/download.html)
- [Nginx Official Docs](https://nginx.org/en/docs/)
- [AWS ALB Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)

---

## ⏭️ Tomorrow — Day 3

**AWS IAM** → Creating users, roles, policies, and Load Balancer configuration  
Security is everything in Cloud — IAM is the foundation!
> [← Day 1](../Day-01/README.md) | [Back to Main](../README.md) | [Day 3 →](../Day-03/README.md)


---

## 📣 LinkedIn Post — Copy & Customize

```
🚀 Day 2 of My 10-Day AWS Challenge! #CloudDevOpsHub

Today was FIRE! 🔥 Here's what I built on AWS:

✅ Launched Windows Server 2025 on EC2 (connected via RDP)
✅ Set up Nginx Web Server on Linux EC2 using MobaXterm
✅ Deployed a LIVE HTML website on AWS! 🌐
✅ Configured Application Load Balancer for High Availability

My website is now live at an AWS IP address — deployed in under 30 minutes! ⚡

Key Learning 💡:
→ SSH (port 22) = Linux access
→ RDP (port 3389) = Windows access  
→ ALB distributes traffic across multiple servers
→ If one server fails, ALB routes to healthy ones — Zero Downtime!

Day 2 done. Building in public with @Vikas Ratnawat | @CloudDevOpsHub Community 💪

#Day2 #AWSChallenge #10DayAWSChallenge #CloudDevOpsHub #AWS #EC2 #Nginx 
#LoadBalancer #CloudComputing #DevOps #LearningInPublic #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
```

> 💬 **LinkedIn Profile Tip:**  
> Add your Nginx/EC2 project to your **Featured Section** on LinkedIn.  
> Even a screenshot counts as a portfolio item for fresher profiles!

---


---

> 🌟 **Star this repo | Fork & Customize | Tag @Vikas Ratnawat + @CloudDevOpsHub**  
> [← Day 1](../Day-01/README.md) | [🏠 Back to Main](../README.md) | [Day 3 →](../Day-03/README.md)
