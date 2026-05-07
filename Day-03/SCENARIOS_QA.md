# 🔥 Day 3 — Real-World Scenario Q&A: IAM

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

## Scenario 1: You received an alert: 3 failed root account login attempts. What do you do?

**Your Answer (How to Approach This):**

1. Enable AWS GuardDuty alert for root login. 2. Check CloudTrail for source IPs. 3. Enable MFA on root immediately. 4. Review recent root activity. 5. Rotate root password. 6. Consider AWS Organizations SCP to alert on ANY root usage. Interview tip: Root login alerts = potential breach in progress. Treat as P1 incident.

---

## Scenario 2: A developer needs temporary access to production S3. How do you grant it securely?

**Your Answer (How to Approach This):**

Use IAM Role + STS AssumeRole with time-limited credentials. 1. Create IAM role: prod-s3-readonly-role. 2. Attach policy: S3 read only on specific bucket. 3. Add condition: 'aws:RequestedRegion': 'ap-south-1', expiry. 4. Developer: aws sts assume-role --role-arn arn:... --duration-seconds 3600. Gets temp credentials expiring in 1 hour. Never give permanent access for temporary needs. Interview gold: This pattern is used in every enterprise.

---

## Scenario 3: How would you implement least-privilege IAM for a team of 20 developers?

**Your Answer (How to Approach This):**

1. Create IAM Groups not individual policies. Groups: junior-dev (EC2 read, S3 read), senior-dev (EC2 full, S3 write), devops (EC2+RDS+VPC). 2. Use permission boundaries so devs can't grant themselves more access. 3. Access Analyzer to identify unused permissions quarterly. 4. IAM credential report monthly review. 5. Force MFA via policy. Real answer: Start restrictive, expand on request. Document every permission change with ticket number.

---

## Scenario 4: A new microservice on EC2 needs to read from SQS and write to DynamoDB. How do you set this up?

**Your Answer (How to Approach This):**

Create IAM Role (not access keys!). Policy: Allow sqs:ReceiveMessage, sqs:DeleteMessage on specific queue ARN. Allow dynamodb:PutItem, dynamodb:UpdateItem on specific table ARN. Attach role to EC2 at launch or associate afterward. Application uses boto3/SDK which auto-fetches temp credentials from instance metadata. No credentials in code, no .env files, no access keys on disk. This is the correct pattern for all service-to-service AWS communication.

---

## Scenario 5: Audit finds an IAM user with AdministratorAccess. They left the company 6 months ago. What steps do you take?

**Your Answer (How to Approach This):**

Immediate: Deactivate access keys. Disable console password. Check CloudTrail last 6 months for this user. Look for: created new users, changed policies, unusual API calls, S3 data access. If suspicious activity found: treat as security incident, escalate. Remove from all groups. Delete user. Review: How was offboarding missed? Add automated process: HR offboarding triggers IAM disable via ServiceNow/Jira. Prevention: IAM credential report weekly review, access key rotation enforcement.

---

## Scenario 6: How do you implement cross-account access in AWS?

**Your Answer (How to Approach This):**

Use IAM Roles with trust policies. Account A (Dev) needs to access S3 in Account B (Prod). Step 1 (Prod account): Create role prod-s3-read-role. Trust policy: allow Account A's IAM principal to assume. Step 2 (Dev account): IAM policy on dev user: allow sts:AssumeRole on prod role ARN. Step 3: Dev user runs: aws sts assume-role. Gets temp credentials. Real use: Centralized logging account, shared services VPC account, security audit account. AWS Organizations makes this easier with resource-based policies and centralized IAM.

---

## Scenario 7: What is your process when a new junior joins and needs AWS access?

**Your Answer (How to Approach This):**

Day 1: Create IAM user, enforce MFA, temporary password must change. Assign to correct group based on role (no individual policies). Add to relevant AWS accounts via AWS SSO/Identity Center. Day 2-30: Access only what they need for current sprint. Month 1 review: Access Advisor check — what did they actually use? Trim what wasn't used (least privilege cleanup). Offboarding automation: HR system → Lambda → disable IAM user → remove from all groups → archive credential report entry. All provisioning tracked in ticket system.

---

## Scenario 8: How do you secure API Gateway with IAM?

**Your Answer (How to Approach This):**

Option 1: IAM Auth. Caller signs requests with SigV4. API Gateway validates signature against IAM. Used for service-to-service. Option 2: Cognito User Pool. User logs in → gets JWT. JWT passed to API Gateway → validates token. Used for user-facing APIs. Option 3: Lambda Authorizer. Custom JWT/API key validation. Maximum flexibility. Option 4: Resource Policies. Restrict which IPs/VPCs/accounts can call the API. Real pattern: Public API → Cognito. Internal microservices → IAM Auth. Both with WAF rate limiting. Always enable CloudTrail for API Gateway.

---

## Scenario 9: An EC2 instance needs to call external APIs (payment gateway). How do you manage secrets?

**Your Answer (How to Approach This):**

Never hardcode credentials. Option 1 (Recommended): AWS Secrets Manager. Store API key encrypted. EC2 IAM role: secretsmanager:GetSecretValue on specific secret ARN. Code: sdk.get_secret_value(SecretId='payment-gateway-key'). Auto-rotation supported (for DB passwords). Audit trail: CloudTrail logs every secret access. Option 2: SSM Parameter Store. Similar but cheaper. Use SecureString (KMS encrypted). Best for non-rotating config. Option 3: Environment variables from Secrets Manager. Inject at container/ECS task startup. Never: .env files on EC2, Git secrets, EC2 user data with credentials.

---

## Scenario 10: How would you set up ABAC (Attribute-Based Access Control) in AWS IAM?

**Your Answer (How to Approach This):**

ABAC uses tags to control access instead of managing individual policies. Example: Developer with tag Team=payments can only access resources tagged Team=payments. IAM Policy: Condition: StringEquals aws:ResourceTag/Team: aws:PrincipalTag/Team. Tag EC2 instances, RDS, S3 with Team=payments, Team=orders, etc. Developers have IAM tag Team=payments. Result: Dev in payments team auto-gets access to all payments resources, zero access to orders resources. Benefit: Scale to 1000 resources without adding any new IAM policies. Just tag the resource!

---

## 📣 LinkedIn Post After Completing Scenarios

```
💡 Day 3 Scenario Practice Done! #CloudDevOpsHub

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
> [← Back to Day 3 README](./README.md)