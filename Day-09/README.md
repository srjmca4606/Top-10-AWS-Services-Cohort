# 🔗 Day 9 — AWS Networking (NACL, VPC Peering & Private EC2)

> **Cohort:** AWS Cloud Cohort by CloudDevOpsHub Community

---

## 🎯 Today's Goal

Go **deep into advanced networking** — create a real VPC with private subnets, launch EC2 in a private subnet, connect two VPCs with VPC Peering, and understand NACL security. This is what Senior Cloud Engineers do daily. 🛡️

---

## 📚 Topics Covered

| # | Topic |
|---|-------|
| 🛡️ | NACL — Network Access Control List (Subnet-Level Security) |
| 🔗 | VPC Peering — Private Connectivity Between VPCs |
| 🧪 | VPC Project — Real-Time Hands-On Architecture |
| 🌍 | Internet Gateway (IGW) — Internet Access for VPC |
| 📌 | VPC Peering — Limitations, Routing & Best Practices |

---

## 🛠️ Daily Task — Practical Challenge

### ✔ Practical 8: Build Full VPC with Private EC2

**Architecture we'll build:**
```
INTERNET
    │
    ▼
Internet Gateway
    │
    ▼
┌──────────────────────────────────────┐
│  VPC: 10.10.0.0/16                   │
│                                      │
│  Public Subnet: 10.10.1.0/24        │
│  ┌────────────────────────────┐      │
│  │  Bastion Host (EC2)         │      │
│  │  Public IP                 │      │
│  └────────────┬───────────────┘      │
│               │ SSH (port 22)        │
│  Private Subnet: 10.10.2.0/24       │
│  ┌────────────▼───────────────┐      │
│  │  Private EC2 (App Server)  │      │
│  │  NO Public IP              │      │
│  └────────────────────────────┘      │
│                                      │
└──────────────────────────────────────┘
```

**Step-by-Step:**
```
1. Create VPC → 10.10.0.0/16
2. Create Public Subnet → 10.10.1.0/24 (AZ: ap-south-1a)
3. Create Private Subnet → 10.10.2.0/24 (AZ: ap-south-1a)
4. Create IGW → Attach to VPC
5. Public Route Table → Add 0.0.0.0/0 → IGW
6. Associate Public Subnet to Public Route Table
7. Launch Bastion EC2 in Public Subnet (with Public IP)
8. Launch Private EC2 in Private Subnet (NO Public IP)

Access Private EC2:
→ SSH to Bastion (public IP)
→ From Bastion, SSH to Private EC2 (private IP 10.10.2.x)
```

---

### ✔ Practical 9: VPC Peering + Infrastructure Project

**VPC Peering Scenario:**
```
Company has 2 environments:
→ VPC-A (Production): 10.10.0.0/16 — in ap-south-1
→ VPC-B (Development): 10.20.0.0/16 — in ap-south-1

Goal: Dev team needs to access Prod database securely
      WITHOUT going through internet!

Solution: VPC Peering
```

**Setup VPC Peering:**
```
Step 1: VPC → Peering Connections → Create
Step 2: Requester VPC → VPC-A
Step 3: Accepter VPC → VPC-B (same account)
Step 4: Create Peering Connection
Step 5: Go to VPC-B → Accept Request

Step 6: Update Route Tables (BOTH VPCs!)
  VPC-A Route Table:
    10.20.0.0/16 → pcx-xxxxx (peering connection)
  
  VPC-B Route Table:
    10.10.0.0/16 → pcx-xxxxx (peering connection)

Step 7: Update Security Groups to allow traffic from other VPC CIDR
Step 8: Test: ping 10.20.x.x from EC2 in VPC-A → Should work!
```

