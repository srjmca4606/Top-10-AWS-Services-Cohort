# 🎯 Day 3 — Interview Questions & Answers: IAM

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

## Q1. You received an alert: 3 failed root account login attempts. What do you do?

**Answer:**

1. Enable AWS GuardDuty alert for root login. 2. Check CloudTrail for source IPs. 3. Enable MFA on root immediately. 4. Review recent root activity. 5. Rotate root password. 6. Consider AWS Organizations SCP to alert on ANY root usage. Interview tip: Root login alerts = potential breach in progress. Treat as P1 incident.

---

## Q2. A developer needs temporary access to production S3. How do you grant it securely?

**Answer:**

Use IAM Role + STS AssumeRole with time-limited credentials. 1. Create IAM role: prod-s3-readonly-role. 2. Attach policy: S3 read only on specific bucket. 3. Add condition: 'aws:RequestedRegion': 'ap-south-1', expiry. 4. Developer: aws sts assume-role --role-arn arn:... --duration-seconds 3600. Gets temp credentials expiring in 1 hour. Never give permanent access for temporary needs. Interview gold: This pattern is used in every enterprise.

---

## Q3. How would you implement least-privilege IAM for a team of 20 developers?

**Answer:**

1. Create IAM Groups not individual policies. Groups: junior-dev (EC2 read, S3 read), senior-dev (EC2 full, S3 write), devops (EC2+RDS+VPC). 2. Use permission boundaries so devs can't grant themselves more access. 3. Access Analyzer to identify unused permissions quarterly. 4. IAM credential report monthly review. 5. Force MFA via policy. Real answer: Start restrictive, expand on request. Document every permission change with ticket number.

---

## Q4. A new microservice on EC2 needs to read from SQS and write to DynamoDB. How do you set this up?

**Answer:**

Create IAM Role (not access keys!). Policy: Allow sqs:ReceiveMessage, sqs:DeleteMessage on specific queue ARN. Allow dynamodb:PutItem, dynamodb:UpdateItem on specific table ARN. Attach role to EC2 at launch or associate afterward. Application uses boto3/SDK which auto-fetches temp credentials from instance metadata. No credentials in code, no .env files, no access keys on disk. This is the correct pattern for all service-to-service AWS communication.

---

## Q5. Audit finds an IAM user with AdministratorAccess. They left the company 6 months ago. What steps do you take?

**Answer:**

Immediate: Deactivate access keys. Disable console password. Check CloudTrail last 6 months for this user. Look for: created new users, changed policies, unusual API calls, S3 data access. If suspicious activity found: treat as security incident, escalate. Remove from all groups. Delete user. Review: How was offboarding missed? Add automated process: HR offboarding triggers IAM disable via ServiceNow/Jira. Prevention: IAM credential report weekly review, access key rotation enforcement.

---

## 📣 LinkedIn Post After Practicing These

```
🎯 Day 3 Interview Prep Done! #IAM #CloudDevOpsHub

Practiced 5 real-world AWS interview questions today!

Most important thing I learned:
[Write your key takeaway from IAM]

These aren't textbook answers — they're real scenarios.
Thanks to @Vikas Ratnawat | @CloudDevOpsHub for the depth! 🙏

#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHub
#CloudDevOpsHubCommunity #VikasRatnawat #AWS #InterviewPrep
```

---

> 🌟 **Fork → Customize → Push → Share on LinkedIn**  
> [← Back to Day 3 README](./README.md)