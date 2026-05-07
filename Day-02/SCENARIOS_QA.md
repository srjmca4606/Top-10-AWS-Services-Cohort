# 🔥 Day 2 — Real-World Scenario Q&A: EC2 Deep Dive

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

## Scenario 1: Your Nginx web server returns 502 Bad Gateway. How do you debug it?

**Your Answer (How to Approach This):**

**502 Bad Gateway = Nginx received invalid response from upstream app**

**Debugging Steps:**

```
Step 1: Check Nginx error log
sudo tail -100 /var/log/nginx/error.log
Look for: "connect() failed", "no live upstreams", "upstream timed out"

Step 2: Check if upstream app is running
sudo systemctl status node  # or gunicorn, php-fpm, etc.
ps aux | grep node

Step 3: Check if app is listening on correct port
netstat -tlnp | grep 3000  # App should be on port 3000
ss -tlnp | grep 3000

Step 4: Check Nginx config
sudo nginx -t  # Test config syntax
cat /etc/nginx/conf.d/myapp.conf
# Look at proxy_pass:
# proxy_pass http://localhost:3000;  ← correct port?

Step 5: Test upstream directly (bypass Nginx)
curl -v http://localhost:3000/health
# If this fails → app is down
# If this works → Nginx config issue

Step 6: Check app logs
sudo journalctl -u myapp -n 100
sudo tail -100 /var/log/myapp/error.log
```

**Common Causes & Fixes:**
- App not running → start it: `sudo systemctl start myapp`
- App on wrong port → fix Nginx config: `proxy_pass http://localhost:PORT`
- App crashed → check app logs for error → fix code bug
- App overloaded → add more instances, enable upstream keepalive
- Nginx upstream timeout → increase `proxy_read_timeout 60s;`

---

## Scenario 2: You need to deploy a web application that can handle 1 million requests per day with high availability. Design the architecture.

**Your Answer (How to Approach This):**

**Architecture for 1M Requests/Day (High Availability):**

**Calculation:**
```
1M requests/day ÷ 86,400 seconds = ~12 requests/second average
Peak (10x average): ~120 requests/second
Each EC2 t3.medium: handles ~200 req/s for typical apps
Minimum: 2 EC2s (1 EC2 = failure, 2 = HA)
```

**Architecture:**
```
                    [Route 53 DNS]
                          │
                    [CloudFront CDN]
                    (static assets cached globally)
                          │
                    [ALB in 2 AZs]
                    (HTTPS termination, health checks)
                   /                        [AZ-1a]                [AZ-1b]
    [EC2 t3.medium]          [EC2 t3.medium]
    (Auto Scaling)            (Auto Scaling)
          │                        │
          └──────────┬─────────────┘
                     │
            [ElastiCache Redis]
            (session, hot data cache)
                     │
            [RDS MySQL Multi-AZ]
            (Primary in AZ-1a)
            (Standby in AZ-1b)
                     │
                   [S3]
            (static assets, backups)
```

**Auto Scaling:** min=2, desired=2, max=10
**ALB:** health check every 30s, deregister unhealthy instances
**CloudFront:** cache static content (CSS, JS, images) at edge
**ElastiCache:** cache database reads (product catalog, user sessions)

**Cost:** ~$200-300/month (Mumbai region)
**Availability:** 99.99% (Multi-AZ RDS + 2 AZ deployment)

---

## Scenario 3: A client says their website is slow. How do you identify if it's an EC2 or application problem?

**Your Answer (How to Approach This):**

**Systematic Performance Investigation:**

```
Step 1: Measure — Establish facts first
  Browser: F12 → Network tab → TTFB (Time To First Byte)
  < 200ms: Server is fast, check frontend
  > 1000ms: Server is slow, investigate backend

Step 2: Check EC2 resources
  CloudWatch: CPU, Memory, NetworkIn/Out, DiskIOPS
  SSH → top/htop
  
  CPU high (>80%)? → App is compute-bound
    Solution: Scale up or optimize code
  
  Memory high (>85%)? → Memory pressure
    Solution: Increase instance size or fix memory leak
  
  Disk I/O high? → Storage bottleneck
    Solution: Upgrade to gp3 with more IOPS

Step 3: Check application logs for slow requests
  tail -f /var/log/nginx/access.log | awk '$NF > 1'
  # Shows requests taking >1 second

Step 4: Check database queries
  Enable slow query log in RDS
  Performance Insights → Top SQL
  Slow query? → Missing index? → EXPLAIN the query

Step 5: Network path
  Is CloudFront caching static assets?
  Are API responses being cached in Redis?
  Large response payloads? Enable gzip compression

Step 6: Load test to confirm findings
  ab -n 1000 -c 50 https://myapp.com/api/products
  # 50 concurrent users, 1000 total requests
  # Shows requests/sec, response time percentiles
```

