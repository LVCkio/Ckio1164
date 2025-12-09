---
title: "Translated Blogs"
date: "2025-09-10"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

This section lists and briefly introduces the blogs that I translated during the internship.

### [Blog 1 – Getting started with Amazon Nova Canvas: Virtual Try-On & Style Options](3.1-Blog1/)

This blog introduces new features of **Amazon Nova Canvas** on Amazon Bedrock, focusing on **virtual try-on** and **style options**. The article explains how users can upload a person image and a garment image, then use the `VIRTUAL_TRY_ON` task type to generate a realistic output where the person is wearing the selected clothing. It also describes masking techniques (`GARMENT`, `PROMPT`, `IMAGE`), how to work with Base64 images, and use the Bedrock Runtime API to generate results programmatically.  
In the second part, the blog explores **pre-trained artistic styles** such as 3D animated family film, photorealism, graphic novel, and more. By simply changing the `style` parameter in the request, users can generate different visual styles from the same prompt. The article also summarizes important notes such as supported Regions, pricing, and where to start with the Nova Canvas User Guide.

### [Blog 2 – Building Serverless Event-Driven Applications with AWS Lambda and Amazon MSK](3.2-Blog2/)

This blog explains **why using AWS Lambda together with Amazon Managed Streaming for Apache Kafka (Amazon MSK)** is a strong choice for event-driven architectures. It highlights key benefits such as:  
- **Native integration:** using event source mappings so Lambda can consume messages from Kafka topics without custom consumer code.  
- **Auto scaling & throughput control:** one poller per partition, provisioned concurrency, and provisioned event source mapping for high-throughput workloads.  
- **Cost-effectiveness:** pay-per-use compute model with no cost for idle time, and the ability to tune batch size and batch window.  

The article also walks through how **Lambda processes messages from MSK**, including offset management, retries, dead-letter queues, and consumer group behavior. A step-by-step **walkthrough** shows how to:  
1. Create an MSK cluster and topic,  
2. Create a Lambda function with the right IAM permissions,  
3. Configure event source mapping (network, parameters, permissions).  

Finally, it discusses **optimizations for stream processing**, such as using provisioned concurrency, tuning event pollers, optimizing batching, and applying event filtering to reduce unnecessary invocations. The conclusion emphasizes that combining MSK and Lambda helps teams build modern serverless streaming applications without managing consumer infrastructure.

### [Blog 3 – Advancing Inter-Agent Communication with the Model Context Protocol (MCP)](3.3-Blog3/)

This blog focuses on **open standards for agentic AI**, especially the **Model Context Protocol (MCP)** and how it enables **agent-to-agent communication**. It explains why AWS supports multiple open protocols (MCP, A2A) and joins the MCP steering committee to promote interoperability in the agent ecosystem.  
The article describes how MCP was originally designed for tool integration, but its architecture (streamable HTTP, capability discovery, authentication, resource sharing, sampling) also makes it suitable for **agent collaboration**. It breaks down key concepts such as MCP servers, clients, tools/agent skills, and how agents can communicate over MCP.

A technical example using **Java + Spring AI** shows how to:  
- Build an Employee Info Agent that calls MCP tools.  
- Expose this agent as an MCP server tool.  
- Integrate it with another HR Agent through MCP, enabling inter-agent queries over HTTP.  

The blog then outlines AWS’s contributions to MCP evolution, including proposals for **human-in-the-loop interactions, streaming partial results, enhanced capability discovery, and asynchronous communication**. It ends by highlighting the enthusiasm from partners (Confluent, CrewAI, Elastic, IBM, LangChain, LlamaIndex, Writer, Broadcom, etc.) and encourages developers to explore MCP docs, GitHub discussions, and AWS guides to start building interconnected agentic applications.
