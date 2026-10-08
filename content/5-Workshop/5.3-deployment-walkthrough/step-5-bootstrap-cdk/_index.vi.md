---
title : "Bootstrap CDK"
date : "2026-07-09"
weight : 5
chapter : false
pre : " <b> Step 5 </b> "
---


---

## Mục Đích

Tạo S3 bucket `cdk-hnb659fds-assets-<ACCOUNT>-<REGION>` để lưu template và assets khi deploy.

---

## Các Lệnh

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO\infrastructure
cdk bootstrap aws://<ACCOUNT_ID>/us-east-1
```

---

## Kết Quả Mong Đợi

```
 ✅  Environment aws://<ACCOUNT_ID>/us-east-1 bootstrapped.
```

---


![Screenshot](/images/5-Workshop/step-501.png)


![Screenshot](/images/5-Workshop/step-502.png)


