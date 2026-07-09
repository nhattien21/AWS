---
title : "Clean Up Resources"
date : "2026-07-09"
weight : 13
chapter : false
pre : " <b> Clean Up </b> "
---


---

## Overview

This section shows how to clean up all AWS resources to avoid ongoing charges.

---

## Step 1: Delete CDK Stacks

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO\infrastructure
cdk destroy --all --force
```

**Expected:**
```
 ✅  CloudNexus-Frontend: destroyed
 ✅  CloudNexus-Backend: destroyed
 ✅  CloudNexus-Simulation: destroyed
```

---

## Step 2: Delete S3 Buckets (Retained)

S3 buckets with `RemovalPolicy.RETAIN` must be deleted manually:

```powershell
# List buckets
aws s3 ls | Select-String "cloudnexus"

# Delete buckets
aws s3 rb s3://cloudnexus-frontend-<SUFFIX> --force
aws s3 rb s3://cloudnexus-results-<ACCOUNT>-us-east-1 --force
```

---

## Step 3: Delete Lambda Layer Versions

```powershell
aws lambda list-layer-versions --layer-name CloudNexus-PythonDeps
aws lambda delete-layer-version --layer-name CloudNexus-PythonDeps --version-number 1
```

---

## Step 4: Delete Secrets Manager

```powershell
aws secretsmanager delete-secret --secret-id cloud-nexus/google-api-key --force-delete-without-recovery
```

---

## Step 5: Delete CloudWatch Log Groups

```powershell
aws logs delete-log-group --log-group-name /aws/lambda/CloudNexus-BackendHandler
```

---

## Step 6: Delete CDK Bootstrap (Optional)

```powershell
aws s3 rb s3://cdk-hnb659fds-assets-<ACCOUNT>-us-east-1 --force
aws cloudformation delete-stack --stack-name CDKToolkit
```

---



