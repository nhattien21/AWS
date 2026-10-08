---
title : "Configure AWS CLI"
date : "2026-07-09"
weight : 1
chapter : false
pre : " <b> Step 1 </b> "
---

---

## Commands

```powershell
aws configure
# AWS Access Key ID: ********
# AWS Secret Access Key: ********
# Default region name: us-east-1
# Default output format: json

aws sts get-caller-identity
```

---

## Expected Output

```json
{
  "UserId": "AIDA...",
  "Account": "<ACCOUNT_ID>",
  "Arn": "arn:aws:iam::<ACCOUNT_ID>:user/..."
}
```

---

![Screenshot](/images/5-Workshop/step-0.png)



