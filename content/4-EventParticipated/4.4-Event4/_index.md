---
title: "Event 4"
date: "2025-09-10"
weight: 3
chapter: false
pre: " <b> 4.4. </b> "
---

# “AWS Security Governance & Automation” Report

### Event Purpose

- Introduce the vision of Cloud Club and the importance of community.
- Share methods for centralized Identity Management and account security.
- Guide on Multi-Layer Security Visibility strategy.
- Automate security incident response process (Automated Alerting) with EventBridge.

### Highlights

#### Identity & Access Management (IAM & Governance)

- **Single Sign-On (SSO)**: "One login, multiple systems" mechanism. Instead of creating many scattered IAM Users, SSO allows centralized identity management, helping users log in once to access multiple AWS accounts/applications.
- **Service Control Policies (SCPs)**: A type of Organizational Policy used to set up "guardrails". SCPs limit maximum permissions for member accounts in AWS Organization.
- **Credentials Spectrum**:
    - **Long-term**: IAM User Access Keys (do not expire) → High risk, need to limit usage.
    - **Short-term**: IAM Roles, STS tokens (expire after 15 minutes - 36 hours) → Higher security, is best practice.
- **MFA (Multi-Factor Authentication)**: Mandatory security layer for every account.

#### Multi-Layer Security Visibility

- **IAM Access Analyzer**: Tool helping detect resources (S3, KMS, IAM Roles...) being publicly shared or shared with untrusted accounts.
- **Event Classification (Logging)**:
    - **Management Events**: Who did what to resources? (Ex: Create EC2, Delete S3 Bucket).
    - **Data Events**: Who accessed the data? (Ex: GetObject S3, Invoke Lambda).
    - **Network Activity Events**: Monitor VPC network traffic.

#### Automation & Alerting

- **Amazon EventBridge**: Real-time Event processing center.
    - Supports **Cross-account Event**: Receive events from child accounts to the central security account.
    - **Automated Alerting**: Automatically send alerts when abnormal behavior is detected.
- **Detection as Code**:
    - Convert threat detection logic into code (Infrastructure as Code).
    - Use **CloudTrail Lake queries** to query history.
    - Version control for security rules.

#### Network Security

- **Common Attack Vectors**: Common network attack vectors (DDoS, SQL Injection, Man-in-the-middle...).
- **AWS Layered Security**: Defense in Depth strategy - protecting from the outer layer (Edge), network layer (VPC), to application and data layers.

### What I Learned

#### Modern Security Mindset

- **Identity is the new perimeter**: In Cloud environment, identity management (IAM/SSO) is more important than traditional firewalls.
- **Zero Trust**: Trust no one, always authenticate (MFA) and grant least privilege.

#### Governance Strategy

- **Shift from Long-term to Short-term**: Understand clearly the risks of long-term Access Keys and the importance of shifting to Temporary Credentials.
- **Governance at Scale**: Use SCPs to manage hundreds of AWS accounts consistently instead of manual configuration for each.

#### Automation Techniques

- **Event-Driven Security**: Instead of manual log review (passive), use EventBridge to react immediately (active) when security incidents occur.

### Application to Work

- **Review IAM**: Check and disable old IAM Access Keys, enforce MFA for the whole team.
- **Deploy Access Analyzer**: Activate immediately to scan if any S3 bucket is mistakenly public.
- **Set up alerts**: Create simple EventBridge rules to send notifications to Slack/Email when someone logs in with Root account or changes Security Group.
- **Learn about CloudTrail**: Configure CloudTrail to record both Management and Data events for critical resources.

### Event Experience

The 3rd event brought a very deep perspective on **Security & Governance** aspects - an area often overlooked in development but vital for businesses.

#### Risk Awareness

- The presentation on **Credentials Spectrum** startled me into realizing how dangerous the habit of using IAM Users with Long-term keys is. Shifting to Short-term credentials is mandatory.

#### The Power of Automation

- I was very impressed with the concept of **"Detection as Code"**. Managing security rules like source code helps operations become transparent and easier to control.
- The **EventBridge** demo showed excellent real-time response capabilities, minimizing the time attackers can cause harm in the system.

#### Multi-layered Defense Mindset

- Understood better about **AWS Layered Security**, security is not just a door but multiple barrier layers coordinating from Network to Identity.

  > This event changed my mindset from "Make it run" to "Make it safe and compliant". Security is not a barrier, but a foundation for sustainable development.