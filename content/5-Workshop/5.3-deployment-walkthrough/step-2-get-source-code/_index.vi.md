---
title : "Tải mã nguồn"
date : "2026-07-09"
weight : 2
chapter : false
pre : " <b> Step 2 </b> "
---


---

## Các Lệnh

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO
Get-ChildItem -Directory
```

---

## Cấu Trúc Dự Án

```
DEMO/
├── src/                    # React frontend source (Vite)
│   ├── components/
│   ├── hooks/
│   │   ├── useAI.js        # Tích hợp AI
│   │   └── useSimulation.js # Logic mô phỏng
│   ├── store/
│   │   └── useStore.js     # Quản lý state
│   └── api.js
├── backend/                # FastAPI backend
│   ├── main.py             # FastAPI app
│   ├── ai_service.py       # Tích hợp Gemini
│   ├── handler.py          # Lambda entry (Mangum)
│   ├── .env                # API keys (KHÔNG commit)
│   └── requirements.txt
├── infrastructure/         # AWS CDK code
│   ├── app.py              # CDK entry point
│   ├── stacks/
│   │   ├── frontend_stack.py    # S3 + CloudFront
│   │   ├── backend_stack.py    # Lambda + API Gateway
│   │   └── secrets_stack.py    # Secrets Manager
│   ├── cdk.json
│   └── requirements.txt
├── scripts/
│   ├── deploy.ps1          # Deployment Windows
│   ├── deploy.sh           # Deployment Mac/Linux
│   ├── push-secret.ps1     # Đẩy API key lên Secrets Manager
│   └── teardown.ps1        # Xóa AWS resources
├── dist/                   # Frontend build output
└── package.json
```

---

![Screenshot](/images/5-Workshop/step-2.png)
