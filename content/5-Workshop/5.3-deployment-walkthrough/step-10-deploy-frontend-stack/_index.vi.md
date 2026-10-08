---
title : "Deploy Frontend Stack"
date : "2026-07-09"
weight : 10
chapter : false
pre : " <b> Step 10 </b> "
---


---

## Mô Tả

Tạo S3 bucket và CloudFront distribution cho static website hosting.

---

## Các Lệnh

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO\infrastructure
cdk deploy CloudNexus-Frontend --require-approval never
```

---

## Kết Quả Mong Đợi

```
✅  CloudNexus-Frontend
Outputs:
  CloudNexus-Frontend.WebsiteURL = https://d3rs3evkmfvesp.cloudfront.net
  CloudNexus-Frontend.BucketName = cloudnexus-frontend-<SUFFIX>
  CloudNexus-Frontend.DistributionId = <DIST_ID>
```

---

## Cấu Hình S3

```python
# frontend_stack.py
self.frontend_bucket = Bucket(self, 'FrontendBucket',
    public_read_access=True,
    block_public_access=BlockPublicAccess(block_acls=False),
    website_index_document='index.html',
    website_error_document='index.html',
    removal_policy=RemovalPolicy.RETAIN,
    versioned=True,
)
```


![Screenshot](/images/5-Workshop/step-10.png)



