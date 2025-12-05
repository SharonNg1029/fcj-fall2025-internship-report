---
title: "Worklog Tuần 4"
date: 2025-09-28
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

- Chuyên sâu về Networking (VPC).
- Tìm hiểu về Resilient Architectures (Kiến trúc bền vững).

### Các công việc cần triển khai trong tuần này:

| **Ngày** | **Nhiệm vụ**                                                                                                                                               | **Bắt đầu** | **Hoàn thành** | **Tài liệu tham khảo**                                                                                                                         |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| 2        | - Tìm hiểu VPC (Virtual Private Cloud):<br>&emsp;+ CIDR Block<br>&emsp;+ Subnet (Public vs Private)<br>&emsp;+ Route Table                                 | 29/09/2025  | 30/09/2025     | [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)                                                |
| 3        | - Tìm hiểu các cổng kết nối:<br>&emsp;+ Internet Gateway (IGW)<br>&emsp;+ NAT Gateway (cho Private Subnet)<br>- **Thực hành:** Thiết kế VPC Custom từ đầu. | 30/09/2025  | 01/10/2025     | [VPC Scenarios](https://www.google.com/search?q=https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenarios.html)                           |
| 4        | - Tìm hiểu **Resilient Architectures**:<br>&emsp;+ Multi-AZ deployment<br>&emsp;+ Backup & Restore strategies<br>&emsp;+ Route 53 (DNS & Failover)         | 01/10/2025  | 01/10/2025     | [Disaster Recovery](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-workloads-on-aws.html) |
| 5        | - **Thực hành:**<br>&emsp;+ Cấu hình Route Table cho Public/Private subnet<br>&emsp;+ Test kết nối Internet từ Private Subnet qua NAT                      | 02/10/2025  | 02/10/2025     | —                                                                                                                                              |
| 6        | - Dịch 3 bài Blog kỹ thuật (chủ đề VPC, Serverless, Security).<br>- Tổng kết module Networking.                                                            | 03/10/2025  | 03/10/2025     | [AWS Blogs](https://aws.amazon.com/blogs/)                                                                                                     |

### Kết quả đạt được tuần 4:

**1. Networking (VPC):**

- Hiểu sâu về luồng đi của gói tin trong mạng AWS.
- Phân biệt rõ ràng vai trò của Security Group (cấp Instance) và NACL (cấp Subnet).
- Tự thiết kế được hệ thống mạng an toàn: Web Server ở Public Subnet, DB Server ở Private Subnet.

**2. Resilient Architectures:**

- Nắm vững khái niệm Multi-AZ để đảm bảo tính sẵn sàng cao (High Availability).
- Biết cách sử dụng NAT Gateway để bảo mật các instance backend.
- Hoàn thành việc dịch tài liệu chuyên ngành để củng cố kiến thức.

---
