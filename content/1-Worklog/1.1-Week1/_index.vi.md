---
title: "Worklog Tuần 1"
date: 2025-09-07
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

- Onboarding và thiết lập môi trường làm việc.
- Nắm vững kiến thức nền tảng về Cloud Computing và AWS Global Infrastructure.
- Làm quen với IAM và bảo mật tài khoản cơ bản.

### Các công việc cần triển khai trong tuần này:

| **Ngày** | **Nhiệm vụ**                                                                                                                                                                                                                                                   | **Bắt đầu** | **Hoàn thành** | **Tài liệu tham khảo**                                                                                        |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | -------------- | ------------------------------------------------------------------------------------------------------------- |
| 2        | - Làm quen với các thành viên FCJ.<br>- Đọc và lưu ý các nội quy, quy định đào tạo.<br>- Tìm hiểu AWS và các nhóm dịch vụ chính:<br>&emsp;+ Compute (EC2, Lambda)<br>&emsp;+ Storage (S3, EBS)<br>&emsp;+ Networking (VPC)<br>&emsp;+ Database (RDS, DynamoDB) | 08/09/2025  | 08/09/2025     | [AWS Cloud Concepts](https://aws.amazon.com/what-is-cloud-computing/)                                         |
| 3        | - Tạo AWS Free Tier account.<br>- Cấu hình MFA cho Root User.<br>- Thiết lập Billing Alarm (ngưỡng $5).                                                                                                                                                        | 09/09/2025  | 10/09/2025     | [Create AWS Account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/) |
| 4        | - Tìm hiểu AWS Console & AWS CLI.<br>- **Thực hành:**<br>&emsp;+ Cài đặt AWS CLI v2<br>&emsp;+ Cấu hình aws configure<br>&emsp;+ Thử các lệnh CLI cơ bản                                                                                                       | 10/09/2025  | 11/09/2025     | [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)              |
| 5        | - Tìm hiểu **Secure Architectures** (Phần 1 - IAM):<br>&emsp;+ IAM User, Group, Role<br>&emsp;+ Policy & Permissions<br>&emsp;+ Principle of Least Privilege<br>- **Thực hành:** Tạo User Admin và phân quyền.                                                 | 11/09/2025  | 12/09/2025     | [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)                    |
| 6        | - Tìm hiểu hạ tầng toàn cầu AWS:<br>&emsp;+ Region<br>&emsp;+ Availability Zone (AZ)<br>&emsp;+ Edge Location                                                                                                                                                  | 12/09/2025  | 13/09/2025     | [Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)                              |

### Kết quả đạt được tuần 1:

**1. Kiến thức nền tảng AWS:**

- Hiểu khái niệm Cloud Computing và các mô hình IaaS, PaaS, SaaS.
- Nắm được các nhóm dịch vụ cốt lõi: Compute, Storage, Networking, Database.
- Hiểu cấu trúc hạ tầng toàn cầu (Region, AZ) để thiết kế hệ thống bền vững (Resilient).

**2. Quản lý tài khoản & Bảo mật (IAM):**

- Đã tạo và cấu hình AWS Free Tier account thành công.
- Kích hoạt MFA bảo vệ tài khoản Root.
- Thiết lập cảnh báo chi phí (Billing Alarm) để kiểm soát ngân sách.
- Hiểu và áp dụng được các khái niệm IAM: User, Group, Policy.

**3. Kỹ năng công cụ (CLI):**

- Cài đặt và cấu hình thành công AWS CLI trên máy cá nhân.
- Nắm được các thông tin cấu hình: Access Key, Secret Key, Region mặc định.
- Thực hiện được các lệnh cơ bản: aws s3 ls, aws ec2 describe-instances, aws iam list-users.

---
