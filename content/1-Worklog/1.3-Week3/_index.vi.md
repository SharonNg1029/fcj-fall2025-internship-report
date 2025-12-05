---
title: "Worklog Tuần 3"
date: 2025-09-07
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

- Nắm vững dịch vụ Compute (EC2).
- Hiểu về High-Performing Architectures (Scaling, Load Balancing).

### Các công việc cần triển khai trong tuần này:

| **Ngày** | **Nhiệm vụ**                                                                                                                                                      | **Bắt đầu** | **Hoàn thành** | **Tài liệu tham khảo**                                                                         |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | -------------- | ---------------------------------------------------------------------------------------------- |
| 2        | - Tìm hiểu EC2 cơ bản:<br>&emsp;+ Instance types (General, Compute, Memory optimized)<br>&emsp;+ AMI (Amazon Machine Image)<br>&emsp;+ Key Pair                   | 22/09/2025  | 22/09/2025     | [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)                               |
| 3        | - Tìm hiểu EBS (Elastic Block Store):<br>&emsp;+ Các loại Volume (gp2, gp3, io1)<br>&emsp;+ Snapshot & Backup<br>- Tìm hiểu Security Group (Firewall ảo).         | 23/09/2025  | 24/09/2025     | [EBS Volume Types](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html)     |
| 4        | - Tìm hiểu **High-Performing Architectures**:<br>&emsp;+ Elastic Load Balancing (ELB)<br>&emsp;+ EC2 Auto Scaling<br>&emsp;+ Vertical vs Horizontal Scaling       | 24/09/2025  | 24/09/2025     | [ELB Product Page](https://aws.amazon.com/elasticloadbalancing/)                               |
| 5        | - **Thực hành Lab:**<br>&emsp;+ Khởi tạo EC2 Linux<br>&emsp;+ Remote SSH vào EC2<br>&emsp;+ Cài đặt Web Server (Apache/Nginx)<br>&emsp;+ Tạo ALB phân tải traffic | 25/09/2025  | 25/09/2025     | [EC2 Getting Started](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) |
| 6        | - Ôn tập lại kiến thức EC2 và EBS đã học trong tuần.<br>- Tổng hợp các lỗi thường gặp khi SSH vào instance.                                                       | 26/09/2025  | 26/09/2025     | —                                                                                              |

### Kết quả đạt được tuần 3:

**1. Amazon EC2 & Compute:**

- Phân biệt được các dòng Instance Type (T, M, C, R) để chọn cấu hình phù hợp hiệu năng/giá tiền.
- Hiểu vòng đời của một Instance (Pending, Running, Stopping, Terminated).
- Quản lý được EBS Volume: Tạo, gắn (attach), và backup snapshot.

**2. High-Performance & Scalability:**

- Nắm được cơ chế hoạt động của Load Balancer trong việc phân phối tải.
- Hiểu cách Auto Scaling Group giúp hệ thống tự động mở rộng khi tải cao (High-Performing).
- Thực hành thành công việc SSH vào server và cài đặt ứng dụng web cơ bản.

---
