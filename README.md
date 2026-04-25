Here is a **clean, professional README.md** for your **BEM11 Security Baseline Lab (Level 1)** using **CloudFormation + GitSync + Least Privilege IAM + Secrets Management**.

You can copy this directly into your GitHub repo.

---

# 📘 BEM11 Security Baseline Lab – Level 1 (IAM & Secrets)

## 🧭 Overview

This project implements **AWS Security Baseline Level 1 controls** using **Infrastructure as Code (CloudFormation)** and **GitSync deployment automation**.

The goal is to enforce:

* 🔐 Least privilege IAM access
* 🔑 Secure secrets management (SSM + Secrets Manager)
* 🧱 Elimination of long-term credentials for applications
* ⚙️ Automated infrastructure provisioning using GitSync + CloudFormation

---

## 🎯 Objectives

This lab demonstrates:

* Creation of **IAM Roles instead of IAM Users for applications**
* Implementation of **least privilege access policies**
* Secure storage of secrets using:

  * AWS Systems Manager Parameter Store
  * AWS Secrets Manager
* Infrastructure deployment using:

  * AWS CloudFormation
  * GitSync automation

---

## 🏗️ Architecture

This stack provisions:

### 🔐 IAM Roles

* EC2 Role (`*_ec2_role`)
* Lambda Role (`*_lambda_role`)
* ECS Role (`*_ecs_role`)

Each role follows **least privilege principles** and is scoped to required actions only.

---

### 🔑 Secrets Management

* AWS Systems Manager Parameter Store (SecureString)
* AWS Secrets Manager secret

Encryption is handled using **AWS KMS key**

---

## 📂 Project Structure

```
.
├── deployment.yaml              # GitSync configuration
├── security-baseline-least-privilege.yaml  # CloudFormation template
└── README.md
```

---

## 🚀 Deployment Method (GitSync + CloudFormation)

### 1. Push code to GitHub repository

### 2. GitSync configuration

```yaml
template-file-path: security-baseline-least-privilege.yaml
stack-name: bem11-security-baseline-lab
region: us-east-1
capabilities:
  - CAPABILITY_IAM
  - CAPABILITY_NAMED_IAM
```

### 3. Automatic deployment

Any push to the repository triggers:

* CloudFormation stack update
* Resource provisioning

---

## ⚙️ Parameters

| Parameter    | Description                            |
| ------------ | -------------------------------------- |
| FullName     | Used for naming IAM roles              |
| S3BucketName | Bucket used for least privilege access |

---

## 🔐 Security Design

### ✔ IAM Best Practices

* No IAM users for applications
* IAM Roles used for EC2, Lambda, ECS
* Scoped permissions (no wildcard S3 access)

### ✔ Least Privilege Example

Instead of:

```
AmazonS3ReadOnlyAccess
```

We use:

```
s3:GetObject
s3:ListBucket
Scoped to specific bucket only
```

---

### ✔ Secrets Protection

* No hardcoded credentials in code
* Secrets stored in:

  * AWS SSM Parameter Store
  * AWS Secrets Manager
* Encrypted using AWS KMS

---

## 📦 Resources Created

| Resource               | Purpose                      |
| ---------------------- | ---------------------------- |
| IAM Role (EC2)         | EC2 instance permissions     |
| IAM Role (Lambda)      | Lambda execution permissions |
| IAM Role (ECS)         | Container task permissions   |
| SSM Parameter          | Secure configuration storage |
| Secrets Manager Secret | Application secrets storage  |
| KMS Key                | Encryption of sensitive data |

---

## 🧪 Validation Checklist (Lab Submission)

* [x] EC2 IAM Role created
* [x] Lambda IAM Role created
* [x] ECS IAM Role created
* [x] Parameter Store entry created
* [x] Secrets Manager secret created
* [x] GitSync deployment successful
* [x] CloudFormation stack created successfully



## 🧠 Key Learnings

* IAM Roles are preferred over IAM Users for workloads
* Secrets must never be hardcoded in applications
* Least privilege reduces attack surface significantly
* CloudFormation enables secure and repeatable infrastructure
* GitSync automates infrastructure deployment from Git repositories

---

## 🔮 Future Improvements

* Add IAM Permission Boundaries
* Add VPC endpoint restrictions (Level 4)
* Enable automatic Secrets rotation
* Integrate AWS CodeGuru for secret scanning
* Add CloudTrail monitoring for IAM activity

