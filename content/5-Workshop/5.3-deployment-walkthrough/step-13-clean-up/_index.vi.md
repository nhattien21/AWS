---
title : "Dọn dẹp tài nguyên"
date : "2026-07-09"
weight : 13
chapter : false
pre : " <b> Clean Up </b> "
---

---

## Tổng Quan

Phần này hướng dẫn cách dọn dẹp tất cả AWS resources để tránh phí ongoing.

---

## Bước 1: Xóa CDK Stacks

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO\infrastructure
cdk destroy --all --force
```

**Kết quả mong đợi:**
```
 ✅  CloudNexus-Frontend: destroyed
 ✅  CloudNexus-Backend: destroyed
 ✅  CloudNexus-Simulation: destroyed
```

---

## Bước 2: Xóa S3 Buckets (Được giữ lại)

S3 buckets với `RemovalPolicy.RETAIN` phải xóa thủ công:

```powershell
# Liệt kê buckets
aws s3 ls | Select-String "cloudnexus"

# Xóa buckets
aws s3 rb s3://cloudnexus-frontend-<SUFFIX> --force
aws s3 rb s3://cloudnexus-results-<ACCOUNT>-us-east-1 --force
```

---

## Bước 3: Xóa Lambda Layer Versions

```powershell
aws lambda list-layer-versions --layer-name CloudNexus-PythonDeps
aws lambda delete-layer-version --layer-name CloudNexus-PythonDeps --version-number 1
```

---

## Bước 4: Xóa Secrets Manager

```powershell
aws secretsmanager delete-secret --secret-id cloud-nexus/google-api-key --force-delete-without-recovery
```

---

## Bước 5: Xóa CloudWatch Log Groups

```powershell
aws logs delete-log-group --log-group-name /aws/lambda/CloudNexus-BackendHandler
```

---

## Bước 6: Xóa CDK Bootstrap (Tùy chọn)

```powershell
aws s3 rb s3://cdk-hnb659fds-assets-<ACCOUNT>-us-east-1 --force
aws cloudformation delete-stack --stack-name CDKToolkit
```

---
