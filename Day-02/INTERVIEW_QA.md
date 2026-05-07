# 🎯 Day 2 — Interview Questions & Answers: EC2 Deep Dive

> **CloudDevOpsHub Cohort-6 | Mentor: Vikas Ratnawat**  
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

## Q1. What is a Load Balancer? Why do we need it?

**Answer:**

A **Load Balancer** distributes incoming traffic across multiple EC2 instances to ensure no single server is overwhelmed.

**Without LB:** All traffic → 1 server → crashes under load  
**With LB:** Traffic → LB → distributed across 5 servers → high availability

**3 types in AWS:**
- **ALB (Application LB)** → HTTP/HTTPS, Layer 7, path/host-based routing
- **NLB (Network LB)** → TCP/UDP, Layer 4, ultra-low latency, millions of req/sec
- **CLB (Classic LB)** → Legacy, don't use in new setups

**Real Example:** Flipkart sale day — 10x normal traffic hits. LB spreads it across 50 servers. If 3 servers crash, LB stops sending traffic to them automatically via health checks.

---

## Q2. What is the difference between ALB and NLB?

**Answer:**

| Feature | ALB | NLB |
|---------|-----|-----|
| OSI Layer | Layer 7 (Application) | Layer 4 (Transport) |
| Protocol | HTTP, HTTPS, WebSocket | TCP, UDP, TLS |
| Routing | Path-based, Host-based | IP, Port-based |
| Static IP | ❌ (DNS only) | ✅ (Elastic IP support) |
| Performance | Good | Ultra-low latency |
| Use Case | Web apps, microservices | Gaming, IoT, real-time |
| Health Check | HTTP/HTTPS | TCP/HTTP |

**Interview Answer:**  
Use ALB for web applications where you need smart HTTP routing — e.g., /api goes to API servers, /images goes to S3.  
Use NLB for real-time applications needing microsecond latency like gaming servers or financial trading platforms.

---

## Q3. What is a Target Group in ALB?

**Answer:**

A **Target Group** is a logical group of targets (EC2 instances, Lambda functions, IPs) that the Load Balancer routes traffic to.

**Architecture:**
```
ALB Listener (Port 80)
    ↓
Listener Rule
    ↓
Target Group (e.g., "web-servers-tg")
    ↓
EC2-1 (healthy) ✅
EC2-2 (healthy) ✅  
EC2-3 (unhealthy) ❌ ← LB stops sending here
```

**Health Check:** LB pings each target every 30s (default). If unhealthy → removed from rotation automatically.

**Real Use:** Microservices routing:
- Rule: path `/api/*` → Target Group: API servers
- Rule: path `/auth/*` → Target Group: Auth servers
- Default: → Target Group: Frontend servers

---

## Q4. How does Nginx work? What is the difference between Nginx and Apache?

**Answer:**

**Nginx** is a high-performance web server and reverse proxy. It handles HTTP requests and serves static files or forwards to application servers.

**Common Uses:**
- Serve static websites
- Reverse proxy (forward to Node.js, Python app)
- SSL termination
- Load balancing (software-level)

| Feature | Nginx | Apache |
|---------|-------|--------|
| Architecture | Event-driven, async | Process/Thread per request |
| Performance | Better under high load | Good for low-moderate load |
| Config style | Simpler | More flexible (.htaccess) |
| Memory | Lower | Higher |
| Use Case | Static files, reverse proxy | Dynamic content, shared hosting |

**Real Fact:** Nginx powers Netflix, Airbnb, WordPress.com. At 10,000 concurrent connections, Nginx uses ~2.5MB RAM vs Apache's ~1GB.

---

## Q5. What is User Data in EC2? How is it used for automation?

**Answer:**

**User Data** is a script that runs automatically when an EC2 instance starts for the first time. It's used to bootstrap/configure the server without manual SSH.

```bash
#!/bin/bash
# This runs on first boot — fully automated server setup!
yum update -y
yum install -y nginx
systemctl start nginx
systemctl enable nginx
echo "<h1>Auto-configured by UserData!</h1>" > /usr/share/nginx/html/index.html
```

**Real Use Cases:**
- Install software automatically (Nginx, Docker, Java)
- Configure application settings
- Register instance with config management (Ansible, Chef)
- Mount EBS volumes, set environment variables

**Interview Tip:** "In auto-scaling groups, User Data is critical — when new instances launch automatically during traffic spikes, User Data ensures they're production-ready immediately without manual intervention." 

---

## Q6. What is the difference between Public IP, Private IP, and Elastic IP?

**Answer:**

| IP Type | Scope | Persistent? | Cost | Use Case |
|---------|-------|-------------|------|----------|
| **Public IP** | Internet-facing | Lost on stop/start | Free | Temporary dev/test access |
| **Private IP** | Within VPC only | Persistent ✅ | Free | Internal communication |
| **Elastic IP** | Internet-facing | Always same | Free if attached | Production DNS, firewall whitelisting |

