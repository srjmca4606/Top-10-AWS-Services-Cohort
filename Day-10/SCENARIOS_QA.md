# 🔥 Day 10 — Real-World Scenario Q&A: AWS CloudWatch & Monitoring

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

## Scenario 1: Your production application went down at 3AM. How do you set up monitoring to prevent this?

**Your Answer (How to Approach This):**

Proactive monitoring setup: 1. ALB 5XX errors > 10/min → PagerDuty (wake up on-call). 2. EC2 CPU > 85% for 5 min → Auto Scale + Slack. 3. RDS CPU > 80% → Slack. 4. RDS FreeStorageSpace < 10GB → Critical alert. 5. ALB UnhealthyHostCount > 0 → Critical. 6. Lambda error rate > 1% → alert. 7. Custom: business transaction rate drops 50% below baseline → P1. Runbook: each alert has documented investigation steps. On-call rotation. Post-incident: What monitoring would have caught this earlier?

---

## Scenario 2: How do you build a CloudWatch dashboard for an executive-level weekly review?

**Your Answer (How to Approach This):**

Executive dashboards need business metrics, not technical ones. Business layer: Revenue/hour (custom metric from app). Orders/minute. Customer-facing error rate %. System health (simple: green/yellow/red). Cost: Weekly spend vs budget. Infrastructure layer (separate drill-down): Service availability heatmap. Error rates by service. Performance: p50, p95, p99 latency. Capacity: EC2 fleet utilization. Build: CloudWatch Dashboard → mix of metrics and custom widgets. Export: CloudWatch → screenshot Monday morning → email. Or: QuickSight for richer visualization.

---

## Scenario 3: How do you implement log-based alerting for security events?

**Your Answer (How to Approach This):**

CloudWatch Log Metric Filters on CloudTrail logs. Pattern: CreateUser, AttachUserPolicy, PutBucketPolicy, AuthorizeSecurityGroupIngress. Each pattern → custom metric → alarm. Example: metric filter for root login: $.userIdentity.type = 'Root'. Alarm: count > 0 → SNS → PagerDuty. More patterns: Failed console logins > 5 in 5 minutes. S3 bucket policy changed. IAM policy attached. Security group changed. EC2 large instance launched. All → SNS → Security team Slack channel. GuardDuty handles ML-based detection automatically.

---

## Scenario 4: A Lambda function is timing out intermittently. How do you diagnose and fix?

**Your Answer (How to Approach This):**

Diagnose: CloudWatch → Lambda metrics: Duration, Errors, Throttles. X-Ray: enable active tracing. Trace shows: which part of function is slow. Common culprits: 1. DB connection (cold start): Use RDS Proxy — connection pooling, faster connection reuse. 2. External API call slow: Add timeout + retry logic. 3. Lambda in VPC: cold start adds 1-5 seconds for ENI provisioning. Use Provisioned Concurrency for latency-sensitive functions. 4. Memory too low: Increase memory (also increases CPU proportionally). 5. Downstream service slow: Check X-Ray trace — which subsegment is slow?

---

## Scenario 5: How do you implement SLA monitoring for a B2B application?

**Your Answer (How to Approach This):**

Define SLAs: Uptime: 99.9% (downtime <8.7 hours/year). Response time: <500ms for 95% of requests. Error rate: <0.1%. Implement SLIs (Service Level Indicators): Uptime: Route 53 health check (external). Response time: ALB TargetResponseTime p95. Error rate: ALB HTTPCode_5XX / TotalRequests. Calculate SLOs: CloudWatch Metrics + Math expressions. Alert: ErrorBudgetRemaining < 10% → investigate. Report: Monthly SLA report exported from CloudWatch. Customer portal: StatusPage.io integration. Escalation: 99.9% breached → executive notification → root cause analysis.

---

## Scenario 6: How do you monitor costs in real-time and prevent budget overruns?

**Your Answer (How to Approach This):**

Setup: AWS Budgets: Monthly budget alarm at 50%, 80%, 100%. SNS → email + Slack. Cost Anomaly Detection: ML-based — detects unexpected cost spikes. Example: EC2 cost increased 300% vs last week → alert immediately. Daily spend report: Lambda → Cost Explorer API → post to Slack every morning. Tag enforcement: AWS Config rule: require Cost Center tag on all EC2/RDS. Untagged resources → automatic shutdown Lambda. FinOps practice: Weekly cost review meeting. Owner of each service reviews their spend. Monthly optimization sprint: rightsizing, RI purchases, lifecycle rules.

---

## Scenario 7: Design a complete observability stack for a microservices application on EKS.

**Your Answer (How to Approach This):**

3 Pillars of Observability: Metrics: CloudWatch Container Insights (EKS). CPU, Memory per pod/service. Custom metrics via StatsD or Prometheus. Logs: Fluent Bit DaemonSet → CloudWatch Logs. Structured JSON logging (fields: service, trace_id, level, message). CloudWatch Logs Insights for querying. Traces: X-Ray sidecar in each pod. End-to-end trace across all microservices. Identify which service is bottleneck. Dashboards: CloudWatch dashboards. Service map (X-Ray): visualize all service dependencies. Alerting: PagerDuty integration for P1. Slack for P2/P3.

---

## Scenario 8: How do you implement automated remediation with CloudWatch alarms?

**Your Answer (How to Approach This):**

Pattern: CloudWatch Alarm → SNS → Lambda → Remediation. Examples: 1. High CPU: Alarm → Lambda → trigger Auto Scaling scale-out event. 2. Full disk: Alarm → Lambda → extend EBS volume + resize filesystem. 3. RDS connections maxed: Alarm → Lambda → restart application connection pools via SSM Run Command. 4. EC2 status check failed: Alarm → EC2 recover action (built-in). 5. Unused EC2 (CPU < 5% for 7 days): → Lambda → tag for review + Slack message to owner. Runbooks: Document what each automation does. Human approval for destructive actions.

---

## Scenario 9: How do you set up distributed tracing for a payment microservice?

**Your Answer (How to Approach This):**

X-Ray setup: Each service: install X-Ray SDK. Add X-Ray middleware to Express/Flask/Spring. Lambda: enable active tracing (one checkbox). ECS: X-Ray daemon as sidecar container. Trace propagation: X-Ray SDK automatically propagates trace ID in HTTP headers. Services pass X-Amzn-Trace-Id header downstream. Full trace assembled in X-Ray console. Custom annotations: Add orderId, customerId, paymentMethod as annotations. Search traces by orderId to debug specific payment failures. SLO monitoring: X-Ray → CloudWatch → p99 latency alarm for payment service. Alert if p99 > 3 seconds.

---

## Scenario 10: A production incident is happening right now. How do you use AWS tools to diagnose it?

**Your Answer (How to Approach This):**

T+0: Alert fired. Open CloudWatch dashboard. Check: which metrics are abnormal? T+2min: ALB metrics — 5XX errors spiking? Which target group? Which EC2s? T+3min: EC2 instances — CPU/memory normal? Any instance health check failures? T+5min: RDS — connections maxed? CPU high? Replica lag? T+7min: Application logs — CloudWatch Logs Insights: filter ERROR last 10 minutes. Find error message. T+10min: X-Ray — Service map → find unhealthy service. Trace the failing request. Find root cause. T+15min: Fix or rollback. Communicate status to stakeholders. After incident: Postmortem. Add monitoring to catch earlier next time.

---

## 📣 LinkedIn Post After Completing Scenarios

```
💡 Day 10 Scenario Practice Done! #CloudDevOpsHub

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
> [← Back to Day 10 README](./README.md)