**Most common root causes (in order):**
1. Missing database index (80% of cases!)
2. No caching layer (hitting DB on every request)
3. N+1 query problem
4. EC2 under-provisioned
5. Unoptimized images (for frontend)

---

## Scenario 4: How do you perform a blue-green deployment on EC2 with zero downtime?

**Your Answer (How to Approach This):**

**Blue-Green Deployment on AWS (Zero Downtime):**

**Concept:**
```
Blue Environment = Current production (v1)
Green Environment = New version (v2)

Traffic: 100% → Blue
Deploy to Green → Test Green
Shift traffic: 100% → Green
Blue becomes standby (rollback option)
```

**Implementation with ALB + ASG:**

```
Step 1: Current State
  ALB → Target Group Blue → EC2s running v1
  DNS: myapp.com → ALB DNS

Step 2: Deploy New Version
  Launch new EC2 ASG (Green) with v2 code
  Register Green ASG with new Target Group (Green TG)
  Health checks pass on Green EC2s

Step 3: Weighted traffic shift (canary approach)
  ALB Listener Rules:
    5% → Green TG (test with small traffic)
  Monitor: error rates, latency on Green
  
  If good:
    50% → Green TG
  Wait, monitor
  
  If good:
    100% → Green TG
  
Step 4: Verify then decommission Blue
  Monitor Green for 30 minutes
  If all good → terminate Blue ASG (saves cost)
  If issue → revert: 100% → Blue TG (instant rollback!)
```

**Using CodeDeploy (Automated):**
```
CodeDeploy → Blue/Green deployment type
Automatically:
  Launches new instances
  Deploys code
  Shifts traffic
  Terminates old instances after N minutes
  Handles rollback if health checks fail
```

**RDS during Blue-Green:**
```
DB changes must be backward-compatible!
Blue (v1) and Green (v2) share same RDS during transition
Use feature flags to activate new DB schema usage
Old columns deleted only after 100% traffic on Green
```

---

## Scenario 5: Your company needs to serve a static website globally with low latency. What do you recommend?

**Your Answer (How to Approach This):**

**Global Static Website Architecture:**

```
Architecture:
User (India) → CloudFront Edge (Mumbai) → cached → <10ms response
User (US) → CloudFront Edge (N. Virginia) → cached → <10ms response
User (Europe) → CloudFront Edge (Frankfurt) → cached → <10ms response

Cache miss → CloudFront → Origin S3 bucket (Mumbai) → cache and serve
```

**Implementation Steps:**
```
Step 1: S3 Bucket
  Create S3 bucket: mywebsite-prod
  Upload HTML, CSS, JS, images
  Enable Static Website Hosting
  Block public access (CloudFront will access it)
  Add bucket policy: allow CloudFront Origin Access Identity

Step 2: CloudFront Distribution
  Origins: S3 bucket
  OAI (Origin Access Identity): secure S3 → CloudFront only
  Caching: 
    HTML → 24 hours
    CSS/JS → 1 year (use content hash in filename)
    Images → 1 year
  Compress: Enable gzip/Brotli automatically
  HTTPS: Default (SSL certificate included free)
  IPv6: Enable

Step 3: Custom Domain + HTTPS
  Route 53: CNAME myapp.com → CloudFront distribution
  ACM (Certificate Manager): Free SSL certificate
  CloudFront: Attach certificate

Step 4: CI/CD Pipeline
  CodePipeline: GitHub push → CodeBuild → S3 sync → CloudFront invalidation
```

**Performance:**
```
Without CloudFront: Mumbai user → US S3 → ~200ms latency
With CloudFront: Mumbai user → Mumbai edge → ~5ms latency (40x faster!)
```

**Cost for typical static website:**
```
S3: < $1/month (most sites)
CloudFront: ~$0.008/GB + $0.0075 per 10K requests
Total: $2-5/month for typical traffic
```

---

## Scenario 6: You have 50 EC2 instances and need to run a script on all of them at once. How do you do it?

**Your Answer (How to Approach This):**

**Methods to Run Commands on 50 EC2 Instances:**

