---
title : "Architecture & Technical Design"
date : "2026-07-09"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---


---

## System Architecture

![System Architecture](/images/2-Proposal/archi.png)

---

## Service Selection & Rationale

| Service | Purpose | Cost |
|---------|---------|------|
| **CloudFront** | Global CDN, HTTPS, low latency | ~$0.085/GB |
| **S3** | Static website hosting for React app | ~$0.023/GB/mo |
| **API Gateway (HTTP)** | Managed REST API, 70% cheaper than REST | $1.00/M calls |
| **Lambda** | Serverless compute, pay-per-request | $0.20/M requests |
| **Secrets Manager** | Securely store Google API key | ~$0.40/month |
| **CloudWatch** | Logging and monitoring (default) | Free tier |

---

## Technology Stack

### Frontend

| Technology | Purpose |
|------------|---------|
| **React 18** | UI framework |
| **ReactFlow** | Drag-and-drop topology editor |
| **Vite** | Build tool, fast HMR |
| **CSS** | Styling (dark mode theme) |

### Backend

| Technology | Purpose |
|------------|---------|
| **FastAPI** | REST API framework |
| **Mangum** | AWS Lambda adapter for ASGI |
| **Google Gemini API** | AI for topology generation, scanning, simulation |

### Infrastructure

| Technology | Purpose |
|------------|---------|
| **AWS CDK (Python)** | Infrastructure as Code |
| **TypeScript/Python** | CDK stack definitions |

---

## Security Architecture

### Secrets Management

```python
# Lambda reads API key from Secrets Manager at runtime
# NOT stored in environment variables or code

_api_key = os.environ.get('GOOGLE_API_KEY')
if not _api_key:
    secrets_client = boto3.client('secretsmanager')
    resp = secrets_client.get_secret_value(SecretId='cloud-nexus/google-api-key')
    _api_key = resp['SecretString']
```

### IAM Role Policy (Lambda)

```json
{
  "Effect": "Allow",
  "Action": [
    "secretsmanager:GetSecretValue"
  ],
  "Resource": "arn:aws:secretsmanager:ap-southeast-1:<ACCOUNT>:secret:cloud-nexus/google-api-key"
}
```

### Security Principles

1. **Principle of Least Privilege:** Lambda only has permissions for Secrets Manager
2. **No hardcoded credentials:** API key stored in Secrets Manager
3. **HTTPS everywhere:** CloudFront → API Gateway → Lambda
4. **IAM roles:** Lambda uses role-based access, no access keys

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/health` | Health check |
| POST | `/api/ai/generate` | Generate topology with AI |
| POST | `/api/simulation/scan` | Scan for vulnerabilities |
| POST | `/api/simulation/run` | Run attack simulation |
| POST | `/api/topology/validate` | Validate topology |

---

## Frontend Architecture

### State Management

```javascript
// Zustand-like store structure
{
  nodes: [],      // ReactFlow nodes
  edges: [],      // ReactFlow edges
  scenarios: [], // Attack scenarios from AI
  logs: [],       // Terminal logs
  // ... other state
}
```

### AI Command Interface

```
$ generate a secure web application topology
$ scan for vulnerabilities
$ simulate SQL injection attack
$ help
```

---

## Scalability & Performance

| Aspect | Implementation |
|--------|----------------|
| **Scale** | Lambda auto-scales, no server to manage |
| **CDN** | CloudFront caches globally |
| **Static Hosting** | S3 handles all frontend traffic |
| **Cold Start** | Lambda cold start ~4s, subsequent requests ~60ms |
| **Memory** | Lambda 512MB, actual usage ~193MB |

---

## Cost Estimation

| Resource | Free Tier | After Free Tier |
|----------|-----------|-----------------|
| Lambda | 1M req/mo | $0.20/M |
| API Gateway | 1M calls/mo | $1.00/M |
| S3 | 5GB | $0.023/GB/mo |
| CloudFront | 1TB/mo | $0.085/GB |
| Secrets Manager | - | ~$0.40/mo |

**Total estimated cost for dev/testing:** < $1/month (within free tiers)

