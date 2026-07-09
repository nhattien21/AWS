---
title : "Deploy Simulation Stack"
date : "2026-07-09"
weight : 6
chapter : false
pre : " <b> Step 6 </b> "
---


---

## Mô Tả

Stack này tạo các service hỗ trợ cho hệ thống simulation.

---

## Các Lệnh

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO\infrastructure
cdk deploy CloudNexus-Simulation --require-approval never
```

---

## Kết Quả Mong Đợi

```
✅  CloudNexus-Simulation
Outputs:
  CloudNexusSimulationStack.ResultBucketName = cloudnexus-results-<ACCOUNT_ID>-us-east-1
  CloudNexusSimulationStack.StateMachineArn = arn:aws:states:us-east-1:<ACCOUNT_ID>:stateMachine:CloudNexus-SimOrchestrator
  CloudNexusSimulationStack.AlertTopicArn = arn:aws:sns:us-east-1:<ACCOUNT_ID>:CloudNexus-Alerts
```

---

## Tài Nguyên Đã Tạo

| Tài nguyên | AWS Service | Mục đích |
|------------|-------------|----------|
| `CloudNexus-ScanQueue` | SQS | Hàng đợi scan job |
| `CloudNexus-ResultsTable` | DynamoDB | Lưu kết quả |
| `CloudNexus-ResultBucket` | S3 | File kết quả lớn |
| `CloudNexus-SimOrchestrator` | Step Functions | Điều phối workflow |
| `CloudNexus-Alerts` | SNS | Thông báo alert |

---

![Screenshot](/images/5-Workshop/step-6.png)
