---
title : "Cập nhật Layer ARN trong CDK"
date : "2026-07-09"
weight : 8
chapter : false
pre : " <b> Step 8 </b> "
---


---

## Mục Đích

Cập nhật Lambda Layer ARN trong CDK stack với version number mới được publish.

---

## Các Lệnh

Mở file backend stack:

```powershell
notepad C:\Users\ADMIN\Desktop\BC\DEMO\infrastructure\stacks\backend_stack.py
```

Tìm dòng này:

```python
layer_arn = 'arn:aws:lambda:us-east-1:<ACCOUNT_ID>:layer:CloudNexus-PythonDeps:1'
```

Cập nhật số version ở cuối (VD: `:1`, `:2`, `:3`, `:4`) tùy vào lần publish layer.

---

