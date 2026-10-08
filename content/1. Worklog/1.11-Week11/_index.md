---
title: "WEEK 11 WORKLOG"
date: "2026-06-26"
weight: 11
chapter: false
pre: " <b> 1.11 </b> "
---

# **WEEK 11 WORKLOG**

### **Week 11 Objectives**

* Build and package Lambda Layers to supply all required dependencies for backend functions.
* Complete FastAPI backend updates: add the `GET /` API metadata endpoint and verify key API routes (`/api/health`, `/api/ai/generate`, `/api/topology/validate`, `/api/simulation/scan`).
* Perform end-to-end (E2E) integration testing from the CloudFront frontend and fix response payloads for simulation endpoints.
* Study AWS ALB Content-based Routing and logging/monitoring tools (Prometheus, Grafana, ELK, Fluentd).

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Monday) | **K8s Resource Management & Scaling**: Learn about Resource Quotas, Limit Ranges. Hands-on configuration of **Horizontal Pod Autoscaler (HPA)**. | 29/06/2026 | 29/06/2026 | |
| 2 (Tuesday) | - Fix CORS preflight (`OPTIONS`) request error returning `400 Bad Request` from API Gateway to CloudFront<br>- Remove CORS config from API Gateway to prevent duplicate handling with FastAPI `CORSMiddleware`<br>- Add `@app.options` handler in FastAPI to handle preflight requests<br>- Update `allow_origins` in `CORSMiddleware` with CloudFront URL and localhost<br>- Add new Lambda Layer containing all required dependencies<br>- Rebuild and redeploy Lambda function | 30/06/2026 | 30/06/2026 | https://vitejs.dev/guide/build<br>https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide |
| 3 (Wednesday) | **K8s Security (Access)**: Practice configuring **RBAC** (Roles, RoleBindings) for access control. Research logging management tools (Fluentd, ELK). | 30/06/2026 | 30/06/2026 | |
| 4 (Thursday) | **Explore ALB Content-based Routing**: Research and document details on how ALB routes traffic based on content (path, header). | 01/07/2026 | 01/07/2026 | |
| 5 (Friday) | - Integrate and test full feature flows: AI generate topology, topology validation, simulation scan, simulation run, simulation with defense<br>- Perform real-world frontend testing on CloudFront to ensure all features run stably<br>- Fix `simulation/with-defense` endpoint to ensure it returns full `attack_steps` and `defense_mechanisms`<br>- Optimize Lambda function names and CloudFormation outputs | 02/07/2026 | 02/07/2026 | https://docs.aws.amazon.com/lambda/latest/dg/nodejs-package.html |

---

### **Week 11 Achievements**

* **CORS Preflight Fix & API Gateway with CloudFront**:
    * Removed redundant CORS configurations on API Gateway to centralize request handling within FastAPI.
* **Deployment & Lambda Infrastructure Optimization**
* **FastAPI Backend & API Endpoints Verification**
* **End-to-End (E2E) Integration & Testing**