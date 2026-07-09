---
title : "Cấu hình Google API Key"
date : "2026-07-09"
weight : 11
chapter : false
pre : " <b> Step 11 </b> "
---
# Step 11: Cấu Hình Google API Key

---

## Tổng Quan

Lambda cần Google API key để gọi Gemini. API key được lưu trong AWS Secrets Manager để bảo mật.

---

## Cách 1: Push Secret Script (Khuyến nghị)

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO
.\scripts\push-secret.ps1
```

Script này đọc `backend/.env` và upload lên Secrets Manager.

---

## Cách 2: Thủ Công qua AWS CLI

```powershell
aws secretsmanager put-secret-value `
  --secret-id cloud-nexus/google-api-key `
  --secret-string "<YOUR_GOOGLE_API_KEY>"
```

---

## Cách 3: Cập nhật Lambda Environment Variable

```powershell
aws lambda update-function-configuration --function-name CloudNexus-BackendHandler-<SUFFIX> `
  --environment "Variables={GOOGLE_API_KEY=<YOUR_KEY>}"
```

---

## Xác Minh Cấu Hình

```powershell
# Kiểm tra secret tồn tại
aws secretsmanager describe-secret --secret-id cloud-nexus/google-api-key

# Hoặc verify Lambda environment
aws lambda get-function-configuration --function-name CloudNexus-BackendHandler-<SUFFIX> --query "Environment.Variables"
```

---


![Screenshot](/images/5-Workshop/step-11.png)




