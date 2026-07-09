---
title : "Deploy Simulation Stack"
date : "2026-07-09"
weight : 6
chapter : false
pre : " <b> Step 6 </b> "
---


---

## Description

This stack creates supporting services for the simulation system.

---

## Commands

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO\infrastructure
cdk deploy CloudNexus-Simulation --require-approval never
```

---

## Expected Output

```
✅  CloudNexus-Simulation
Outputs:
  CloudNexusSimulationStack.ResultBucketName = cloudnexus-results-<ACCOUNT_ID>-us-east-1
  CloudNexusSimulationStack.StateMachineArn = arn:aws:states:us-east-1:<ACCOUNT_ID>:stateMachine:CloudNexus-SimOrchestrator
  CloudNexusSimulationStack.AlertTopicArn = arn:aws:sns:us-east-1:<ACCOUNT_ID>:CloudNexus-Alerts
```

---

## Resources Created

| Resource | AWS Service | Purpose |
|----------|-------------|---------|
| `CloudNexus-ScanQueue` | SQS | Scan job queue |
| `CloudNexus-ResultsTable` | DynamoDB | Store results |
| `CloudNexus-ResultBucket` | S3 | Large result files |
| `CloudNexus-SimOrchestrator` | Step Functions | Workflow orchestration |
| `CloudNexus-Alerts` | SNS | Alert notifications |

---

![Screenshot](/images/5-Workshop/step-6.png)





