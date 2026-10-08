---
title: "Đề Xuất Dự Án"
date: "2026-07-04"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# Cloud Nexus

---

## 1. Tổng Quan Dự Án

**Cloud Nexus** là nền tảng Mô hình hóa Mối đe dọa (Threat Modeling) dành cho các chuyên gia an ninh mạng và kiến trúc sư hạ tầng. Hệ thống cho phép người dùng thiết kế topology mạng trực quan thông qua giao diện kéo-thả, sau đó tận dụng AI (Google Gemini 2.0 Flash) để tự động:

- Tạo topology mạng từ mô tả văn bản
- Quét lỗ hổng bảo mật và xác định kịch bản tấn công
- Mô phỏng đường dẫn tấn công với hình ảnh hóa từng bước
- Đề xuất biện pháp phòng thủ để giảm thiểu rủi ro

### Công Nghệ

| Thành Phần | Công Nghệ |
|-----------|------------|
| Frontend | React + Vite + ReactFlow + Tailwind CSS |
| Backend | FastAPI trên AWS Lambda + API Gateway |
| Engine AI | Google Gemini API (Gemini 2.0 Flash) |
| Hạ Tầng | AWS CDK (TypeScript), Python 3.12 ARM64 |
| Dịch Vụ AWS | S3, API Gateway, Lambda, Cognito, DynamoDB, SQS, SNS, Step Functions, Secrets Manager |

---

## 2. Mục Tiêu

### Mục Tiêu Chung
Xây dựng nền tảng serverless trên AWS tự động hóa đánh giá bảo mật mạng — từ thiết kế topology đến phát hiện lỗ hổng và mô phỏng tấn công.

### Mục Tiêu Cụ Thể
- **REST API + Dashboard Web:** Triển khai backend FastAPI trên Lambda với API Gateway, frontend React trên S3 + CloudFront
- **Tích Hợp AI:** Google Gemini tạo topology mạng, phân tích lỗ hổng, mô phỏng đường dẫn tấn công với các kỹ thuật hacker thực tế
- **Hình Ảnh Hóa Tấn Công:** Mô phỏng đường dẫn tấn công được animate trên canvas ReactFlow hiển thị từng bước xâm nhập
- **Đề Xuất Phòng Thủ:** Gợi ý phòng thủ từ AI với khả năng chạy lại mô phỏng sau khi áp dụng biện pháp phòng thủ
- **Kiến Trúc Serverless:** Toàn bộ backend chạy trên Lambda + API Gateway không cần quản lý server
- **Hạ Tầng Triển Khai:** AWS CDK cho phép triển khai và hủy bỏ chỉ bằng một lệnh


---

## 3. Bài Toán

| Vấn Đề | Giải Pháp |
|---------|----------|
| Phát hiện lỗ hổng mạng thủ công tốn thời gian và đòi hỏi chuyên môn sâu | AI tự động quét topology và xác định kịch bản tấn công |
| Khó hình dung và hiểu đường dẫn tấn công | Mô phỏng trực quan từng bước trên canvas ReactFlow |
| Cài đặt môi trường thử nghiệm phức tạp đòi hỏi nhiều hạ tầng | Serverless trên AWS Lambda, không cần cấp phát server |
| Không có cách đánh giá hiệu quả phòng thủ trước khi triển khai | So sánh đường dẫn tấn công trước/sau khi áp dụng biện pháp phòng thủ |
| Các công cụ truyền thống phức tạp và đòi hỏi đào tạo nhiều | Giao diện kéo-thả trực quan cho thiết kế topology |

### Đối Tượng Người Dùng
- Chuyên gia Phân tích Bảo mật thực hiện đánh giá bảo mật mạng
- Kiến trúc sư Đám Mây thiết kế hạ tầng đám mây bảo mật
- Sinh viên An Ninh Mạng học các khái niệm mô hình hóa mối đe dọa
- Chuyên gia Red Team lập kế hoạch kiểm thử xâm nhập

---

## 4. Kiến Trúc Hệ Thống

![Kiến Trúc Hệ Thống](/images/2-Proposal/archi.png)

---

## 5. Lịch Trình (1/6 → 8/7)

