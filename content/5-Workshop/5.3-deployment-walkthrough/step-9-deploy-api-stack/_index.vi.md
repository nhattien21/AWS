---
title : "Deploy API Stack"
date : "2026-07-09"
weight : 9
chapter : false
pre : " <b> Step 9 </b> "
---


---

## Mô Tả

Tạo API Gateway, Lambda (FastAPI + Mangum), và Secrets Manager.

---

## Các Lệnh

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO\infrastructure
cdk deploy CloudNexus-Backend --require-approval never
```

---

## Kết Quả Mong Đợi

```
✅  CloudNexus-Backend
Outputs:
  CloudNexus-Backend.ApiUrl = https://<API_ID>.execute-api.us-east-1.amazonaws.com/prod/
  CloudNexus-Backend.FunctionName = CloudNexus-BackendHandler-<SUFFIX>
```

---

## Tài Nguyên Đã Tạo

| Tài nguyên | AWS Service | Mục đích |
|------------|-------------|----------|
| `CloudNexus-BackendHandler` | Lambda | FastAPI backend |
| `CloudNexus-PythonDeps` | Layer | Python dependencies |
| `CloudNexus-API` | API Gateway | REST endpoint |
| `CloudNexus-Secrets` | Secrets Manager | Lưu trữ API key |

---






![Screenshot](/images/5-Workshop/step-9.png)


---

