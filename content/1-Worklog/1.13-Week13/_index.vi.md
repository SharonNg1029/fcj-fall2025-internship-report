---
title: "Worklog Tuần 13"
date: 2025-12-01
weight: 13
chapter: false
pre: " <b> 1.13. </b> "
---

### Mục tiêu tuần 13:

- Tích hợp Backend (API Gateway).
- Triển khai CloudFront + WAF và Route53 (Custom Domain).

### Các công việc cần triển khai trong tuần này:

| **Ngày** | **Nhiệm vụ**                                                                                                                                                                                                                | **Bắt đầu** | **Hoàn thành** | **Tài liệu tham khảo**                                                                                                                                                                                               |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2        | - Kết nối API Gateway (REST).<br>- Xử lý lỗi CORS.                                                                                                                                                                          | 02/12/2025  | 02/12/2025     | [CORS in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html) <br> [Axios Documentation](https://axios-http.com/docs/intro)                                                   |
| 3        | - Tích hợp CRUD Sinh viên:<br>&emsp;+ Create (Thêm sinh viên mới)<br>&emsp;+ Read (Lấy danh sách/chi tiết)<br>&emsp;+ Update (Cập nhật thông tin)<br>&emsp;+ Delete (Xóa sinh viên)<br>- Xử lý state loading/error trên UI. | 03/12/2025  | 03/12/2025     | [React Query](https://tanstack.com/query/latest) <br> [Error Handling Best Practices](https://kentcdodds.com/blog/use-react-error-boundary-to-handle-errors-in-react)                                                |
| 4        | Triển khai **CloudFront + WAF**.<br>- Tạo WAF Web ACL với các rules bảo mật.<br>- Tạo CloudFront Distribution.<br>- Thêm Origin cho API Gateway.                                                           | 04/12/2025  | 04/12/2025     | [AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/what-is-aws-waf.html) <br> [CloudFront Distribution](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-working-with.html) |
| 5        | - Cấu hình **Route53** (Custom Domain).<br>- Tạo DNS Records cho domain.<br>- Cấu hình SSL Certificate với ACM (AWS Certificate Manager).                                                                                   | 05/12/2025  | 05/12/2025     | [Route 53 DNS](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) <br> [ACM Certificate](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html)                                    |
| 6        | - Refactor code Frontend.<br>- Optimize Performance (lazy loading, memoization).<br>- Viết documentation cho code.                                                                                                          | 06/12/2025  | 06/12/2025     | [React Performance](https://react.dev/learn/render-and-commit) <br> [Code Documentation](https://jsdoc.app/)                                                                                                         |

### Kết quả đạt được tuần 13:

**1. Kết nối API thành công:**

- Kết nối được Frontend với API Gateway do Backend team làm.
- Xử lý được lỗi CORS bằng cách làm việc với Backend team.
- Xây dựng API service với Axios, tự động thêm JWT Token vào header.
- Làm được loading spinner và error notification khi call API.

**2. Hoàn thành giao diện CRUD Sinh viên:**

- Làm form thêm/sửa sinh viên với validation.
- Hiển thị danh sách sinh viên với bảng có phân trang và tìm kiếm.
- Làm được xóa sinh viên với modal xác nhận.
- Dùng React Query để quản lý data và tự động refresh sau khi thay đổi.

**3. Triển khai CloudFront + WAF:**

- Tạo WAF Web ACL với các rules bảo mật cơ bản (SQL Injection, XSS, Rate Limiting).
- Tạo CloudFront Distribution để phân phối nội dung.
- Thêm Origin cho API Gateway vào CloudFront.
- Cấu hình Cache Behavior phù hợp cho API requests.

**4. Cấu hình Route53 và SSL:**

- Tạo Hosted Zone cho custom domain.
- Cấu hình DNS Records (A Record, CNAME) trỏ về CloudFront.
- Request SSL Certificate từ ACM cho domain.
- Attach certificate vào CloudFront Distribution.
- Test truy cập qua custom domain với HTTPS thành công.

**5. Học được:**

- Cách tích hợp Frontend với REST API.
- Hiểu về CDN và cách CloudFront hoạt động.
- Cấu hình WAF để bảo vệ ứng dụng khỏi các tấn công phổ biến.
- Quản lý DNS với Route53 và cấu hình SSL Certificate với ACM.
- Làm việc nhóm với Backend team để xử lý vấn đề CORS và test End-to-End.

---