| Giai Đoạn | Nội Dung | Thời Gian |
|-------|---------|----------|
| **Giai Đoạn 1: Nền Tảng** | Thiết lập môi trường, cấu hình tài khoản AWS, IAM policies, khung dự án | 1/6 → 4/6 |
| **Giai Đoạn 2: Phát Triển Frontend** | Xây dựng UI React với canvas ReactFlow, component nodes, giao diện terminal, hệ thống theme | 5/6 → 11/6 |
| **Giai Đoạn 3: Phát Triển Backend** | Triển khai FastAPI, tích hợp AI service với Gemini, các endpoint API cho tạo/quét/mô phỏng | 12/6 → 18/6 |
| **Giai Đoạn 4: Hạ Tầng** | CDK stacks cho Lambda, API Gateway, S3, CloudFront, Cognito, Secrets Manager | 19/6 → 24/6 |
| **Giai Đoạn 5: Tích Hợp & Triển Khai** | Lambda Layer cho dependencies, triển khai stacks, cấu hình API key qua Secrets Manager | 25/6 → 30/6 |
| **Giai Đoạn 6: Kiểm Thử & Hoàn Thiện** | Kiểm thử end-to-end, sửa lỗi, cải thiện trải nghiệm người dùng, hoàn thiện mô phỏng phòng thủ | 1/7 → 6/7 |
| **Giai Đoạn 7: Tài Liệu & Hoàn Tất** | Tài liệu dự án, README, tài liệu trình bày, dọn dẹp | 7/7 → 8/7 |

---

## 6. Ngân Sách

### Chi Phí Hàng Tháng AWS (Ước Tính)

| Dịch Vụ | Free Tier | Chi Phí |
|---------|-----------|------|
| S3 Static Hosting | 5 GB | ~$0.01 |
| API Gateway | 1M calls/tháng | $0 (free tier) |
| Lambda | 1M requests/tháng | $0 (free tier) |
| Cognito | 50K MAUs | $0 (free tier) |
| DynamoDB | 25 GB | $0 (free tier) |
| SQS | 1M messages/tháng | $0 (free tier) |
| SNS | 100K notifications | $0 (free tier) |
| Step Functions | 4K state transitions | $0 (free tier) |
| Secrets Manager | 1 secret | ~$0.40 |
| CloudFront | 1TB transfer | $0 (free tier) |
| **Tổng** | | **~$0.41/tháng** |

### Chi Phí Google Gemini API
- Gemini 2.0 Flash: Miễn phí với giới hạn sử dụng rộng rãi
- Chi phí phát sinh chỉ khi vượt quá giới hạn free tier

### Tóm Tắt
Dự án hoạt động hoàn toàn trong AWS Free Tier + Google Gemini Free Tier, dẫn đến chi phí vận hành gần như bằng không cho phát triển và kiểm thử.

---

## 7. Đánh Giá Rủi Ro

| Rủi Ro | Mô Tả | Mức Độ | Giảm Thiểu |
|------|-------------|----------|------------|
| **Lộ API Key** | Google API key vô tình commit lên Git | Cao | Secrets Manager lưu trữ khóa an toàn; .gitignore ngăn commit; Lambda đọc tại runtime |
| **Chi Phí Phát Sinh** | Lambda được gọi quá nhiều hoặc bị tấn công | Trung Bình | CloudWatch Alarms, AWS Budget Alerts, giới hạn Lambda concurrency |
| **Lỗi Response AI** | Gemini trả về JSON không hợp lệ hoặc định dạng bất ngờ | Trung Bình | Retry logic (2 lần) với thông báo lỗi fallback |
| **Lambda Timeout** | Response AI quá chậm (>30s) | Thấp | Tăng cấu hình timeout; xử lý async qua SQS |
| **Lỗi Triển Khai CDK** | Phiên bản AWS CDK không khớp giữa các môi trường | Thấp | Pinned CDK version; kiểm tra validation trước triển khai |
| **Vấn Đề CORS** | Browser chặn các request API cross-origin | Thấp | CORS middleware pre-configured với các origins được cho phép |
| **Mất Dữ Liệu** | DynamoDB bị xóa nhầm | Trung Bình | Point-in-Time Recovery được bật; backup định kỳ |
| **Độ Chính Xác Mô Phỏng** | Đường dẫn tấn công do AI tạo có thể không phản ánh kịch bản thực tế | Trung Bình | Thông báo rõ ràng mô phỏng mang tính giáo dục; khuyến nghị review bởi chuyên gia |

---


