---
title: "Event 3"
date: "2025-09-10"
weight: 2
chapter: false
pre: " <b> 4.3. </b> "
---

# “DevOps on AWS” Report

### Event Purpose

- Guide career paths in DevOps and Cloud fields.
- Deeply understand CI/CD processes and Containerization.
- Analyze the role of Infrastructure as Code (IaC) versus ClickOps.
- Compare Orchestration solutions on AWS (ECS vs EKS) and Monitoring/Observability strategies.

### Highlights

#### Next-Generation DevOps Roadmap

- **Related roles**: DevOps Engineer, Cloud Engineer, Platform Engineer, Site Reliability Engineer (SRE).
- **T-shaped Skill**: A skill development model with broad knowledge across many areas and deep knowledge in one specific area.
- **Advice for beginners**:
    - **Do**: Start with Fundamentals, learn through Real Projects, Document everything, focus on mastering one skill at a time.
    - **Don't**: Get stuck in "Tutorial Hell", copy-paste blindly, compare yourself to others, give up after failures.

#### Continuous Integration & Deployment (CI/CD)

- **CI (Continuous Integration)**: The process of frequently integrating code into a shared repository.
- **Distinguishing CD**:
    - **Continuous Delivery**: Automated up to Acceptance Test; deployment to Production requires a manual trigger.
    - **Continuous Deployment**: Fully automated from code to Production.

#### Infrastructure as Code (IaC)

- **Problems with ClickOps**: Slow, prone to Human Error, inconsistent, difficult to collaborate.
- **Benefits of IaC**: Automation, Scalability, Reproducibility, enhanced collaboration.
- **AWS CloudFormation**:
    - AWS's built-in IaC tool using JSON/YAML templates.
    - **Stack**: A collection of resources managed together.
    - **Drift Detection**: Detects discrepancies between actual configuration and the template (when someone manually modifies resources).
- **AWS CDK (Cloud Development Kit)**:
    - Uses programming languages (Python, TS...) to define infrastructure.
    - Concepts: L1 (Mapping 1:1), L2, L3 Constructs.
    - CLI commands: **cdk init**, **cdk synth**, **cdk deploy**, **cdk diff,** **cdk destroy**.
- **Terraform**: Open-source tool, supports Multi-cloud, uses HCL language, manages state via State file.

#### Container Ecosystem

- **Docker Fundamentals**: Dockerfile (build definition) → Image (packaging blueprint) → Container (runtime).
- **Amazon ECR**: AWS's private container registry, supporting image scanning, immutable tags and lifecycle policies.
- **Container Orchestration**: Manages container lifecycle (restart, scale, distribute traffic).
    - **Amazon ECS**: AWS native solution. Supports Launch types: EC2 (server management) and Fargate (Serverless - easier).
    - **Amazon EKS**: Managed Kubernetes service. Suitable for complex systems requiring K8s experience.
    - **Amazon App Runner**: Simplest solution to deploy web apps/APIs quickly and cost-effectively.

#### Monitoring & Observability

- **Distinction**:
    - **Monitoring**: Tracking Logs, Metrics (How is the system performing?).
    - **Observability**: Deeply understanding root causes (Why is the system performing this way?).
- **Amazon CloudWatch**:
    - Collects Metrics (CPU, RAM, Network...) and Logs in real-time.
    - **Alarms**: Alerts and automated responses.
    - **Dashboards**: Visualize operational data.
- **AWS X-Ray**: Distributed tracing for microservices, helping visualize service maps and analyze performance bottlenecks.

### What I Learned

#### System Thinking

- **Infrastructure Drift**: Understanding the risks of manual resource modification outside of IaC and the importance of using Drift Detection.
- **Trade-offs**: Knowing how to select IaC tools (CDK for AWS-centric, Terraform for Multi-cloud) and Orchestration tools (ECS for beginners/simple needs, EKS for advanced features).

#### Operational Skills

- **Standard Process**: The core difference between Continuous Delivery and Continuous Deployment lies in the manual approval step to Production.
- **Container Management**: Understanding the workflow from Dockerfile to ECR and how ECS/EKS orchestrates container operations.

### Application to Work

- **Transition to IaC**: Start writing CloudFormation or CDK for current projects instead of operating on the Console.
- **Optimize Pipeline**: Review CI/CD processes, integrate automated testing steps before deployment.
- **Implement Observability**: Integrate AWS X-Ray into applications to trace requests across microservices, combined with CloudWatch Alarms for proactive monitoring.
- **Refactor Docker**: Optimize Dockerfiles and use ECR Lifecycle Policies to manage image storage space.

### Event Experience

Participating in the **“Next-Generation DevOps & Cloud Architecture”** event was a crucial stepping stone helping me clearly define my DevOps skill development path.

#### Clear Orientation

- The speaker outlined a very practical learning Roadmap with the advice "Don't stay in Tutorial Hell" - which resonated with me.
- The **T-shaped skill** model helped me realize I don't need to know everything at once but should focus deeply on one area first.

#### In-depth Tool Knowledge

- The detailed comparison between **ECS and EKS** made me more confident in choosing the right compute solution for my project (as a beginner, ECS Fargate is the optimal choice).
- The presentation on **IaC and Drift Detection** completely changed my mindset on infrastructure management: "Use code, not clicks".

#### The Importance of Observability

- I realized that Monitoring (looking at charts) is not enough; achieving **Observability** (understanding the nature) through tools like AWS X-Ray is necessary to thoroughly resolve issues in distributed systems.

#### Some photos from the event

- Add your photos here
  > This event not only provided technical knowledge but also inspired a professional mindset, from building CI/CD culture to automating everything possible.