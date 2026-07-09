---
title : "Concept & Objectives"
date : "2026-07-09"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---


---

**Cloud Nexus** is a Threat Modeling Platform designed for cybersecurity professionals and infrastructure architects. It provides a visual, interactive environment where users can design network topologies by dragging and dropping components, then leverage AI (Google Gemini) to automatically analyze security vulnerabilities, simulate potential attack paths, and receive actionable defense recommendations.

**Target Users:**
- Security Analysts seeking automated vulnerability assessment
- Cloud Architects designing secure network topologies
- Cybersecurity Students learning attack/defense patterns
- Penetration Testers planning engagement strategies

---

## Problems Solved

| Problem | Solution |
|---------|---------|
| Manual network vulnerability detection is time-consuming | AI automatically scans topology using Gemini |
| Difficult to visualize and understand attack paths | Animated simulation on ReactFlow canvas |
| Complex test environment setup | Serverless on AWS, zero server management |
| No standardized defense assessment | Compare attack paths before/after adding defenses |
| Lack of interactive learning tools | Real-time terminal interface with AI commands |

---

## Project Objectives

| Criteria | Description |
|----------|-------------|
| **Visual Editor** | ReactFlow-based drag-and-drop topology builder |
| **AI Integration** | Google Gemini generates topologies, analyzes vulnerabilities, simulates attacks |
| **Real-time Feedback** | Terminal-style interface for AI commands and results |
| **Serverless Backend** | Lambda + API Gateway (FastAPI + Mangum) |
| **Infrastructure as Code** | AWS CDK — deploy/destroy with single commands |
| **Secure API Key Storage** | AWS Secrets Manager (not hardcoded) |
| **CDN Delivery** | CloudFront for global low-latency access |

---

## Success Criteria

1. Frontend loads from CloudFront CDN: `https://d3rs3evkmfvesp.cloudfront.net/`
2. API Gateway returns `{"status":"ok"}` at `/api/health`
3. Lambda calls Google Gemini and returns valid topology JSON
4. Attack simulation displays animated attack paths on canvas
5. Entire infrastructure deployed with `cdk deploy` and destroyed with `cdk destroy`

---

## Alignment with Cloud Architecture Best Practices

- **Serverless-first:** Lambda, API Gateway, S3, CloudFront
- **Infrastructure as Code:** AWS CDK (Python)
- **Security:** Secrets Manager, IAM roles, no hardcoded credentials
- **Cost Optimization:** Free tier for core services, pay-per-use
- **Global Delivery:** CloudFront CDN with HTTPS

