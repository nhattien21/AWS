---
title: "WEEK 11 WORKLOG"
date: "2026-06-26"
weight: 11
chapter: false
pre: " <b> 1.11 </b> "
---

# **WEEK 11 WORKLOG**

### **Week 11 Objectives**

* Tìm hiểu và thực hành các tính năng nâng cao của **Kubernetes** (K8s) bao gồm quản lý tài nguyên, auto-scaling, và bảo mật.
* Cấu hình thành công **Horizontal Pod Autoscaler (HPA)** để tự động co giãn Pod.
* Cấu hình thành công các chính sách bảo mật K8s như **Network Policies** và **RBAC**.
* Nghiên cứu về các hệ thống giám sát (Monitoring) và quản lý log (Logging) cho K8s (Prometheus, Grafana, ELK, Fluentd).
* Tìm hiểu và cấu hình tính năng nâng cao của **AWS Application Load Balancer (ALB)**, cụ thể là **Content-based Routing**.
* Nghiên cứu về hỗ trợ **HTTP/2** trên ALB.

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Thứ Hai) | **K8s Quản lý Tài nguyên & Scaling**: Học về Resource Quotas, Limit Ranges. Thực hành cấu hình **Horizontal Pod Autoscaler (HPA)**. | 29/06/2026 | 29/06/2026 | |
| 2 (Thứ Ba) | - Sửa lỗi CORS preflight (`OPTIONS`) request từ CloudFront bị API Gateway trả `400 Bad Request`<br>- Loại bỏ CORS config khỏi API Gateway để tránh xử lý trùng với FastAPI `CORSMiddleware`<br>- Bổ sung handler `@app.options` trong FastAPI xử lý preflight<br>- Cập nhật `allow_origins` trong `CORSMiddleware` với CloudFront URL và localhost<br>- Thêm Lambda Layer mới với đầy đủ dependencies<br>- Rebuild và redeploy Lambda function | 30/06/2026 | 30/06/2026 | https://vitejs.dev/guide/build<br>https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide |
| 3 (Thứ Tư) | **K8s Security (Access)**: Thực hành cấu hình **RBAC** (Roles, RoleBindings) để quản lý quyền truy cập. Nghiên cứu các tool quản lý log (Fluentd, ELK). | 30/06/2026 | 30/06/2026 | |
| 4 (Thứ Năm) | **Tìm hiểu ALB Content-based Routing**: Nghiên cứu và viết tài liệu chi tiết về cách ALB định tuyến lưu lượng dựa trên nội dung (path, header). | 01/07/2026 | 01/07/2026 | |
| 5 (Thứ Sáu) | - Tích hợp và kiểm tra toàn bộ luồng chức năng: AI generate topology, topology validation, simulation scan, simulation run, simulation with defense<br>- Test thực tế từ giao diện frontend trên CloudFront: tất cả chức năng hoạt động ổn định<br>- Fix endpoint `simulation/with-defense` đảm bảo trả đầy đủ `attack_steps` và `defense_mechanisms`<br>- Tối ưu Lambda function name và CloudFormation outputs | 02/07/2026 | 02/07/2026 | https://docs.aws.amazon.com/lambda/latest/dg/nodejs-package.html |
 
---

### **Week 11 Achievements**

* **Khắc phục lỗi CORS Preflight & Xử lý API Gateway với CloudFront**:
    * Loại bỏ cấu hình CORS trùng lặp trên API Gateway để đảm bảo FastAPI xử lý tập trung.
* **Tối ưu Deployment & Lambda Infrastructure**
* **Hoàn thiện FastAPI Backend & Kiểm tra API Endpoints**
* **Tích hợp & Kiểm thử Toàn bộ Luồng Chức năng (E2E Test)**
