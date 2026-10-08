---
title : "Kiểm tra hệ thống"
date : "2026-07-09"
weight : 12
chapter : false
pre : " <b> Step 12 </b> "
---
# Step 12: Kiểm Tra Hệ Thống

---

## URL Demo Trực Tiếp

**Truy cập ứng dụng đã deploy:** [https://d3rs3evkmfvesp.cloudfront.net/](https://d3rs3evkmfvesp.cloudfront.net/)

---

## Health Check

```powershell
# Lấy API URL từ CloudFormation
$apiUrl = aws cloudformation describe-stacks --stack-name CloudNexus-Backend --query "Stacks[0].Outputs[?OutputKey=='ApiUrl'].OutputValue" --output text

# Test health endpoint
curl -s "$apiUrl/api/health"
```

**Kết quả mong đợi:**
```json
{"status":"ok"}
```

---

## Kiểm Tra Frontend

Mở trên trình duyệt: [https://d3rs3evkmfvesp.cloudfront.net/](https://d3rs3evkmfvesp.cloudfront.net/)

**Xác minh:**
- React app load hoàn toàn
- ReactFlow canvas hiển thị
- Terminal interface nhìn thấy được
- Dark mode theme active

---

## Test AI Generation

```powershell
$body = '{"prompt":"simple web server with database"}'
curl -s -X POST "$apiUrl/api/ai/generate" -H "Content-Type: application/json" -d $body
```

**Kết quả mong đợi:** JSON topology với nodes và edges

---

![Screenshot](/images/5-Workshop/step-12.png)