**Method 1: AWS Systems Manager Run Command (Recommended)**
```
SSM → Run Command → AWS-RunShellScript
Target: Tag-based (Environment=Production) → auto-selects all 50
Command:
  #!/bin/bash
  yum update -y
  systemctl restart myapp

Execution:
  Concurrency: 10 (run on 10 at a time to avoid overload)
  Error threshold: 5% (stop if >5% fail)
  Output: CloudWatch Logs (each instance's output captured)

Benefits:
  - No SSH needed, no key management
  - Full audit trail in CloudTrail
  - Output captured centrally
  - Works on instances WITHOUT public IPs
```

**Method 2: SSM Session Manager + Script**
```
For interactive sessions when needed
aws ssm start-session --target i-1234567890abcdef0
# Opens shell without SSH!
```

**Method 3: Ansible (for complex configuration management)**
```yaml
# inventory: uses AWS dynamic inventory plugin
# ansible.cfg: uses SSM as connection method

- hosts: tag_Environment_Production
  become: yes
  tasks:
    - name: Update packages
      yum:
        name: "*"
        state: latest
    - name: Restart app
      systemd:
        name: myapp
        state: restarted
```

**Method 4: AWS Systems Manager Patch Manager**
For OS patching specifically:
```
SSM → Patch Manager → Patch baseline → Maintenance window
Auto-patches all tagged instances on schedule
Post-patch: automatically restarts as needed
```

**Best Practice:**
SSM Run Command for ad-hoc → Ansible/SSM State Manager for continuous config management

---

## Scenario 7: A security team says EC2 instances shouldn't have public IPs. How do you maintain operational access?

**Your Answer (How to Approach This):**

**Secure Access Without Public IPs:**

**Current (insecure):** Public EC2 → SSH from internet (port 22 open)

**Target (secure):** Private EC2 → No public IP → Access via SSM or Bastion

**Solution 1: AWS Systems Manager Session Manager (Best)**
```
Requirements:
  - IAM role on EC2 with AmazonSSMManagedInstanceCore policy
  - SSM Agent installed (pre-installed on Amazon Linux 2)
  - VPC Interface Endpoint for SSM (if no internet access)

Usage:
  AWS Console → SSM → Session Manager → Start Session
  OR
  aws ssm start-session --target i-1234567890abcdef0

Benefits:
  ✅ No port 22 open anywhere
  ✅ No key pairs needed
  ✅ Full session audit in CloudTrail
  ✅ Session logging to S3/CloudWatch
  ✅ Works through VPC Endpoints (no internet)
  ✅ IAM controls who can access which instances
```

**Solution 2: Bastion Host (Hardened)**
```
Private EC2 → only SSH via Bastion
Bastion: 
  t3.micro in public subnet
  SG: Allow 22 from corporate IPs ONLY (not 0.0.0.0/0!)
  SSH Agent Forwarding or ProxyJump
  CloudTrail: log all SSH sessions
  MFA: enforce MFA to access bastion IAM role
```

**Solution 3: AWS VPN + Private Access**
```
Corporate VPN → connects to AWS VPC
Internal developers → VPN → access private IPs directly
No internet-facing access at all
```

**For compliance:** SSM Session Manager provides complete audit trail — who connected, when, what commands ran — which satisfies SOC 2, PCI-DSS requirements.

---

## Scenario 8: How do you handle EC2 instance metadata securely?

**Your Answer (How to Approach This):**

**EC2 Instance Metadata Service (IMDS):**

IMDS provides EC2 instances information about themselves (instance ID, IAM credentials, region, etc.)

**Endpoint:** `http://169.254.169.254/latest/meta-data/`

**Security Risk — IMDSv1:**
```
If a vulnerable app on EC2 allows SSRF (Server-Side Request Forgery):
Attacker → sends request to app → app fetches http://169.254.169.254/iam/security-credentials/
→ Attacker gets temporary AWS credentials → can access AWS APIs!

Real incident: Capital One breach 2019 — SSRF + IMDSv1 = massive data breach
```

**Fix — IMDSv2 (Token-Based):**
```
IMDSv2 requires a PUT request first to get a session token
Then use token for actual metadata request

SSRF attacks can't do PUT requests easily!

Step 1: Get token (expires in 21600 seconds = 6 hours)
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token"   -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")

Step 2: Use token
curl -H "X-aws-ec2-metadata-token: $TOKEN"   http://169.254.169.254/latest/meta-data/iam/security-credentials/
```

**Enforce IMDSv2 at launch:**
```
Launch Template → Advanced Details:
  Metadata accessible: Enabled
  IMDSv2: Required (not Optional!)
  Hop limit: 1 (prevents container escape)
```

