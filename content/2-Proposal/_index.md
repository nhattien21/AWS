---
title: "Proposal"
date: "2026-07-04"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# Cloud Nexus

---

## 1. Project Overview

**Cloud Nexus** is a Threat Modeling Platform for cybersecurity professionals and infrastructure architects. The system allows users to design network topologies visually using a drag-and-drop interface, then leverage AI (Google Gemini 2.0 Flash) to automatically:

- Generate network topologies from text descriptions
- Scan for vulnerabilities and identify attack scenarios
- Simulate attack paths with step-by-step visualization
- Recommend defense measures for mitigation

### Technology Stack

| Component | Technology |
|-----------|------------|
| Frontend | React + Vite + ReactFlow + Tailwind CSS |
| Backend | FastAPI on AWS Lambda + API Gateway |
| AI Engine | Google Gemini API (Gemini 2.0 Flash) |
| Infrastructure | AWS CDK (TypeScript), Python 3.12 ARM64 |
| AWS Services | S3, API Gateway, Lambda, Cognito, DynamoDB, SQS, SNS, Step Functions, Secrets Manager |

---

## 2. Objectives

### General Objective
Build a serverless platform on AWS that automates network security assessment — from topology design to vulnerability detection and attack simulation.

### Specific Objectives
- **REST API + Web Dashboard:** Deploy FastAPI backend on Lambda with API Gateway, React frontend hosted on S3 + CloudFront
- **AI Integration:** Google Gemini generates network topologies, analyzes vulnerabilities, simulates attack paths with realistic hacker techniques
- **Attack Visualization:** Animated attack path simulation on ReactFlow canvas showing compromise steps
- **Defense Recommendations:** AI-powered defense suggestions with the ability to re-run simulations after applying defenses
- **Serverless Architecture:** Entire backend runs on Lambda + API Gateway with no server management
- **Infrastructure as Code:** AWS CDK enables one-command deployment and destruction


---

## 3. Problem Statement

| Problem | Solution |
|---------|----------|
| Manual network vulnerability detection is time-consuming and requires deep expertise | AI automatically scans topology and identifies attack scenarios |
| Difficult to visualize and understand attack paths | Visual step-by-step simulation on ReactFlow canvas |
| Complex test environment setup requires significant infrastructure | Serverless on AWS Lambda, no server provisioning needed |
| No way to evaluate defense effectiveness before deployment | Compare attack paths before/after applying defense measures |
| Traditional tools are complex and require extensive training | Intuitive drag-and-drop interface for topology design |

### Target Audience
- Security Analysts conducting network security assessments
- Cloud Architects designing secure cloud infrastructure
- Cybersecurity Students learning threat modeling concepts
- Red Team Operators planning penetration tests

---

## 4. System Architecture

![System Architecture](/images/2-Proposal/archi.png)

---

## 5. Timeline (Jun 1 → Jul 8)

| Phase | Content | Duration |
|-------|---------|----------|
| **Phase 1: Foundation** | Environment setup, AWS account configuration, IAM policies, project scaffolding | Jun 1 → Jun 4 |
| **Phase 2: Frontend Development** | Build React UI with ReactFlow canvas, node components, terminal interface, theme system | Jun 5 → Jun 11 |
| **Phase 3: Backend Development** | FastAPI implementation, AI service integration with Gemini, API endpoints for generation/scan/simulation | Jun 12 → Jun 18 |
| **Phase 4: Infrastructure** | AWS CDK stacks for Lambda, API Gateway, S3, CloudFront, Cognito, Secrets Manager | Jun 19 → Jun 24 |
| **Phase 5: Integration & Deployment** | Lambda Layer for dependencies, stack deployment, API key configuration via Secrets Manager | Jun 25 → Jun 30 |
| **Phase 6: Testing & Refinement** | End-to-end testing, bug fixing, user experience improvements, defense simulation polish | Jul 1 → Jul 6 |
| **Phase 7: Documentation & Finalization** | Project documentation, README, presentation materials, cleanup | Jul 7 → Jul 8 |

---

## 6. Budget

### AWS Monthly Cost (Estimated)

| Service | Free Tier | Cost |
|---------|-----------|------|
| S3 Static Hosting | 5 GB | ~$0.01 |
| API Gateway | 1M calls/mo | $0 (free tier) |
| Lambda | 1M requests/mo | $0 (free tier) |
| Cognito | 50K MAUs | $0 (free tier) |
| DynamoDB | 25 GB | $0 (free tier) |
| SQS | 1M messages/mo | $0 (free tier) |
| SNS | 100K notifications | $0 (free tier) |
| Step Functions | 4K state transitions | $0 (free tier) |
| Secrets Manager | 1 secret | ~$0.40 |
| CloudFront | 1TB transfer | $0 (free tier) |
| **Total** | | **~$0.41/month** |

### Google Gemini API Cost
- Gemini 2.0 Flash: Free tier available with generous rate limits
- Extra cost only applies when exceeding free tier limits

### Summary
The project operates entirely within AWS Free Tier + Google Gemini Free Tier, resulting in virtually no operational costs for development and testing.

---

## 7. Risk Assessment

| Risk | Description | Severity | Mitigation |
|------|-------------|----------|------------|
| **API Key Leak** | Google API key accidentally committed to Git | High | Secrets Manager stores keys securely; .gitignore prevents commits; Lambda reads at runtime |
| **Unexpected Cost** | Lambda called excessively or attacked | Medium | CloudWatch Alarms, AWS Budget Alerts, Lambda concurrency limits |
| **AI Response Error** | Gemini returns invalid JSON or unexpected format | Medium | Retry logic (2 times) with fallback error message |
| **Lambda Timeout** | AI response too slow (>30s) | Low | Increased timeout configuration; async processing via SQS |
| **CDK Deploy Error** | AWS CDK version mismatch between environments | Low | Pinned CDK version; pre-deploy validation checks |
| **CORS Issues** | Browser blocks cross-origin API requests | Low | CORS middleware pre-configured with allowed origins |
| **Data Loss** | DynamoDB accidentally deleted | Medium | Point-in-Time Recovery enabled; regular backups |
| **Simulation Accuracy** | AI-generated attack paths may not reflect real-world scenarios | Medium | Clear disclaimer that simulations are educational; human review recommended |

---


