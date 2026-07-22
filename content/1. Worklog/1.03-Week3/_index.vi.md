---
title: "WEEK 3 WORKLOG"
date: "2026-05-01"
weight: 3
chapter: false
pre: " <b> 1.3 </b> "
---

### **Week 3 Objectives**

* Hiểu và triển khai dịch vụ lưu trữ hybrid **AWS Storage Gateway** (cài đặt, kích hoạt).
* Hiểu và triển khai dịch vụ bảo mật **AWS WAF** (Web Application Firewall) để bảo vệ ứng dụng web.
* Nắm vững và thực hành các phương pháp quản lý tài nguyên AWS bằng cách sử dụng **Tags** và **Resource Groups**.
* Tìm hiểu lý thuyết về giám sát lưu lượng mạng bằng **VPC Flow Logs** và quản lý, tối ưu hóa chi phí với **AWS Budgets**.
* Tìm hiểu về quy trình và các bước cơ bản để chuẩn bị cho một buổi Workshop kỹ thuật.

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Thứ Hai) | **Triển khai AWS Storage Gateway**: Chuẩn bị môi trường (S3 Bucket, EC2) và tiến hành cài đặt, kích hoạt (activate) Storage Gateway. | 04/05/2026 | 04/05/2026 | |
| 2 (Thứ Ba) | **Lab 26 – AWS WAF**: Triển khai ứng dụng web trên S3 và cấu hình AWS WAF với Web ACLs, sử dụng AWS Managed Rules để bảo vệ. | 05/05/2026 | 05/05/2026 | https://000026.awsstudygroup.com/ |
| 3 (Thứ Tư) | **Lab 27 – Manage Resources**: Thực hành tạo, sửa, xóa Tags trên EC2 instance. Sử dụng Tag Editor và Resource Groups để quản lý tài nguyên. | 06/05/2026 | 06/05/2026 | https://000027.awsstudygroup.com/ |
| 4 (Thứ Năm) | **Giám sát & Quản lý Chi phí**: Tìm hiểu lý thuyết về giám sát qua VPC Flow Logs và AWS Budgets. | 07/05/2026 | 07/05/2026 | https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-records-examples.html |

---

### **Week 3 Achievements**

* **Triển khai Storage Gateway**: Triển khai và kích hoạt thành công **AWS Storage Gateway** trên EC2, hiểu rõ vai trò của nó như một cầu nối lưu trữ hybrid cloud kết nối hạ tầng on-premises với S3.
* **Bảo mật ứng dụng với AWS WAF**: Triển khai ứng dụng web thành công và cấu hình **Web ACLs** áp dụng các bộ quy tắc quản lý sẵn (**AWS Managed Rules**) để phòng chống các mối đe dọa web phổ biến (như OWASP Top 10).
* **Quản lý Tài nguyên AWS**: Nắm vững phương pháp phân loại và quản lý tài nguyên hệ thống thông qua việc gắn **Tags**, sử dụng **Tag Editor** và nhóm tài nguyên bằng **Resource Groups**.
* **Nghiên cứu VPC Flow Logs & AWS Budgets**:
    * Hiểu cấu trúc log và cơ chế theo dõi, phân tích lưu lượng IP inbound/outbound trong VPC thông qua **VPC Flow Logs**.
    * Nắm vững cách thiết lập **AWS Budgets** để theo dõi chi phí sử dụng dịch vụ và cấu hình cảnh báo tự động khi vượt ngưỡng ngân sách.
* **Kỹ năng Xử lý Sự cố & Chuẩn bị Workshop**:
    * Tiếp tục củng cố kỹ năng troubleshooting, giải quyết các sự cố về **Security Group** (mở port 443 outbound cho Storage Gateway).
    * Nắm rõ quy trình và các bước cơ bản để chuẩn bị, xây dựng nội dung cho một buổi workshop kỹ thuật.