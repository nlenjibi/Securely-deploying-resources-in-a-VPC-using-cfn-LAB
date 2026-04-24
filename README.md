```md
# 📘 BEM11 – Securely Deploying Resources in a VPC using CloudFormation

> **Module:** BEM11 – Deep Dive into Networking on AWS  
> **Lab:** Securely Deploying Resources in a VPC using CloudFormation  

---

## 1. Architecture Overview

### 🔹 Solution Summary
This project deploys a **highly available and secure VPC architecture** using CloudFormation.

- 1 VPC (10.0.0.0/16)
- 2 Availability Zones
- 2 Public Subnets (Web Tier)
- 2 Private Subnets (Application Tier)
- 2 NAT Gateways (one per AZ)
- 4 EC2 Instances:
  - 2 Public (Apache Web Servers)
  - 2 Private (Application Instances)
- Internet Gateway for public access
- **AWS Systems Manager (Session Manager) for access (no SSH)**

---

### 🔹 Architecture Layout

```

Region
└── VPC (10.0.0.0/16)
│
├── AZ1                          AZ2
│   ├── Public Subnet            Public Subnet
│   │   ├── Web EC2              Web EC2
│   │   └── NAT Gateway          NAT Gateway
│   │
│   └── Private Subnet           Private Subnet
│       └── App EC2              App EC2
│
└── Internet Gateway

```

---

### 🔹 Traffic Flow

| Source | Destination | Path |
|--------|------------|------|
| Internet | Public EC2 | IGW → Public Subnet |
| Public EC2 | Internet | IGW |
| Private EC2 | Internet | NAT Gateway → IGW |
| Public ↔ Private | Internal | VPC routing |

---

### 🔹 Key Requirements Covered

- ✅ Multi-AZ deployment  
- ✅ Public and private subnet isolation  
- ✅ NAT Gateway per AZ (no cross-AZ dependency)  
- ✅ Apache installed via User Data  
- ✅ No SSH access  
- ✅ Session Manager for all access  
- ✅ Least-privilege security groups  

---

## 2. Prerequisites

- AWS Account  
- AWS CLI installed and configured (`aws configure`)  
- IAM permissions (CloudFormation, EC2, VPC, IAM)  
- Session Manager enabled (default for Amazon Linux)

---

## 3. Repository Structure

```

.
├── vpc-lab.yaml
├── README.md
└── docs/

````

---

## 4. Parameters

| Parameter | Description |
|----------|------------|
| StudentName | Your name (shown on webpage) |
| LabName | Lab title |
| InstanceType | EC2 instance type |
| AMI | Latest Amazon Linux (auto-resolved) |

---

## 5. Deployment Guide

### Option A – AWS Console
1. Go to **CloudFormation → Create Stack**  
2. Upload `vpc-lab.yaml`  
3. Enter stack name  
4. Set `StudentName`  
5. Deploy  

---

### Option B – AWS CLI

```bash
aws cloudformation create-stack \
  --stack-name bem11-vpc-lab \
  --template-body file://vpc-lab.yaml \
  --capabilities CAPABILITY_NAMED_IAM \
  --parameters ParameterKey=StudentName,ParameterValue="Your Name"
````

---

## 6. Validation Checklist (Live Demo)

### ✅ 1. Web Servers Accessible

Open:

```
http://<Public-DNS-1>
http://<Public-DNS-2>
```

✔ Displays your name and lab

---

### ✅ 2. Public → Private Connectivity

Using Session Manager:

```bash
ping <private-ip>
```

✔ Should succeed

---

### ✅ 3. Private → Internet (NAT Test)

```bash
ping 8.8.8.8
curl google.com
```

✔ Confirms outbound internet via NAT Gateway

---

### ✅ 4. No SSH Access

* Port 22 is not open
* Only Session Manager works

---

## 7. Session Manager Usage

### Connect via Console

EC2 → Instance → Connect → Session Manager

### Connect via CLI

```bash
aws ssm start-session --target <instance-id>
```

---

## 8. Architectural Knowledge (Q&A)

### 🔹 What is a NAT Gateway?

A managed AWS service that allows **private instances to access the internet securely** without allowing inbound traffic.

---

### 🔹 Why use one NAT Gateway per AZ?

* Improves availability
* Avoids single point of failure
* Eliminates cross-AZ traffic costs

---

### 🔹 What is a Regional NAT Gateway?

* A single NAT serving multiple AZs
* Simpler but less fault isolation
* Can replace multiple NATs but not ideal for high availability

---

## 9. Design Decisions

| Decision             | Reason                            |
| -------------------- | --------------------------------- |
| Multi-AZ             | High availability                 |
| Private subnets      | Secure internal resources         |
| NAT per AZ           | Fault tolerance                   |
| No SSH               | Secure access via Session Manager |
| Apache via User Data | Automated setup                   |

---

## 10. Cost & Cleanup

### 💰 Cost Considerations

* NAT Gateways are the main cost
* EC2 instances also contribute

---

### 🧹 Cleanup

```bash
aws cloudformation delete-stack --stack-name bem11-vpc-lab
```