> ⚠️ **VPC Peering Limitations (Must Know for Interviews):**
> - No transitive peering (A↔B, B↔C doesn't mean A↔C)
> - CIDRs must NOT overlap
> - Works cross-region and cross-account
> - For transitive routing → Use AWS Transit Gateway instead

---

### ✔ NACL Deep Dive

```
NACL Rules example (allow web traffic):

INBOUND RULES:
Rule# │ Protocol │ Port  │ Source      │ Allow/Deny
100   │ TCP      │ 80    │ 0.0.0.0/0   │ ALLOW
110   │ TCP      │ 443   │ 0.0.0.0/0   │ ALLOW
120   │ TCP      │ 22    │ YOUR_IP/32  │ ALLOW
*     │ ALL      │ ALL   │ 0.0.0.0/0   │ DENY

OUTBOUND RULES:
Rule# │ Protocol │ Port       │ Destination │ Allow/Deny
100   │ TCP      │ 1024-65535 │ 0.0.0.0/0   │ ALLOW (ephemeral!)
*     │ ALL      │ ALL        │ 0.0.0.0/0   │ DENY
```

> 💡 **Interview Trap:** NACL is stateless! You must allow ephemeral ports (1024-65535) for return traffic. Security Groups don't need this — they're stateful!

---

## 🏆 Today's Challenge

```
🔥 CHALLENGE:
1. Create a custom VPC with public + private subnets
2. Launch EC2 in private subnet (no public IP)
3. SSH to it using Bastion Host method
4. Run: hostname && ip addr show
5. Screenshot showing you're inside a PRIVATE server!

Bonus: Create VPC Peering between 2 VPCs
```

---

## 📸 Screenshot Checklist

- [ ] Custom VPC created (custom CIDR, not default)
- [ ] Private EC2 with NO public IP
- [ ] SSH session inside private EC2 via Bastion
- [ ] VPC Peering connection in "Active" state (bonus)
- [ ] Route tables showing peering routes (bonus)

---


## 📊 VPC Peering vs Transit Gateway

| Feature | VPC Peering | Transit Gateway |
|---------|-------------|-----------------|
| Transitive routing | ❌ No | ✅ Yes |
| Max VPCs | Scales poorly | 5,000+ |
| Cost | Free | Paid per attachment |
| Use case | 2-3 VPCs | Enterprise multi-VPC |

---

## 🔗 Resources

- [VPC Peering Guide](https://docs.aws.amazon.com/vpc/latest/peering/)
- [Bastion Host Best Practices](https://docs.aws.amazon.com/quickstart/latest/linux-bastion/)
- [AWS Transit Gateway](https://aws.amazon.com/transit-gateway/)
> [← Day 8](../Day-08/README.md) | [Back to Main](../README.md) | [Day 10 →](../Day-10/README.md)


---

## 📣 LinkedIn Post — Copy & Customize

```
🔗 Day 9 of My 10-Day AWS Challenge! #CloudDevOpsHub

Today I did something most beginners SKIP — advanced VPC networking! 🌐

✅ Built a custom VPC with Public + Private Subnets
✅ Launched EC2 in a PRIVATE subnet (zero public internet access)
✅ Accessed it via Bastion Host (SSH jump server pattern)
✅ Configured VPC Peering between 2 isolated networks
✅ Understood NACL vs Security Group differences

The pattern I built today is used in EVERY production company:

Public Subnet → ALB / Bastion (faces internet)
Private Subnet → App servers (no internet exposure)
DB Subnet → RDS (isolated completely)

This is called defense-in-depth security 🛡️

Interview question I can now answer perfectly:
"How do you connect two VPCs in the same region?"
→ VPC Peering for direct connection
→ Transit Gateway for hub-and-spoke (multiple VPCs)

One more day to complete this challenge! 💪

#Day9 #AWSChallenge #10DayAWSChallenge #CloudDevOpsHub #AWS #VPC 
#Networking #Security #BastionHost #VPCPeering #CloudEngineering #LearningInPublic #CloudDevOpsHub #CloudDevOpsHubCommunity #VikasRatnawat #AWSCohortChallenge #10Days10AWSTasks
```

> 💬 **LinkedIn Profile Tip:**  
> Posts about **security topics** attract HR from cybersecurity and FinTech companies.  
> Add "#CloudSecurity" and "#NetworkSecurity" for extra reach!

---


---

> 🌟 **Star this repo | Fork & Customize | Tag @Vikas Ratnawat + @CloudDevOpsHub**  
> [← Day 8](../Day-08/README.md) | [🏠 Back to Main](../README.md) | [Day 10 →](../Day-10/README.md)