**Architecture Reality:**
```
Internet → Elastic IP (stable) → ALB → EC2 Private IP (10.0.1.x)
```

In production, EC2s should NEVER have public IPs directly — they go behind a Load Balancer. Only the ALB needs a public endpoint.

---

## Q7. What is EC2 Auto Scaling? How does it work?

**Answer:**

**Auto Scaling** automatically adds or removes EC2 instances based on demand, ensuring you have the right number of servers at all times.

**Components:**
- **Launch Template** → What type of EC2 to launch (AMI, instance type, user data)
- **Auto Scaling Group (ASG)** → Min, max, desired capacity settings
- **Scaling Policy** → When to scale (CPU > 70% → add instance)

**Scaling Types:**
- **Target Tracking** → Maintain CPU at 50% (recommended)
- **Step Scaling** → Add 2 instances if CPU > 60%, add 5 if > 80%
- **Scheduled** → Scale up at 9AM IST every weekday

**Real World:**  
"During Diwali sale, Myntra's ASG scales from 20 to 200 EC2 instances in 3 minutes. After sale, scales back down automatically. Saves 80% cost vs keeping 200 instances all year." 

---

## Q8. What is EBS? What are the different EBS volume types?

**Answer:**

**EBS (Elastic Block Store)** is a persistent block storage for EC2 — like a hard disk for your virtual machine.

**Volume Types:**
| Type | IOPS | Use Case | Cost |
|------|------|----------|------|
| gp3 (General Purpose) | 3,000-16,000 | OS volumes, most apps | Low |
| gp2 (General Purpose) | 3 IOPS/GB | Legacy, being replaced by gp3 | Low |
| io1/io2 (Provisioned) | Up to 64,000 | Databases, SAP HANA | High |
| st1 (Throughput HDD) | N/A | Big data, log processing | Low |
| sc1 (Cold HDD) | N/A | Infrequent access archives | Lowest |

**Key Facts:**
- EBS is AZ-specific (can't attach Mumbai AZ-1a EBS to AZ-1b EC2)
- Can detach from one EC2 and attach to another
- Snapshots = backups stored in S3 (cross-region possible)
- gp3 is 20% cheaper than gp2 with better performance — always use gp3 for new volumes

---

## Q9. How do you connect to a Windows EC2 instance?

**Answer:**

**Windows EC2 uses RDP (Remote Desktop Protocol) on port 3389.**

**Steps:**
1. Launch Windows EC2 → Ensure port 3389 open in Security Group
2. EC2 Console → Select instance → Connect → RDP Client
3. Click "Get Password" → Upload your .pem key file → Decrypt password
4. Download RDP file → Open with Remote Desktop Connection
5. Username: `Administrator`, Password: (decrypted above)

**⚠️ Security Best Practices:**
- Never open RDP to 0.0.0.0/0 — restrict to your IP only
- Use a Bastion Host / Jump Server in production
- Consider AWS Systems Manager Session Manager (no open ports needed!)
- Change default Administrator password immediately after login

**Interview Tip:** "In enterprise setups, we use AWS SSM Session Manager instead of RDP — no need to open port 3389, full audit trail of all sessions." 

---

## Q10. What happens when an EC2 health check fails in a Load Balancer?

**Answer:**

When an EC2 instance fails a health check, the Load Balancer:

1. **Marks the instance as Unhealthy** after consecutive failures (default: 2 failures)
2. **Stops routing new traffic** to that instance
3. **Existing connections** are gracefully drained (default: 300s deregistration delay)
4. **Continues health checks** — if instance recovers, it's added back automatically

**Health Check Settings:**
```
Protocol: HTTP
Path: /health (your app's health endpoint)
Interval: 30 seconds
Healthy threshold: 2 consecutive successes
Unhealthy threshold: 2 consecutive failures
Timeout: 5 seconds
```

**With Auto Scaling:**
If ASG is attached, it also:
- Terminates the unhealthy EC2
- Launches a new replacement instance automatically
- Registers new instance with the load balancer

**Real Scenario:** "At 2AM, one of our EC2s ran out of memory. ALB detected health check failure, stopped sending traffic. ASG terminated it and launched a fresh one. Zero downtime. Zero alerts to the on-call team." 

---

## 📣 LinkedIn Post After Practicing These

```
🎯 Day 2 Interview Prep Done! #EC2DeepDive #CloudDevOpsHub

Practiced 10 real-world AWS interview questions today!

Most important thing I learned:
[Write your key takeaway from EC2 Deep Dive]

These aren't textbook answers — they're real scenarios.
Thanks to @Vikas Ratnawat | @CloudDevOpsHub for the depth! 🙏

#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHub
#CloudDevOpsHubCommunity #VikasRatnawat #AWS #InterviewPrep
```

---

> 🌟 **Fork → Customize → Push → Share on LinkedIn**  
> [← Back to Day 2 README](./README.md)