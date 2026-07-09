---
title : "Update Layer ARN in CDK"
date : "2026-07-09"
weight : 8
chapter : false
pre : " <b> Step 8 </b> "
---


---

## Purpose

Update the Lambda Layer ARN in the CDK stack with the newly published version number.

---

## Commands

Open the backend stack file:

```powershell
notepad C:\Users\ADMIN\Desktop\BC\DEMO\infrastructure\stacks\backend_stack.py
```

Find this line:

```python
layer_arn = 'arn:aws:lambda:us-east-1:<ACCOUNT_ID>:layer:CloudNexus-PythonDeps:1'
```

Update the version number at the end (e.g., `:1`, `:2`, `:3`, `:4`) depending on how many times you published the layer.

---


