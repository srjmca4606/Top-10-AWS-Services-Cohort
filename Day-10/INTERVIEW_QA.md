# 🎯 Day 10 — Interview Questions & Answers: AWS CloudWatch & Monitoring

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

## Q1. Your production application went down at 3AM. How do you set up monitoring to prevent this?

**Answer:**

Proactive monitoring setup: 1. ALB 5XX errors > 10/min → PagerDuty (wake up on-call). 2. EC2 CPU > 85% for 5 min → Auto Scale + Slack. 3. RDS CPU > 80% → Slack. 4. RDS FreeStorageSpace < 10GB → Critical alert. 5. ALB UnhealthyHostCount > 0 → Critical. 6. Lambda error rate > 1% → alert. 7. Custom: business transaction rate drops 50% below baseline → P1. Runbook: each alert has documented investigation steps. On-call rotation. Post-incident: What monitoring would have caught this earlier?

---

## Q2. How do you build a CloudWatch dashboard for an executive-level weekly review?

**Answer:**

Executive dashboards need business metrics, not technical ones. Business layer: Revenue/hour (custom metric from app). Orders/minute. Customer-facing error rate %. System health (simple: green/yellow/red). Cost: Weekly spend vs budget. Infrastructure layer (separate drill-down): Service availability heatmap. Error rates by service. Performance: p50, p95, p99 latency. Capacity: EC2 fleet utilization. Build: CloudWatch Dashboard → mix of metrics and custom widgets. Export: CloudWatch → screenshot Monday morning → email. Or: QuickSight for richer visualization.

---

## Q3. How do you implement log-based alerting for security events?

**Answer:**

CloudWatch Log Metric Filters on CloudTrail logs. Pattern: CreateUser, AttachUserPolicy, PutBucketPolicy, AuthorizeSecurityGroupIngress. Each pattern → custom metric → alarm. Example: metric filter for root login: $.userIdentity.type = 'Root'. Alarm: count > 0 → SNS → PagerDuty. More patterns: Failed console logins > 5 in 5 minutes. S3 bucket policy changed. IAM policy attached. Security group changed. EC2 large instance launched. All → SNS → Security team Slack channel. GuardDuty handles ML-based detection automatically.

---

## Q4. A Lambda function is timing out intermittently. How do you diagnose and fix?

**Answer:**

Diagnose: CloudWatch → Lambda metrics: Duration, Errors, Throttles. X-Ray: enable active tracing. Trace shows: which part of function is slow. Common culprits: 1. DB connection (cold start): Use RDS Proxy — connection pooling, faster connection reuse. 2. External API call slow: Add timeout + retry logic. 3. Lambda in VPC: cold start adds 1-5 seconds for ENI provisioning. Use Provisioned Concurrency for latency-sensitive functions. 4. Memory too low: Increase memory (also increases CPU proportionally). 5. Downstream service slow: Check X-Ray trace — which subsegment is slow?

---

## Q5. How do you implement SLA monitoring for a B2B application?

**Answer:**

Define SLAs: Uptime: 99.9% (downtime <8.7 hours/year). Response time: <500ms for 95% of requests. Error rate: <0.1%. Implement SLIs (Service Level Indicators): Uptime: Route 53 health check (external). Response time: ALB TargetResponseTime p95. Error rate: ALB HTTPCode_5XX / TotalRequests. Calculate SLOs: CloudWatch Metrics + Math expressions. Alert: ErrorBudgetRemaining < 10% → investigate. Report: Monthly SLA report exported from CloudWatch. Customer portal: StatusPage.io integration. Escalation: 99.9% breached → executive notification → root cause analysis.

---

## 📣 LinkedIn Post After Practicing These

```
🎯 Day 10 Interview Prep Done! #AWSCloudWatch&Monitoring #CloudDevOpsHub

Practiced 5 real-world AWS interview questions today!

Most important thing I learned:
[Write your key takeaway from AWS CloudWatch & Monitoring]

These aren't textbook answers — they're real scenarios.
Thanks to @Vikas Ratnawat | @CloudDevOpsHub for the depth! 🙏

#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHub
#CloudDevOpsHubCommunity #VikasRatnawat #AWS #InterviewPrep
```

---

> 🌟 **Fork → Customize → Push → Share on LinkedIn**  
> [← Back to Day 10 README](./README.md)