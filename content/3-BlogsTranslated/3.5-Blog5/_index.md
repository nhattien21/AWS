---
title: "Blog 2"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# SUMMARY: METHODS TO SECURE THE SOFTWARE SUPPLY CHAIN ACCORDING TO AWS WELL-ARCHITECTED FRAMEWORK

This article addresses the growing risks of software supply chain attacks (such as malicious code injected into open-source libraries). To counter these threats, AWS recommends applying principles from the Security Pillar of the AWS Well-Architected Framework.

Here are the 5 core method groups:

### 1. Identity & Access Management
Attackers often target development environments (developer machines) or CI/CD pipelines to steal credentials.
* **Eliminate Long-term Credentials:** Absolutely do not hardcode keys like AWS IAM Access Keys on personal computers or within the source code.
* **Use Short-lived Credentials:** CI/CD environments (such as GitHub Actions, GitLab) should use OIDC (OpenID Connect) to acquire single-use temporary IAM credentials.
* **Apply Least Privilege:** CI/CD pipelines should only be granted the minimum necessary permissions required to perform their tasks (e.g., only allowed to push images to a specific ECR repository, without deletion rights).

### 2. Data Protection & Integrity
You must ensure that your source code and executable files (artifacts) cannot be tampered with during transit.
* **Centralized Dependency Management:** Use AWS CodeArtifact as a proxy to store and manage internal/external packages. This prevents typosquatting attacks (where malicious libraries are named similarly to legitimate ones) by allowing downloads only from approved sources.
* **Artifact Signing:** Use AWS Signer to digitally sign container images or code snippets. Systems in the Production environment (e.g., Amazon EKS) must be configured to verify the signature's validity before allowing deployment.

### 3. Vulnerability Management
Traditional static scanning tools (CVE) lack the capability to detect unknown Zero-day malware.
* **Automated and Continuous Scanning in CI/CD:** Integrate Amazon Inspector directly into the build process. This tool analyzes behavior to detect potential malware (sleeper packages) even before they are publicly disclosed.
* **Build an SBOM (Software Bill of Materials):** Always maintain a Software Bill of Materials (SBOM). It helps you track exactly which libraries your application uses, enabling an extremely rapid response when a new vulnerability (like Log4j) is announced.

### 4. Infrastructure Protection
* **Isolate CI/CD Environments:** The build/deployment system must run in an isolated environment, limiting external internet access to prevent malware from automatically downloading additional malicious payloads (call-home) during the build.
* **Apply Defense in Depth:** Require MFA (Multi-Factor Authentication) and multi-approval workflows for critical changes in the Production environment.

### 5. Detection & Incident Response
Assuming that the system could be compromised, you need the capability to detect anomalies as early as possible.
* **Advanced API Monitoring:** Always enable AWS CloudTrail to log every action. Set up alerts for anomalous behavior, such as a push image command (`ecr:PutImage`) originating from an unknown IP address instead of a valid CI/CD server.
* **Use AI/ML for Threat Detection:** Activate Amazon GuardDuty to monitor and detect anomalous network flows or cryptojacking behavior that might occur if the supply chain is compromised.

> **Core Summary:** Supply chain security is not just about scanning source code; it is a comprehensive defense-in-depth strategy: From granting least privilege to CI/CD, managing library origins, and signing output artifacts, to continuously monitoring all behaviors within the AWS environment.

---

# [Security] Software Supply Chain Security according to AWS Well-Architected Standards 🛡️☁️

Hello everyone in the AWS Study Group VN! 👋

Recently, software supply chain attacks via the npm Registry—such as the Shai-Hulud, tea.xyz, or axios incidents—have become increasingly common. Attackers typically target two vulnerabilities: stealing maintainer accounts to inject malicious code, and user CI/CD environments unknowingly executing these packages.

![Software Supply Chain Attack Flow Simulation](/images/image1.png)
*Caption: Figure 1 - Simulation of a software supply chain attack flow: Attacker compromises the maintainer's system -> publishes an npm version containing malicious code -> users (developers/CI/CD systems) install it -> credentials are harvested and exfiltrated back to the attacker.*

