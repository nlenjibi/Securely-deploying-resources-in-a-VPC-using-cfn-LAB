Here is a **clean, submission-ready README.md** for your **BEM11 Security Baseline Lab – Level 4 (Least Privilege + VPC Endpoints + IaC + GitSync)**.

---

# 📘 BEM11 Security Baseline Lab – Level 4

## 🔐 Overview

This project implements AWS Security Baseline controls using **Infrastructure as Code (IaC)** with AWS CloudFormation and Git-based deployment (GitSync).

It demonstrates **least privilege security design**, **resource-based policies**, and **secure network access using VPC endpoints**.

The goal is to enforce strong security controls across identity, storage, and network layers.

---

## 🎯 Objectives

* Implement **least privilege IAM access**
* Restrict S3 access using **resource-based policies**
* Deploy infrastructure using **CloudFormation templates**
* Use **VPC Gateway and Interface Endpoints**
* Enforce secure private access to AWS services
* Automate deployment using GitSync CI/CD

---

## 🏗️ Architecture Components

This lab deploys the following resources:

### 🧑‍💻 Identity & Access

* IAM User (restricted access)
* IAM Role for EC2 (least privilege permissions)

### 🪣 Storage

* S3 Restricted Bucket
* S3 Unrestricted Bucket
* Bucket policy enforcing:

  * Explicit allow for one IAM user
  * Explicit deny for all others

### 🌐 Network

* VPC
* Private Subnet
* Route Table
* Security Group

### 🔌 VPC Endpoints

* Gateway Endpoint for S3
* Interface Endpoint for S3
* Endpoint policies restricting bucket access

### 🖥️ Compute

* EC2 instance in private subnet
* Session Manager access enabled via SSM

---

## 📂 Repository Structure

```
.
├── security-baseline-level4.yaml   # CloudFormation template
├── git-sync-config.yaml            # Deployment configuration
└── README.md                       # Documentation
```

---

## 🚀 Deployment (GitSync)

The stack is deployed using GitSync with CloudFormation.

### Deployment File

* `git-sync-config.yaml`

### Stack Name

```
bem11-security-baseline-level4-lab
```

### Region

```
us-east-1
```

---

## 🔐 Security Controls Implemented

### 1. Least Privilege IAM

* IAM user restricted to S3 PutObject only
* EC2 role limited to required S3 read permissions
* No wildcard administrative access

### 2. Resource-Based Policy (S3)

* Only a specific IAM user can upload objects
* Explicit deny for all other principals

### 3. VPC Endpoint Security

* Gateway endpoint restricts S3 access
* Interface endpoint enforces private connectivity
* Endpoint policies block restricted bucket access

### 4. Network Isolation

* EC2 deployed in private subnet
* No direct internet access
* S3 access only via VPC endpoints

---

## 🧪 Testing Scenarios

### S3 Access Testing

* ❌ Unauthorized IAM user → Access Denied
* ✅ Authorized IAM user → Upload successful

### VPC Gateway Endpoint

* ✅ Unrestricted bucket access works
* ❌ Restricted bucket access denied

### VPC Interface Endpoint

* ✅ Unrestricted bucket access works
* ❌ Restricted bucket access denied

---


## 🔁 Clean Up Steps

After testing:

* Delete VPC endpoints
* Delete S3 buckets and objects
* Terminate EC2 instance
* Delete IAM users and roles (if required)

---

## 🧠 Key Learning Outcomes

* Understanding of **least privilege security design**
* Practical use of **IAM policies and resource-based policies**
* Secure S3 access using **VPC endpoints**
* Infrastructure automation using **CloudFormation**
* Secure CI/CD deployment using GitSync

---

