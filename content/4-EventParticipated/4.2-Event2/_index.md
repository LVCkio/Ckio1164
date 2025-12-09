---
title: "Event 2"
date: "2025-09-10"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}}

# Event Summary Report: “Monitoring & Observability on AWS Workshop”

#### 1. Event Objectives

This workshop helped me gain a better understanding of:

- The difference between **Monitoring** and **Observability**
- AWS services used for system monitoring and visibility
- How to use **Amazon CloudWatch** to track logs, metrics, alarms, and dashboards
- An introduction to **AWS X-Ray** and how it helps analyze microservices performance

---

#### 2. Introduction to Monitoring and Observability

#### Monitoring

- Monitoring is the process of **tracking the system** through logs, metrics, events, etc.
- It shows the **current state** of the system in real time.
- Useful for DevOps & SRE teams to detect issues early.

#### Observability

- Observability goes beyond monitoring.
- It helps us **understand why** a problem happened, not just what happened.
- Focuses on answering: *“What is happening inside the system and why?”*

#### Monitoring & Observability on AWS

The workshop introduced three main AWS tools:

- **Amazon CloudWatch** – logs, metrics, alarms, dashboards
- **Amazon Managed Grafana** – advanced visualization and analytics
- **AWS X-Ray** – distributed tracing for microservices

---

#### 3. Amazon CloudWatch

#### 3.1 CloudWatch Overview

I learned that CloudWatch is not just a monitoring tool, but a full **observability platform**:

- Monitors resources and applications in **real time**
- Collects **metrics and logs** for deeper analysis
- Supports **alarms and automated responses**, helping DevOps & SRE workflows
- Provides dashboards for operational insights and cost optimization

---

#### 3.2 CloudWatch Metrics

- A metric is data that represents the **performance of the system** (CPU, RAM, latency, error rate, etc.)
- CloudWatch collects default AWS metrics automatically
- We can create **custom metrics** or collect data from on-premises systems using the CloudWatch Agent
- Metrics integrate easily with other AWS services (Lambda, ECS, API Gateway, etc.)

---

#### 3.3 CloudWatch Logs

- Stores and analyzes application logs
- Allows searching, filtering, and creating log-based metrics
- Helps detect errors, track user behavior, and debug applications

---

#### 3.4 CloudWatch Alarms

- Used to trigger alerts based on metric thresholds
- When triggered, alarms can:

  - Send notifications via SNS
  - Automatically scale resources
  - Trigger Lambda functions to handle incidents

---

#### 3.5 CloudWatch Dashboards

- Provides a visual view of system health
- Supports custom charts and graphs
- Useful for DevOps teams to monitor systems at a glance

---

#### 4. AWS X-Ray

AWS X-Ray was introduced as a powerful tool for observing and debugging **microservices-based systems**.

#### 4.1 Distributed Tracing

- Tracks **an entire request path** end-to-end
- Generates **service maps** to visualize how microservices interact
- Requires integrating the X-Ray SDK to produce trace IDs

#### 4.2 Performance Analysis

- Helps identify:

  - Performance bottlenecks
  - Slow services
  - Failures inside microservice calls

#### 4.3 Integration with CloudWatch

- X-Ray traces can be combined with CloudWatch metrics and logs
- Provides a more complete picture of system behavior
- Helps developers fix issues faster

---

#### 5. Key Takeaways

- Monitoring and observability are different—observability helps understand *why* issues happen
- CloudWatch is very powerful because it supports **metrics → logs → alarms → dashboards**
- X-Ray is extremely useful for monitoring microservices and request flows
- Combining CloudWatch and X-Ray creates a strong observability stack for any application

---

#### 6. Applications to Work and Study

- I can use CloudWatch to monitor my AWS lab projects
- For microservices applications, I want to try integrating X-Ray to visualize request flows
- Dashboards can help me present system status in reports
- Alarms allow automated detection of issues without manual monitoring

---

#### 7. My Experience at the Event

- I learned practical system monitoring techniques that I had only seen in theory before
- The demos of metrics, alarms, and X-Ray made everything easier to understand
- This knowledge is very useful if I want to pursue DevOps or Cloud Engineering
- The workshop helped broaden my understanding of system observability