So, how can we protect our systems against these risks? Based on the AWS Well-Architected Framework (Security Pillar), today our team summarizes 5 best practices from the AWS Security Blog to help you strengthen your defenses:

### 1. Eliminate Long-term Credentials and Apply Least Privilege
Malware frequently scans CI/CD environments and developer machines searching for secrets (such as npm tokens or AWS IAM access keys).
* **For Developers:** Use the `aws login` command to retrieve short-lived credentials instead of storing hardcoded keys on your local machine.
* **For CI/CD:** Use OIDC (OpenID Connect) with GitHub Actions/GitLab CI to issue temporary credentials for each individual job run. If you must use a third-party tool that doesn't support OIDC, store secrets in AWS Secrets Manager and automate key rotation.

### 2. Defense in Depth & Artifact Signing
A compromised account should not mean "game over."
* Enforce MFA and require multi-approval workflows for all deployments to the Production environment.
* Use AWS Signer to create cryptographic signatures for your artifacts. The Amazon ECR managed signing feature can automatically sign container images when they are pushed to ECR. Subsequently, admission controllers on EKS (such as Kyverno) verify the signature's validity before allowing deployment.

![Secure CI/CD Flow with AWS CodePipeline and AWS Signer](/images/image2.png)
*Caption: Figure 2 - Secure CI/CD flow with AWS CodePipeline and AWS Signer: Developer pushes code -> ECR automatically triggers AWS Signer upon receiving a new image -> Signature is stored alongside the image -> Runtime environment (EKS/ECS) pulls and verifies the signature before deployment.*

### 3. Centralized Dependency Management
* Instead of letting developers pull packages directly from external registries, use AWS CodeArtifact to manage internal and external packages. You can define a list of safe upstreams, completely blocking typosquatting attacks (where bad actors create packages with names very similar to real ones).
* **Verify npm provenance attestation:** A feature that helps verify the package you download was genuinely built from the correct source code and CI/CD workflow of the author, preventing artifact tampering.

### 4. Automated and Continuous Scanning
Traditional vulnerability scanning tools (which rely solely on CVE databases) are helpless against unreported Zero-day malware.
* Integrate Amazon Inspector directly into your CI/CD workflow. Unlike standard CVE scanning, Inspector uses behavioral analysis at scale to detect "sleeper packages" (dormant malware) or packages conducting anomalous information harvesting even before they are officially assigned a malicious package ID (MAL-ID).
* Always maintain SBOMs (Software Bills of Materials) to know exactly which dependencies your application runs, allowing you to isolate blast radiuses extremely fast during an incident.

![Amazon Inspector and Automated Remediation Workflow](/images/image3.png)
*Caption: Figure 3 - Amazon Inspector and automated response workflow: Inspector scans ECR, CodeArtifact, and Lambda while analyzing behavior to detect critical vulnerabilities (Malicious Package, Credential Harvesting). When detected, a finding event is sent to an automated remediation pipeline (EventBridge -> Security Hub -> SNS -> Lambda Remediation).*

### 5. Enhanced Logging & Monitoring
* Always enable AWS CloudTrail to audit all API calls. Watch closely for anomalous activities such as: `sts:AssumeRole` executions from unexpected IP ranges, or `ecr:PutImage` actions pushed directly from a developer machine bypassing the CI/CD pipeline.
* Combine Amazon GuardDuty and EventBridge to detect threats in real-time and trigger automated responses if risks manifest.

### Overall Architecture:
![Overall Software Supply Chain Security Architecture](/images/image3.png)
*Caption: Figure 4 - Comprehensive 5-stage software supply chain security architecture: Prevent credential leaks (IAM, Secrets Manager) -> Control Dependencies (CodeArtifact, CodeBuild) -> Artifact Signing & Scanning (Inspector, Signer, EKS/ECS) -> Monitor (GuardDuty, CloudTrail) -> Automated Response (SNS, Lambda).*

---

## Conclusion
In summary: Supply chain security is not just about writing secure code; it is about building a defense-in-depth architecture, limiting privileges, and maintaining strict control over every single artifact introduced into the system.