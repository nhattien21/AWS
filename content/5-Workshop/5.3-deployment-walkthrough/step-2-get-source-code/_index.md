---
title : "Get Source Code"
date : "2026-07-09"
weight : 2
chapter : false
pre : " <b> Step 2 </b> "
---


---

## Commands

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO
Get-ChildItem -Directory
```

---

## Project Structure

```
DEMO/
├── src/                    # React frontend source (Vite)
│   ├── components/
│   ├── hooks/
│   │   ├── useAI.js        # AI integration
│   │   └── useSimulation.js # Simulation logic
│   ├── store/
│   │   └── useStore.js     # State management
│   └── api.js
├── backend/                # FastAPI backend
│   ├── main.py             # FastAPI app
│   ├── ai_service.py       # Gemini integration
│   ├── handler.py           # Lambda entry (Mangum)
│   ├── .env                # API keys (NOT committed)
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
│   ├── deploy.ps1          # Windows deployment
│   ├── deploy.sh           # Mac/Linux deployment
│   ├── push-secret.ps1     # Push API key to Secrets Manager
│   └── teardown.ps1        # Delete AWS resources
├── dist/                   # Frontend build output
└── package.json
```

---
![Screenshot](/images/5-Workshop/step-2.png)
