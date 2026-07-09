---
title : "Configure Google API Key"
date : "2026-07-09"
weight : 11
chapter : false
pre : " <b> Step 11 </b> "
---
# Step 11: Configure Google API Key

---

## Overview

Lambda needs Google API key to call Gemini. The API key is stored in AWS Secrets Manager for security.

---

## Method 1: Push Secret Script (Recommended)

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO
.\scripts\push-secret.ps1
```

This script reads `backend/.env` and uploads to Secrets Manager.

---

## Method 2: Manual via AWS CLI

```powershell
aws secretsmanager put-secret-value `
  --secret-id cloud-nexus/google-api-key `
  --secret-string "<YOUR_GOOGLE_API_KEY>"
```

---

## Method 3: Update Lambda Environment Variable

```powershell
aws lambda update-function-configuration --function-name CloudNexus-BackendHandler-<SUFFIX> `
  --environment "Variables={GOOGLE_API_KEY=<YOUR_KEY>}"
```

---

## Verify Configuration

```powershell
# Check if secret exists
aws secretsmanager describe-secret --secret-id cloud-nexus/google-api-key

# Or verify Lambda environment
aws lambda get-function-configuration --function-name CloudNexus-BackendHandler-<SUFFIX> --query "Environment.Variables"
```

---
![Screenshot](/images/5-Workshop/step-11.png)

