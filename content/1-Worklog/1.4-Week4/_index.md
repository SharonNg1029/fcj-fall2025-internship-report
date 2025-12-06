---
title: "Week 4 Worklog"
date: 2025-09-28
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

- Deep dive into Networking (VPC).
- Learn about Resilient Architectures.

### Tasks to be carried out this week:

| **Day** | **Tasks**                                                                                                                                      | **Start**  | **Finish** | **References**                                                                                          |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ------------------------------------------------------------------------------------------------------- |
| 2       | - Learn VPC (Virtual Private Cloud):<br>&emsp;+ CIDR Block<br>&emsp;+ Subnet (Public vs Private)<br>&emsp;+ Route Table                        | 29/09/2025 | 30/09/2025 | [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)         |
| 3       | - Learn connectivity components:<br>&emsp;+ Internet Gateway (IGW)<br>&emsp;+ NAT Gateway<br>- **Practice:** Design a Custom VPC from scratch. | 30/09/2025 | 01/10/2025 | [VPC Scenarios](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenarios.html)                    |
| 4       | - Learn **Resilient Architectures**:<br>&emsp;+ Multi-AZ deployment<br>&emsp;+ Backup & Restore<br>&emsp;+ Route 53 (DNS & Failover)           | 01/10/2025 | 01/10/2025 | [Disaster Recovery](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/) |
| 5       | - **Practice:**<br>&emsp;+ Configure Route Table for Public/Private subnet<br>&emsp;+ Test Internet from Private Subnet via NAT                | 02/10/2025 | 02/10/2025 | —                                                                                                       |
| 6       | - Translate 3 technical blogs (VPC, Serverless, Security).<br>- Summarize Networking module.                                                   | 03/10/2025 | 03/10/2025 | [AWS Blogs](https://aws.amazon.com/blogs/)                                                              |

### Week 4 Achievements:

**1. Networking (VPC):**

- Understood packet flow inside AWS networking.
- Differentiated Security Group (instance-level) vs NACL (subnet-level).
- Designed a secure network architecture: Web Server in Public Subnet, DB in Private Subnet.

**2. Resilient Architectures:**

- Mastered Multi-AZ concept for High Availability.
- Understood NAT Gateway usage to secure backend instances.
- Completed translation of technical documents for deeper understanding.

---
