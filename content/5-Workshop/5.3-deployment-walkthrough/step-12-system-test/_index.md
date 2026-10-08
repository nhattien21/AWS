---
title : "System Test & Validation"
date : "2026-07-09"
weight : 12
chapter : false
pre : " <b> Step 12 </b> "
---
# Step 12: System Test & Validation

---

## Live Demo URL

**Access the deployed application:** [https://d3rs3evkmfvesp.cloudfront.net/](https://d3rs3evkmfvesp.cloudfront.net/)

---

## Health Check

```powershell
# Get API URL from CloudFormation
$apiUrl = aws cloudformation describe-stacks --stack-name CloudNexus-Backend --query "Stacks[0].Outputs[?OutputKey=='ApiUrl'].OutputValue" --output text

# Test health endpoint
curl -s "$apiUrl/api/health"
```

**Expected:**
```json
{"status":"ok"}
```

---

## Frontend Test

Open in browser: [https://d3rs3evkmfvesp.cloudfront.net/](https://d3rs3evkmfvesp.cloudfront.net/)

**Verify:**
- React app loads completely
- ReactFlow canvas displays
- Terminal interface is visible
- Dark mode theme active

---

## AI Generation Test

```powershell
$body = '{"prompt":"simple web server with database"}'
curl -s -X POST "$apiUrl/api/ai/generate" -H "Content-Type: application/json" -d $body
```

**Expected:** JSON topology with nodes and edges

---

![Screenshot](/images/5-Workshop/step-12.png)
