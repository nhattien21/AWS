---
title: "WEEK 7 WORKLOG"
date: "2026-08-29"
weight: 7
chapter: false
pre: " <b> 1.7 </b> "
---

# **WEEK 7 WORKLOG**

### **Week 7 Objectives**

* Learn and deploy a **Serverless** architecture using **AWS Lambda** and **API Gateway**.
* Automate the Serverless deployment using a CloudFormation template.
* Learn the advanced CloudFormation feature **StackSets** to deploy resources across multiple accounts/regions.
* Begin learning a new IaC tool: **Terraform** (HCL syntax).
* Practice basic Terraform commands (`init`, `plan`, `apply`) and learn configuration management with **variables**.

---

### **Tasks to be carried out this week**

| Day | Task | Start Date | Completion Date | Reference/Material |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Mon) | **Learn Serverless**: Study AWS Lambda, write CloudFormation (for IAM Role, Lambda Function) and integrate API Gateway. | 29/05/2026 | 29/05/2026 | |
| 2 (Tue) | **CloudFormation StackSets**: Learn StackSets, write an S3 template, and deploy it to multiple accounts/regions. | 01/06/2026 | 01/06/2026 | |
| 3 (Wed) | **Start Terraform**: Write the first `main.tf` file (AWS provider, `aws_s3_bucket` resource). Run `terraform init` and `plan`. | 02/06/2026 | 02/06/2026 | |
| 4 (Thu) | **CloudFormation Cleanup**: (Consolidation day) Deploy, test (SSH/HTTP), and delete a complete stack (`delete-stack`). | 03/06/2026 | 03/06/2026 | |
| 5 (Fri) | **Complete Terraform Intro**: Run `terraform apply` to create S3 bucket. Learn to use `variables` and `.tfvars` files. | 04/06/2026 | 04/06/2026 | |

---

### **Week 7 Achievements**

* Mastered the concept of **Serverless** architecture.
* Successfully deployed an **AWS Lambda** function (Node.js) and configured an **IAM Role** (for logging) entirely using **CloudFormation**.
* Successfully integrated the Lambda function with **API Gateway** (using `AWS_PROXY`) to create a public-facing API endpoint.
* Mastered and successfully deployed **CloudFormation StackSets**, understanding how to use it for resource synchronization across multiple accounts.
* Began learning a new IaC tool, **Terraform**, and its HCL syntax.
* Successfully wrote the first Terraform configuration to create an `aws_s3_bucket` resource.
* Mastered the basic Terraform workflow:
    * `terraform init`: To initialize the provider.
    * `terraform plan`: To preview changes.
    * `terraform apply`: To create the resources.
* Learned to parameterize Terraform configurations using **variables** (in `.tf` files) and manage their values (in `.tfvars` files).