**Enforce org-wide via SCP:**
```json
{
  "Effect": "Deny",
  "Action": "ec2:RunInstances",
  "Resource": "arn:aws:ec2:*:*:instance/*",
  "Condition": {
    "StringNotEquals": {"ec2:MetadataHttpTokens": "required"}
  }
}
```

---

## Scenario 9: Your team wants to deploy the same infrastructure in multiple AWS regions for DR. How do you approach this?

**Your Answer (How to Approach This):**

**Multi-Region DR Strategy:**

**Step 1: Choose RTO/RPO targets first**
```
RTO (Recovery Time Objective): How long can we be down?
  < 15 minutes → Active-Active
  < 1 hour → Active-Passive (Warm Standby)
  < 4 hours → Pilot Light
  > 4 hours → Backup & Restore

RPO (Recovery Point Objective): How much data can we lose?
  Near-zero → Synchronous replication
  < 15 min → Async replication
  Hours → Snapshot-based backup
```

**Strategy 1: Active-Active (RTO: seconds)**
```
Route 53 Latency Routing:
  India users → Mumbai region (ap-south-1)
  US users → N. Virginia (us-east-1)
  Both regions handle traffic simultaneously

Database:
  Aurora Global Database
  Primary: Mumbai → Replicates in <1 second to N. Virginia
  Failover: Route 53 health check detects Mumbai down → all traffic to Virginia
```

**Strategy 2: Warm Standby (RTO: 30 minutes)**
```
Primary: Mumbai (full production size)
DR: N. Virginia (smaller, but running)
  EC2: t3.micro (scale up to t3.large on failover)
  RDS: Aurora read replica (promote to primary on failover)
  ASG: Scaled down, scales up on failover

Failover steps (30 minutes):
  Promote Aurora replica
  Scale up ASG
  Route 53: switch DNS to N. Virginia
```

**Infrastructure as Code (Critical!):**
```
Terraform: Same code, different region variables
  terraform apply -var="region=ap-south-1" → Mumbai
  terraform apply -var="region=us-east-1"  → Virginia
  Identical infrastructure guaranteed!

Never manually build DR environment!
```

---

## Scenario 10: How do you implement cost optimization for 100+ EC2 instances in production?

**Your Answer (How to Approach This):**

**EC2 Cost Optimization for Large Fleet (100+ Instances):**

**Step 1: Right-sizing Analysis**
```
CloudWatch Metrics → CPU, Memory, Network over 30 days
Compute Optimizer:
  → Analyzes actual utilization
  → Recommends: "This t3.xlarge runs at 8% CPU → downsize to t3.medium"
  → Potential saving: 40-60% on over-provisioned instances

Tool: AWS Cost Explorer → Rightsizing Recommendations
Action: Replace instance types (stop, change type, start)
Target: No instance running at <20% CPU average
```

**Step 2: Purchasing optimization**
```
Classify workloads:
  Category A: 24/7 critical production → Reserved (1yr or 3yr)
  Category B: Standard business hours → Scheduled Reserved
  Category C: Batch/CI/CD → Spot Instances
  Category D: Variable → On-Demand + Savings Plans

Typical 100-instance fleet:
  40 instances → Reserved 1yr (always on)    : 40% savings
  30 instances → Savings Plans               : 40% savings  
  20 instances → Spot (batch jobs, test)      : 70% savings
  10 instances → On-Demand (unpredictable)   : baseline

Total fleet savings: ~45% vs all On-Demand
```

**Step 3: Automation — Stop/Start Schedules**
```
Dev/Test: Not needed 24/7
  Lambda + EventBridge:
    7PM IST: Stop all dev EC2 instances
    8AM IST: Start all dev EC2 instances
  Savings: 50% (off 13 hours/day + weekends)
  
Instance Scheduler (AWS Solution):
  Free AWS solution, configure via DynamoDB
  Tags: Schedule=BusinessHours → auto stop/start
```

**Step 4: Savings Plans vs Reserved Instances**
```
Savings Plans: Commit to $/hour spend (flexible across instance types)
Reserved: Commit to specific instance type (less flexible, slightly cheaper)

Recommendation: Compute Savings Plans for most → flexibility for fleet changes
```

**Step 5: Monthly Review Process**
```
First Monday of month:
  1. Compute Optimizer → act on new rightsizing recommendations
  2. Cost Explorer → identify top 10 cost drivers
  3. Unused resources → EBS volumes, EIPs, idle NAT Gateways
  4. Reserved Instance coverage report → buy new RIs for uncovered instances
```

---

## 📣 LinkedIn Post After Completing Scenarios

```
💡 Day 2 Scenario Practice Done! #CloudDevOpsHub

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
> [← Back to Day 2 README](./README.md)