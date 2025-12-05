---
title: "Các bài blogs đã dịch"
date: 2025-09-07
weight: 3
chapter: false
pre: " <b> 3. </b> "
---


###  [Blog 1 - Giới thiệu Strands Agents, mã nguồn mở của AI Agents SDK](3.1-Blog1/)
Strands Agents là SDK mã nguồn mở giúp xây dựng và triển khai AI agents theo cách tiếp cận model-driven chỉ với vài dòng code. Nó hỗ trợ các hệ thống sản xuất tại các đội ngũ AWS như Amazon Q Developer và AWS Glue, rút ngắn thời gian phát triển từ hàng tháng xuống chỉ vài ngày nhờ tận dụng LLM cho lập kế hoạch, gọi công cụ và phản ánh. Các tính năng chính bao gồm tích hợp công cụ mượt mà (hơn 20 công cụ sẵn có và hàng nghìn qua MCP), hỗ trợ nhiều mô hình (Bedrock, Claude, Llama, Ollama), và triển khai sẵn sàng sản xuất trên dịch vụ AWS với khả năng quan sát OpenTelemetry.

###  [Blog 2 - Giới thiệu tính năng kế toán dành cho AWS Parallel Computing Service](3.2-Blog2/)
Dịch vụ AWS Parallel Computing Service (PCS) giới thiệu kế toán quản lý để đơn giản hóa quản lý HPC với Slurm, cung cấp giám sát tài nguyên, giới hạn sử dụng và phân bổ dự án cho tính phí nội bộ và lập ngân sách. Điều này loại bỏ nhu cầu cơ sở dữ liệu riêng, mang lại khả năng quan sát rõ ràng về "ai đã làm gì" để báo cáo và lập kế hoạch dung lượng tốt hơn. Kích hoạt trong quá trình thiết lập cụm với Slurm 24.11+, cấu hình qua console, và tận dụng các trường hợp như phân bổ công việc, giới hạn CPU để ngăn tích trữ, báo cáo sử dụng, và phân tích lỗi. Giá cả bao gồm phí hàng giờ theo kích thước cụm cộng GB-tháng lưu trữ—bắt đầu tại console AWS PCS để vận hành HPC hiệu quả, có thể mở rộng.

###  [Blog 3 - Đơn giản hóa việc tích hợp AWS AppSync Events với Powertools for AWS Lambda](3.3-Blog3/)
Sự kiện AWS AppSync cung cấp tính năng thời gian thực qua WebSocket, và Powertools cho AWS Lambda mới nhất giới thiệu AppSyncEventsResolver cho Python, TypeScript và .NET—đơn giản hóa xử lý sự kiện với định tuyến mẫu, lọc, chuyển đổi và quản lý lỗi. Thiết lập trình xử lý cho PUBLISH và SUBSCRIBE bằng kênh trực quan và ký tự đại diện, giảm mã lặp trong khi đảm bảo ủy quyền mạnh mẽ, truy cập đầy đủ sự kiện và tổng hợp tối ưu. Điều này giúp nhà phát triển tập trung vào logic kinh doanh mà không gián đoạn Lambda, lý tưởng cho ứng dụng chat, bảng điều khiển trực tiếp hoặc hệ thống IoT—tăng cường hiệu suất trải nghiệm thời gian thực.