---
title : "Ý Tưởng & Mục Tiêu"
date : "2026-07-09"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---


---

## Bối Cảnh & Bài Toán

**Cloud Nexus** là nền tảng mô phỏng và phân tích bảo mật mạng (Threat Modeling Platform) được thiết kế cho các chuyên gia an ninh mạng và kiến trúc sư hạ tầng. Nền tảng cung cấp môi trường tương tác trực quan nơi người dùng có thể thiết kế topology mạng bằng cách kéo-thả các thành phần, sau đó sử dụng AI (Google Gemini) để tự động phân tích lỗ hổng bảo mật, mô phỏng các đường đi tấn công tiềm năng, và nhận các khuyến nghị phòng thủ khả thi.

**Đối tượng người dùng:**
- Chuyên gia bảo mật cần đánh giá lỗ hổng tự động
- Kiến trúc sư đám mây thiết kế topology mạng an toàn
- Sinh viên học về an ninh mạng tìm hiểu mô hình tấn công/phòng thủ
- Chuyên gia kiểm thử xâm nhập lập kế hoạch chiến lược

---

## Vấn Đề Được Giải Quyết

| Vấn đề | Giải pháp |
|--------|------------|
| Phát hiện lỗ hổng mạng thủ công tốn thời gian | AI tự động quét topology bằng Gemini |
| Khó hình dung và hiểu đường đi tấn công | Mô phỏng hoạt hình trên canvas ReactFlow |
| Thiết lập môi trường test phức tạp | Serverless trên AWS, không cần quản lý server |
| Thiếu công cụ đánh giá phòng thủ chuẩn | So sánh attack path trước/sau khi thêm defense |
| Thiếu công cụ học tập tương tác | Giao diện terminal thời gian thực với lệnh AI |

---

## Mục Tiêu Dự Án

| Tiêu chí | Mô tả |
|----------|--------|
| **Trình chỉnh sửa trực quan** | ReactFlow-based với kéo-thả xây dựng topology |
| **Tích hợp AI** | Google Gemini tạo topology, phân tích lỗ hổng, mô phỏng tấn công |
| **Phản hồi thời gian thực** | Giao diện terminal cho lệnh AI và kết quả |
| **Serverless Backend** | Lambda + API Gateway (FastAPI + Mangum) |
| **Hạ tầng như Code** | AWS CDK — deploy/destroy bằng lệnh đơn |
| **Lưu trữ API Key an toàn** | AWS Secrets Manager (không hardcode) |
| **CDN toàn cầu** | CloudFront truy cập low-latency toàn cầu |

---

## Tiêu Chí Đánh Giá Thành Công

1. Frontend load từ CloudFront CDN: `https://d3rs3evkmfvesp.cloudfront.net/`
2. API Gateway trả về `{"status":"ok"}` tại `/api/health`
3. Lambda gọi Google Gemini và trả về topology JSON hợp lệ
4. Mô phỏng tấn công hiển thị đường đi tấn công hoạt hình trên canvas
5. Toàn bộ hạ tầng deploy bằng `cdk deploy` và xóa bằng `cdk destroy`

---

## Phù Hợp với Best Practices Kiến Trúc Đám Mây

- **Serverless-first:** Lambda, API Gateway, S3, CloudFront
- **Hạ tầng như Code:** AWS CDK (Python)
- **Bảo mật:** Secrets Manager, IAM roles, không hardcode credentials
- **Tối ưu chi phí:** Free tier cho service cốt lõi, pay-per-use
- **Phân phối toàn cầu:** CloudFront CDN với HTTPS

