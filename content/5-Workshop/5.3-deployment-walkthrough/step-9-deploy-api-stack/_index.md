---
title : "Deploy API Stack"
date : "2026-07-09"
weight : 9
chapter : false
pre : " <b> Step 9 </b> "
---


---

## Description

Creates API Gateway, Lambda (FastAPI + Mangum), and Secrets Manager.

---

## Commands

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO\infrastructure
cdk deploy CloudNexus-Backend --require-approval never
```

---

## Expected Output

```
✅  CloudNexus-Backend
Outputs:
  CloudNexus-Backend.ApiUrl = https://<API_ID>.execute-api.us-east-1.amazonaws.com/prod/
  CloudNexus-Backend.FunctionName = CloudNexus-BackendHandler-<SUFFIX>
```

---

## Resources Created

| Resource | AWS Service | Purpose |
|----------|-------------|---------|
| `CloudNexus-BackendHandler` | Lambda | FastAPI backend |
| `CloudNexus-PythonDeps` | Layer | Python dependencies |
| `CloudNexus-API` | API Gateway | REST endpoint |
| `CloudNexus-Secrets` | Secrets Manager | API key storage |

---

![Screenshot](/images/5-Workshop/step-9.png)