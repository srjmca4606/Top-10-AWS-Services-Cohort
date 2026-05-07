# 🔥 Day 1 — Real-World Scenario Q&A: AWS Introduction & EC2 Basics

> **CloudDevOpsHub Cohort-6 | Mentor: Vikas Ratnawat**  
> 🔗 [linkedin.com/in/vikasratnawat](https://www.linkedin.com/in/vikasratnawat/) | [clouddevopshub](https://www.linkedin.com/in/clouddevopshub/)

---

## 🎬 These Scenarios Were Discussed in the Day's Recording

```
📺 Watch the session first: https://devopswithvikas.com/eud/bookings
📌 Then practice each scenario answer
🎤 Speak the answer out loud — don't just read it
💼 In interview: Use these as story-based answers
```

---

## Scenario 1: Your startup's website is getting 10x more traffic during a product launch. How do you handle this using AWS EC2?

**Your Answer (How to Approach This):**

**Solution: Auto Scaling + Load Balancer**

**Current Architecture (Problem):** Single EC2 → crashes during spike

**Target Architecture:**
```
Traffic → ALB → Auto Scaling Group (EC2 instances)
                ├── EC2-1 (always running)
                ├── EC2-2 (launched when CPU > 60%)
                ├── EC2-3 (launched when CPU > 70%)
                └── EC2-N (up to 20 — scales back down after)
```

**Steps:**
1. Create Launch Template (AMI + instance type + user data)
2. Create Auto Scaling Group with min=2, desired=2, max=20
3. Set scaling policy: CPU > 60% → add 2 instances
4. Put ASG behind Application Load Balancer
5. Set scale-in protection during launch hour

**Cost Strategy:**
- 2 Reserved Instances always running (base load)
- Additional On-Demand during spike
- Set max=20 to prevent surprise bills

**Result:** Application handles 10x traffic automatically. After launch, scales back to 2 instances. You only paid for what was used.

---

## Scenario 2: You deployed an EC2 instance but can't SSH into it. Walk me through your troubleshooting steps.

**Your Answer (How to Approach This):**

**Systematic Troubleshooting (Real Interview Answer):**

```
Step 1: Check instance state
  EC2 → Running? If stopped → start it
  System Status Check: Green? Instance Status Check: Green?
  If either fails → likely OS issue → reboot or stop/start

Step 2: Check Security Group
  EC2 → Security → Inbound Rules
  Is port 22 open?
  Source: Is it 0.0.0.0/0 or your specific IP?
  If using My IP → has your IP changed? (dynamic ISP IPs change!)

Step 3: Verify you have the right key pair
  The .pem file must match the key pair selected at launch
  If wrong key → Can't connect ever (only fix: restore from AMI)

Step 4: Check EC2 Username
  Amazon Linux 2: ec2-user
  Ubuntu: ubuntu
  Red Hat: ec2-user
  Windows: Administrator

Step 5: Check .pem permissions
  chmod 400 my-key.pem
  If permissions are wrong → "WARNING: UNPROTECTED PRIVATE KEY"

Step 6: Verify Public IP
  Does the EC2 have a public IP? (subnet must have auto-assign enabled)
  If using EIP → is it associated?

Step 7: Check NACL
  VPC → Subnets → NACL
  Is port 22 allowed inbound AND outbound (1024-65535)?

Step 8: Try EC2 Instance Connect
  EC2 Console → Connect → EC2 Instance Connect (browser)
  If this works but SSH doesn't → issue is local (key, IP, client)
```

**Most Common Root Cause:** Security Group not allowing port 22 from your current IP.

---

## Scenario 3: Your company wants to move from an on-premises data center to AWS. What is your migration approach?

**Your Answer (How to Approach This):**

**6 R's of Migration (AWS Framework):**

```
Rehost (Lift & Shift):
  Move VMs as-is to EC2
  Fastest, minimal change
  Use: AWS Application Migration Service (MGN)
  Timeline: Weeks

Replatform (Lift & Tinker):
  Move to managed services
  Oracle DB on EC2 → AWS RDS
  Self-hosted Redis → ElastiCache
  Keep app code same
  Timeline: Months

Refactor/Re-architect:
  Redesign for cloud-native
  Monolith → Microservices on ECS/EKS
  Highest benefit, longest timeline
  Timeline: Months to years

Repurchase:
  Move to SaaS
  Self-hosted CRM → Salesforce
  Self-hosted email → Office 365

Retire:
  Decommission apps no longer needed
  Typically 10-20% of portfolio!

Retain:
  Keep on-premises for now
  Compliance, latency, or not yet ready
```

**Phased Approach:**
```
Month 1-3: Assessment + Planning
  → AWS Application Discovery Service → inventory all servers
  → Identify dependencies
  → Estimate costs

Month 4-6: Pilot Migration
  → Migrate 2-3 non-critical apps first
  → Validate tooling and processes

Month 7-18: Wave-based Migration
  → 10-20 apps per wave
  → Test → validate → cutover
  → Retire old hardware wave by wave
```

---

## Scenario 4: You are running an e-commerce website on a single EC2 t2.micro instance. Sales are doubling every month. What is your scaling strategy?

**Your Answer (How to Approach This):**

**Progressive Scaling Strategy:**

**Phase 1: Immediate (Current month) — Vertical Scaling**
```
t2.micro → t3.medium (2 vCPU, 4GB RAM) — Stop instance, change type, restart
Cost: ~₹3,000/month → ₹8,000/month
Handles: ~2-3x traffic increase
```

**Phase 2: Next month — Add Read Replica**
```
RDS Single → RDS + 1 Read Replica
Direct all product catalog reads to replica
Reduces primary DB load by 60%
```

**Phase 3: Month 3 — Add Caching Layer**
```
Add ElastiCache (Redis) in front of DB
Cache product pages, session data, cart
85% of reads → served from cache (microseconds)
DB load drops further
```

**Phase 4: Month 4 — Horizontal Scaling**
```
Single EC2 → Auto Scaling Group (min 2, max 20)
Add Application Load Balancer
Deploy in 2 Availability Zones
Add CloudFront CDN for static assets
```

**Phase 5: 6 months+ — Microservices**
```
Monolith → Separate services:
  Product Service → ECS containers
  Payment Service → ECS containers  
  Cart Service → DynamoDB (serverless, auto-scales)
  Search → Elasticsearch
```

**Interview Insight:** "We solved this exact problem at [company]. Each phase handled 2-3x traffic increase. Full migration from single EC2 to distributed system took 6 months with zero customer-facing downtime." 

---

## Scenario 5: You accidentally exposed your AWS Access Key on GitHub. What do you do in the next 5 minutes?

**Your Answer (How to Approach This):**

**Incident Response — Exposed AWS Credentials (5-Minute Response):**

```
MINUTE 1: Invalidate immediately
  IAM → Users → Select user → Security Credentials
  Deactivate the compromised access key (status → Inactive)
  Do NOT delete yet — keep for investigation

MINUTE 2: Check CloudTrail NOW
  CloudTrail → Event History
  Filter: Last 1 hour, filter by the access key
  Look for: CreateUser, AttachUserPolicy, CreateRole, RunInstances
  → Has the key been used? By whom? What did they do?

MINUTE 3: Revoke all sessions
  IAM → Policies → Create and attach inline deny policy to the user
  {"Effect": "Deny", "Action": "*", "Resource": "*"}
  This blocks all active sessions even if key was cached

MINUTE 4: Assess damage
  Check for: New IAM users/roles, EC2 instances in ALL regions
  Check billing: Was anything expensive launched?
  Check S3: Any data exfiltrated?
  Check us-east-1 — attackers often use this region first!

MINUTE 5: Create new key + rotate
  Create new access key
  Update all applications using old key
  Delete old compromised key entirely
```

**After immediate response:**
```
GitHub: Delete the repo commit history (git filter-branch or BFG Repo Cleaner)
        OR mark repo private immediately
Review: How did key end up in code? 
Prevention: git-secrets, AWS Secrets Manager, environment variables only
Audit: Full CloudTrail review of past 7 days
Report: Security incident report to team/management
```

**Prevention (setup today):**
- Add git-secrets pre-commit hook
- Use IAM Roles instead of access keys
- Enable GuardDuty — detects key usage from unexpected IPs
- AWS secret scanning on GitHub via AWS partnered alerting

---

## Scenario 6: A junior developer says: 'I'll put the database on the same EC2 as the web server to save money.' How do you respond?

**Your Answer (How to Approach This):**

**This is an anti-pattern. Here's how to explain and convince:**

**Problems with DB + Web on same EC2:**

```
Problem 1: Resource competition
  Web server gets traffic spike → uses 80% CPU → DB gets 20%
  Slow queries → Web requests timeout → Users leave
  
Problem 2: Security nightmare
  Web server (public-facing) can be compromised
  Attacker on web server → directly accesses DB (same machine!)
  Defense-in-depth requires isolation

Problem 3: No independent scaling
  DB needs more RAM → can't scale without affecting web server
  Web needs more CPU → can't scale without restarting DB
  
Problem 4: No HA/failover
  Single server fails → BOTH web AND DB down simultaneously
  Recovery: restore whole server → longer RTO
```

**Better Architecture (still cheap for startup):**
```
Web Server: EC2 t3.micro (~$8/month)
Database: RDS db.t3.micro (~$15/month)
           Free tier: 750 hrs/month (first year FREE!)

Additional benefits:
- RDS: Automated backups, monitoring, patching handled by AWS
- Security: DB in private subnet, web server in public
- Scaling: Scale web and DB independently
```

**Cost Reality:**
"RDS Free Tier covers 750 hours/month — db.t3.micro is FREE for first year. Even after free tier, $15/month for a managed DB is worth it vs debugging production issues on a combined server." 

**This conversation shows you understand AWS architecture principles — not just cost optimization.

---

## Scenario 7: Explain how you would set up a Dev/Test/Prod environment in AWS.

**Your Answer (How to Approach This):**

**Environment Strategy (Enterprise Best Practice):**

**Option A: Separate AWS Accounts (Recommended)**
```
AWS Organization
├── Management Account (billing, SSO)
├── Development Account
│   └── VPC: 10.0.0.0/16
│   └── EC2: t3.micro (cheap, stop at night)
│   └── RDS: db.t3.micro
├── Staging Account
│   └── VPC: 10.1.0.0/16  
│   └── EC2: same size as prod (accurate testing!)
│   └── RDS: same size as prod
└── Production Account
    └── VPC: 10.2.0.0/16
    └── EC2: t3.large (production sizing)
    └── RDS: db.r5.large, Multi-AZ
```

**Why separate accounts?**
- SCPs: Dev can't accidentally change Prod
- Blast radius: Dev incident can't impact Prod
- Billing: Clear cost visibility per environment
- IAM: Dev credentials can't reach Prod resources

**Option B: Same Account, Separate VPCs (Smaller teams)**
```
VPC: Dev (10.0.0.0/16) → VPC: Staging (10.1.0.0/16) → VPC: Prod (10.2.0.0/16)
VPC Peering between Staging and Prod only (for data sync)
Strict IAM policies: Devs can't touch Prod
```

**Cost Optimization for Dev/Test:**
```
Dev EC2: Stop at 8PM, start at 8AM (Lambda + EventBridge schedule)
Dev RDS: db.t3.micro (smallest possible)
Dev: No Multi-AZ (saves 50%)
Dev: No Read Replicas
Savings: ~60% vs running dev at same scale as prod
```

---

## Scenario 8: Your EC2 instance is running out of disk space. How do you fix this without downtime?

**Your Answer (How to Approach This):**

**Fix Without Downtime (Online EBS Volume Extension):**

```
Step 1: Identify the full disk
SSH into EC2:
df -h  
# Shows: /dev/xvda1  8.0G  7.8G  200M  98% /  ← FULL!

Step 2: Modify EBS volume (no downtime needed!)
EC2 → Volumes → Select volume → Actions → Modify Volume
Size: 8 GB → 20 GB → Modify

Wait 2-3 minutes for modification to complete.
Status: "in-use-optimizing" → "in-use"

Step 3: Extend the file system (ONLINE, no reboot!)
# Check partition
lsblk
# Output: xvda → xvda1 (partition)

# Grow partition (for xvda1):
sudo growpart /dev/xvda 1

# Extend file system:
# For ext4:
sudo resize2fs /dev/xvda1

# For xfs (Amazon Linux 2):
sudo xfs_growfs /

Step 4: Verify
df -h
# Shows: /dev/xvda1  20G  7.8G  12G  39% /  ← Fixed!
```

**Prevention:**
```
CloudWatch alarm: DiskSpaceUtilization > 80% → SNS → email
→  Never be surprised by full disk again

Lambda automation:
  Alarm triggers → Lambda → extend EBS + grow filesystem automatically
```

**For massive data growth:** Consider moving data to S3 (infinite, cheap) and keeping only working data on EBS.

---

## Scenario 9: How do you ensure your EC2 application automatically recovers from a crash?

**Your Answer (How to Approach This):**

**Multi-Layer Recovery Strategy:**

**Layer 1: EC2 Auto Recovery (hardware failures)**
```
CloudWatch Alarm:
  Metric: StatusCheckFailed_System
  Period: 1 minute, Evaluation: 2 periods
  Action: EC2 Action → Recover
  
What happens: AWS migrates instance to healthy host
  Same instance ID, same EIP, same private IP
  Downtime: ~10-15 minutes
```

**Layer 2: Application Auto-Restart (software crashes)**
```
Use systemd to auto-restart your app:

/etc/systemd/system/myapp.service:
[Unit]
Description=My Application
After=network.target

[Service]
ExecStart=/usr/bin/node /app/server.js
Restart=always
RestartSec=5
User=ec2-user

[Install]
WantedBy=multi-user.target

# Enable:
sudo systemctl enable myapp
sudo systemctl start myapp

# Now app restarts automatically after any crash!
```

**Layer 3: Auto Scaling (instance-level failures)**
```
Auto Scaling Group: min=2, desired=2, max=10
  If EC2 health check fails → ASG terminates → launches replacement
  New instance pulls latest app from S3/CodeDeploy automatically
  User: No impact (ALB routes to healthy instances)
```

**Layer 4: Multi-AZ (AZ-level failures)**
```
Deploy across 2 AZs in ASG
  AZ-1a fails → all traffic routes to AZ-1b instances
  ASG launches replacement instances in AZ-1b
```

---

## Scenario 10: You need to run a batch job daily at 2AM that processes 100GB of data. What AWS setup do you recommend?

**Your Answer (How to Approach This):**

**Cost-Optimized Batch Processing Architecture:**

**Option 1: Spot Instance + EventBridge (Recommended)**
```
2AM: EventBridge Scheduler triggers Lambda
Lambda: Launches Spot EC2 instance with:
  - Instance type: c5.2xlarge (8 vCPU, 16GB RAM)
  - AMI: Custom AMI with processing software
  - User Data: Script to run job + terminate when done
  - Spot: ~70% cheaper than On-Demand

Spot Instance:
  Pulls 100GB data from S3
  Processes data
  Writes results to S3/RDS
  Self-terminates: aws ec2 terminate-instances --instance-ids $INSTANCE_ID

3:30AM: Processing done, instance terminates
Cost: ~$0.50 per night vs $1.70 On-Demand = saves ₹900/month
```

**Option 2: AWS Batch (Fully Managed)**
```
AWS Batch manages:
  - EC2 Spot fleet provisioning
  - Job queuing and retry
  - Scaling compute resources

Job definition: Docker container with your processing code
Job queue → Compute environment (Spot instances)
EventBridge → triggers Batch job at 2AM

Benefits: No infra management, automatic retries, handles spot interruption
```

**Option 3: ECS Fargate (Serverless containers)**
```
EventBridge at 2AM → ECS Fargate Task
Container: Your processing code
Fargate: Serverless, no instance management
Cost: Higher than Spot but less operational overhead
Good for: Smaller jobs, less tolerance for Spot interruption
```

**Failure Handling:**
```
Spot interruption (Spot): 
  2-minute warning → checkpoint progress → S3
  New Spot instance → resumes from checkpoint
  Never lose more than 2 minutes of progress
```

---

## 📣 LinkedIn Post After Completing Scenarios

```
💡 Day 1 Scenario Practice Done! #CloudDevOpsHub

Today I practiced 10 real AWS scenario-based interview questions.

My favorite scenario today:
[Share your favorite scenario and your approach to solving it]

This is the kind of preparation that makes the difference between
'I know AWS' and 'I can DO AWS in production.' 🎯

Attending AWS Cohort-6 by @Vikas Ratnawat | @CloudDevOpsHub 🔥
#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHubCommunity
#VikasRatnawat #AWS #CloudEngineer #InterviewReady
```

---

> 🌟 **Fork → Customize with your experience → Share on LinkedIn**  
> [← Back to Day 1 README](./README.md)