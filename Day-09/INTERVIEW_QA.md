# 🎯 Day 9 — Interview Questions & Answers: AWS Networking Advanced

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

## Q1. Your VPC peering connection is established but instances can't communicate. Debug.

**Answer:**

Checklist: 1. Both route tables updated? VPC-A needs route to VPC-B CIDR via pcx-xxx. VPC-B needs route to VPC-A CIDR via pcx-xxx. Most common miss! 2. Security Groups: does target EC2's SG allow traffic from source CIDR (not just SG reference — different VPCs need IP ranges). 3. NACL: are stateless rules allowing both inbound and outbound (with ephemeral ports)? 4. No overlapping CIDRs? 5. Check VPC Peering Status: Active? 6. Is traffic going cross-region? Ensure inter-region peering data transfer costs are expected. Test: nc -zv PRIVATE_IP PORT

---

## Q2. A user reports intermittent connection drops to your application. How do you investigate?

**Answer:**

Intermittent = harder to debug. Approach: 1. Enable VPC Flow Logs. Query rejected connections for the user's IP. 2. Check ELB access logs: connection errors, 504 timeout patterns. 3. Check EC2 health checks in Target Group: was EC2 marked unhealthy? 4. CloudWatch: NetworkPacketsOut, TCP connections graph. 5. Check NACL: NACLs are stateless. If ephemeral ports not properly configured → random drops. 6. Check if instance ran out of ENI capacity: network buffer exhaustion. 7. Check security group rules — any time-limited rules that expired?

---

## Q3. How do you implement microsegmentation in a Kubernetes cluster on EKS?

**Answer:**

Kubernetes Network Policies: Define which pods can talk to which. NetworkPolicy resource controls pod-to-pod traffic. Example: payments-pod can only receive from orders-pod, nothing else. AWS VPC CNI: Each pod gets a VPC IP. Security Groups for Pods: Apply SG to individual pods (not just nodes). Pod isolation: payments pod SG: only allow 8080 from orders pod SG. Infrastructure level: Node groups in private subnets. API server access: private endpoint only. Calico or Cilium: advanced network policy engine. Service mesh (App Mesh/Istio): mTLS between pods.

---

## Q4. Design a hub-and-spoke network for an enterprise with 50 AWS accounts.

**Answer:**

Hub account: Shared Services VPC. Contains: NAT Gateway (shared), DNS, monitoring, security tools. Transit Gateway: deployed in hub account. Share via Resource Access Manager (RAM) to all spoke accounts. Each spoke account: Attaches their VPC to TGW. Route table: all traffic → TGW. Internet: all internet traffic routes via hub NAT (centralized egress). Security: Centralized egress filtering in hub VPC (Network Firewall or 3rd party). No direct spoke-to-internet. Cost: 1 NAT Gateway vs 50. Monitoring: Centralized flow logs.

---

## Q5. How do you implement private DNS resolution across multiple VPCs?

**Answer:**

Route 53 Resolver. Inbound Endpoints: On-premises → query AWS private DNS. Outbound Endpoints: AWS → query on-premises DNS. Forwarding Rules: VPC-A: forward *.corp.internal → Resolver Outbound → On-prem DNS. Shared: AWS RAM share resolver rules across all VPCs. Central DNS account: All VPCs associated with private hosted zones. VPCs share same private hosted zone. Resources registered by name. Any VPC can resolve any other VPC's private names. Example: api.internal resolves in all VPCs.

---

## 📣 LinkedIn Post After Practicing These

```
🎯 Day 9 Interview Prep Done! #AWSNetworkingAdvanced #CloudDevOpsHub

Practiced 5 real-world AWS interview questions today!

Most important thing I learned:
[Write your key takeaway from AWS Networking Advanced]

These aren't textbook answers — they're real scenarios.
Thanks to @Vikas Ratnawat | @CloudDevOpsHub for the depth! 🙏

#AWSCohortChallenge #10Days10AWSTasks #CloudDevOpsHub
#CloudDevOpsHubCommunity #VikasRatnawat #AWS #InterviewPrep
```

---

> 🌟 **Fork → Customize → Push → Share on LinkedIn**  
> [← Back to Day 9 README](./README.md)