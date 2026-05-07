# 🎯 Day 8 — Interview Questions & Answers: AWS VPC Overview

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

## Q1. Design a VPC for a 3-tier web application that must be PCI-DSS compliant.

**Answer:**

PCI-DSS requires strict network segmentation. Architecture: Internet → WAF → ALB (public subnet). ALB → App servers (private subnet). App servers → RDS (isolated DB subnet — no internet route AT ALL). Security: NACL: block all between tiers except required ports. Security Groups: each tier has its own SG, only allows traffic from adjacent tier. VPC Flow Logs: all traffic logged to S3 for 1 year. No direct SSH — SSM Session Manager only. All traffic encrypted: HTTPS, TLS for DB connections. Inspector: continuous vulnerability scanning. Config: compliance rules monitored continuously.

---

## Q2. Two of your VPCs have overlapping CIDRs and now you can't peer them. How do you resolve?

**Answer:**

Problem: VPC-A: 10.0.0.0/16, VPC-B: 10.0.0.0/16. Cannot peer (overlapping). Solutions: Option 1 (if VPCs are new): Re-create one VPC with non-overlapping CIDR. Option 2 (existing VPCs): AWS PrivateLink — share specific services without needing full VPC peering. No CIDR conflict. Option 3: Application-level proxy. EC2 in each VPC with different IPs, proxy traffic. Option 4: AWS Transit Gateway + NAT. Complex but workable. Prevention: IPAM (IP Address Manager) — plan all CIDRs centrally before creating any VPC.

---

## Q3. An EC2 in a private subnet can't reach the internet. Troubleshoot.

**Answer:**

Checklist: 1. Does private subnet route table have: 0.0.0.0/0 → NAT Gateway? 2. Is NAT Gateway in PUBLIC subnet (not private!)? 3. Does NAT Gateway have an Elastic IP? 4. Does public subnet route: 0.0.0.0/0 → Internet Gateway? 5. Security Group on EC2: outbound allows HTTPS (443)? 6. NACL on private subnet: outbound allows required ports + inbound allows 1024-65535? 7. Is NAT Gateway in the SAME AZ as EC2? (Best practice — separate NAT per AZ). Test: curl --connect-timeout 5 https://google.com

---

## Q4. How do you implement network segmentation for a microservices architecture?

**Answer:**

Each microservice in its own subnet. SG per microservice: only allow traffic from services that legitimately call it. Example: Payment service SG: Inbound 8080 from Order service SG only. Order service SG: Inbound 8080 from API Gateway SG only. No microservice talks to another unless explicitly needed. Use App Mesh (service mesh): mTLS between services, no unauthorized connections. VPC Flow Logs: detect unexpected inter-service communication. Service-to-service: IAM auth via API Gateway, not network-level trust.

---

## Q5. Your NAT Gateway is costing $2000/month. How do you reduce this?

**Answer:**

Analysis: Cost Explorer → NAT Gateway → split by data transfer. Find top talkers: VPC Flow Logs + Athena query: top EC2s by bytes through NAT. Common causes: EC2s downloading packages repeatedly. Fix: Use S3 VPC Endpoint (FREE) for S3 traffic. Savings: huge if lots of S3 access. Use ECR VPC Endpoint for Docker pulls. Instance Metadata: remove unnecessary outbound traffic. One NAT per AZ: ensure EC2s use NAT in same AZ (cross-AZ NAT adds transfer cost). CloudFront: for content delivery instead of direct S3 via NAT. Typical result: 60-80% cost reduction.

---

## 📣 LinkedIn Post After Practicing These

```
🎯 Day 8 Interview Prep Done! #AWSVPCOverview #CloudDevOpsHub

Practiced 5 real-world AWS interview questions today!

Most important thing I learned:
[Write your key takeaway from AWS VPC Overview]

These aren't textbook answers — they're real scenarios.
Thanks to @Vikas Ratnawat | @CloudDevOpsHub for the depth! 🙏

#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHub
#CloudDevOpsHubCommunity #VikasRatnawat #AWS #InterviewPrep
```

---

> 🌟 **Fork → Customize → Push → Share on LinkedIn**  
> [← Back to Day 8 README](./README.md)