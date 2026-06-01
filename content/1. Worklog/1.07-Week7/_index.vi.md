---
title: "WEEK 7 WORKLOG"
date: "2026-05-29"
weight: 7
chapter: false
pre: " <b> 1.7 </b> "
---

### **Week 7 Objectives**

* Tìm hiểu và triển khai kiến trúc Serverless (phi máy chủ) bằng **AWS Lambda** và **API Gateway**.
* Viết template CloudFormation để tự động hóa việc triển khai hàm Lambda và endpoint API.
* Tìm hiểu tính năng nâng cao của CloudFormation: **StackSets**, để triển khai tài nguyên (S3) đồng bộ trên nhiều tài khoản và region.
* Bắt đầu tìm hiểu một công cụ IaC mới: **Terraform** (HCL syntax).
* Thực hành các lệnh cơ bản của Terraform (`init`, `plan`, `apply`) và học cách quản lý cấu hình bằng **biến (variables)** và file `.tfvars`.

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Thứ Hai) | **Tìm hiểu Serverless**: Học về AWS Lambda, viết template CloudFormation (tạo IAM Role, Lambda Function) và tích hợp API Gateway. | 29/05/2026 | 29/05/2026 | |
| 2 (Thứ Ba) | **CloudFormation StackSets**: Tìm hiểu StackSets, viết template (tạo S3) và deploy StackSet ra nhiều tài khoản/region. | 01/06/2026 | 01/06/2026 | |
| 3 (Thứ Tư) | **Bắt đầu với Terraform**: Viết file `main.tf` đầu tiên (khai báo provider "aws", resource "aws_s3_bucket"). Chạy `terraform init` và `plan`. | 02/06/2026 | 02/06/2026 | |
| 4 (Thứ Năm) | **Hoàn thiện & Dọn dẹp (CloudFormation)**: Triển khai, kiểm tra (SSH/HTTP) một stack hoàn chỉnh (VPC+EC2) và học cách xóa stack (`delete-stack`). | 03/06/2026 | 03/06/2026 | |
| 5 (Thứ Sáu) | **Hoàn thành Terraform**: Chạy `terraform apply` để tạo S3 bucket. Học cách dùng **variables** và file `.tfvars` để quản lý tên bucket. | 04/06/2026 | 04/06/2026 | |

---

### **Week 7 Achievements**

* Nắm vững khái niệm về kiến trúc **Serverless** và lợi ích của nó.
* Triển khai thành công hàm **AWS Lambda** (Node.js) và cấu hình **IAM Role** (cho phép ghi log) hoàn toàn bằng template **CloudFormation**.
* Tích hợp thành công Lambda với **API Gateway** (sử dụng `AWS_PROXY`) để tạo một endpoint API có thể truy cập công khai.
* Nắm vững và triển khai thành công **CloudFormation StackSets**, hiểu rõ cách dùng nó để quản lý và đồng bộ tài nguyên trên nhiều tài khoản AWS.
* Bắt đầu học và làm quen với một công cụ IaC phổ biến khác là **Terraform**.
* Viết thành công file cấu hình Terraform (HCL) đầu tiên để tạo tài nguyên `aws_s3_bucket`.
* Nắm vững vòng đời cơ bản của Terraform:
    * `terraform init`: Để khởi tạo provider.
    * `terraform plan`: Để xem trước các thay đổi.
    * `terraform apply`: Để áp dụng và tạo tài nguyên thực tế.
* Biết cách tham số hóa cấu hình Terraform bằng cách sử dụng **variables** (khai báo trong `.tf`) và quản lý giá trị biến (trong `.tfvars`).
* Ôn lại quy trình dọn dẹp tài nguyên bằng AWS CLI (`aws cloudformation delete-stack`).