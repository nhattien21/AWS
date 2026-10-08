---
title : "Prerequisites"
date : "2026-07-09"
weight : -1
chapter : false
pre : " <b> Step 0 </b> "
---

---

## Required Tools

| Tool | Requirement | Install |
|------|-------------|---------|
| AWS Account | Active | [Register AWS](https://aws.amazon.com) |
| Region | `us-east-1` | Select in AWS Console |
| AWS CLI | v2.x | `winget install Amazon.AWSCLI` |
| Node.js | v18+ | `winget install OpenJS.NodeJS.LTS` |
| Python | 3.12 | `winget install Python.Python.3.12` |
| npm | v9+ | (comes with Node.js) |
| AWS CDK | v2.170+ | `npm install -g aws-cdk` |
| Git | Latest | `winget install Git.Git` |
| Google API Key | Gemini free | [Get here](https://aistudio.google.com/app/apikey) |

---

## Verify Installation

```powershell
aws --version
node --version
npm --version
cdk --version
python --version
```

---

![Screenshot](/images/5-Workshop/step-0.png)

---

## Required IAM Permissions

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": [
      "cloudformation:*", "s3:*", "iam:*", "lambda:*",
      "apigateway:*", "cognito-idp:*", "dynamodb:*",
      "sqs:*", "sns:*", "states:*", "secretsmanager:*",
      "logs:*", "ssm:GetParameter", "ecr:*"
    ],
    "Resource": "*"
  }]
}
```

---

## Estimated Cost

> All services in this lab are within AWS Free Tier or have very low cost (< $1/month if not heavily used).

