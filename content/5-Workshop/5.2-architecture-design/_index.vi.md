---
title : "Kiến Trúc & Thiết Kế Kỹ Thuật"
date : "2026-07-09"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---


---

## Kiến Trúc Hệ Thống

![Kiến Trúc Hệ Thống](/images/2-Proposal/archi.png)

---

## Lựa Chọn Dịch Vụ & Lý Do

| Dịch vụ | Mục đích | Chi phí |
|---------|---------|---------|
| **CloudFront** | CDN toàn cầu, HTTPS, low latency | ~$0.085/GB |
| **S3** | Static website hosting cho React app | ~$0.023/GB/tháng |
| **API Gateway (HTTP)** | Managed REST API, rẻ hơn REST 70% | $1.00/M calls |
| **Lambda** | Serverless compute, trả tiền theo request | $0.20/M requests |
| **Secrets Manager** | Lưu trữ an toàn Google API key | ~$0.40/tháng |
| **CloudWatch** | Logging và monitoring (default) | Free tier |

---

## Tech Stack

### Frontend

| Công nghệ | Mục đích |
|----------|----------|
| **React 18** | UI framework |
| **ReactFlow** | Trình chỉnh sửa topology kéo-thả |
| **Vite** | Build tool, HMR nhanh |
| **CSS** | Styling (dark mode theme) |

### Backend

| Công nghệ | Mục đích |
|----------|----------|
| **FastAPI** | REST API framework |
| **Mangum** | AWS Lambda adapter cho ASGI |
| **Google Gemini API** | AI cho tạo topology, quét, mô phỏng |

### Infrastructure

| Công nghệ | Mục đích |
|----------|----------|
| **AWS CDK (Python)** | Infrastructure as Code |
| **TypeScript/Python** | CDK stack definitions |

---

## Kiến Trúc Bảo Mật

### Quản Lý Secrets

```python
# Lambda đọc API key từ Secrets Manager khi chạy
# KHÔNG lưu trong environment variables hoặc code

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

### Nguyên Tắc Bảo Mật

1. **Principle of Least Privilege:** Lambda chỉ có quyền với Secrets Manager
2. **Không hardcode credentials:** API key lưu trong Secrets Manager
3. **HTTPS mọi nơi:** CloudFront → API Gateway → Lambda
4. **IAM roles:** Lambda dùng role-based access, không access keys

---

## API Endpoints

| Method | Endpoint | Mô tả |
|--------|----------|--------|
| GET | `/api/health` | Health check |
| POST | `/api/ai/generate` | Tạo topology bằng AI |
| POST | `/api/simulation/scan` | Quét lỗ hổng bảo mật |
| POST | `/api/simulation/run` | Chạy mô phỏng tấn công |
| POST | `/api/topology/validate` | Validate topology |

---

## Frontend Architecture

### State Management

```javascript
// Cấu trúc store kiểu Zustand
{
  nodes: [],      // ReactFlow nodes
  edges: [],      // ReactFlow edges
  scenarios: [], // Attack scenarios từ AI
  logs: [],       // Terminal logs
  // ... other state
}
```

### Giao Diện Lệnh AI

```
$ generate a secure web application topology
$ scan for vulnerabilities
$ simulate SQL injection attack
$ help
```

---

## Khả Năng Mở Rộng & Hiệu Suất

| Khía cạnh | Triển khai |
|-----------|------------|
| **Scale** | Lambda tự động scale, không cần quản lý server |
| **CDN** | CloudFront cache toàn cầu |
| **Static Hosting** | S3 xử lý tất cả frontend traffic |
| **Cold Start** | Lambda cold start ~4s, request tiếp theo ~60ms |
| **Memory** | Lambda 512MB, sử dụng thực tế ~193MB |

---

## Ước Tính Chi Phí

| Tài nguyên | Free Tier | Sau Free Tier |
|------------|-----------|---------------|
| Lambda | 1M req/tháng | $0.20/M |
| API Gateway | 1M calls/tháng | $1.00/M |
| S3 | 5GB | $0.023/GB/tháng |
| CloudFront | 1TB/tháng | $0.085/GB |
| Secrets Manager | - | ~$0.40/tháng |

**Tổng chi phí ước tính cho dev/testing:** < $1/tháng (trong free tiers)

