---
title: "Event 1"
date: "2025-09-10"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}}


#### Event Summary Report: “GenAI & Bedrock AI Services Workshop”

#### Event Objectives

This workshop helped me gain a clearer understanding of:

- How **Foundation Models** are built and how they work
- Common **Prompt Engineering** techniques
- The architecture of **Retrieval-Augmented Generation (RAG)** – one of today’s most widely used AI approaches
- AWS pretrained AI services
- How to build an **AI Agent** using Amazon Bedrock AgentCore

---

#### Speakers

- **Lam Tuan Kiet** – FPT Software (previously worked in the banking industry)
- **Danh Hoanh Hieu Nghi** – Bedrock AgentCore Specialist
- **Dinh Le Hoang Anh** – AI/ML Specialist

---

#### Key Insights I Learned

#### Foundation Models

- Foundation Models are trained using **self-supervised learning**, meaning they learn from massive amounts of unlabeled data.
- One model can perform many different tasks.
- Amazon Bedrock provides powerful models such as **Titan, Luma, DeepSeek, and more**.

---

#### Prompt Engineering

I learned that writing prompts properly can significantly improve the model’s responses.

#### Main Techniques:

- **Zero-shot prompting**: asking a question without giving examples.
- **Few-shot prompting**: providing example outputs so the model can follow a specific format.
- **Chain of Thought (CoT)**: guiding the model to reason step by step, helping it produce more logical and accurate answers.

I found Chain of Thought especially interesting because it allows the model to explain how it arrived at its answer.

---

#### Retrieval-Augmented Generation (RAG)

This was my favorite part because it is widely used in real-world applications, especially in banking.

#### RAG consists of 3 main steps:

1. **Retrieval** – finding relevant information
2. **Augmentation** – adding this information into the prompt
3. **Generation** – producing the answer using a Large Language Model (LLM)

---

#### Embedding

- Converts text into vectors for semantic search
- Amazon Titan Embedding supports many languages

---

#### How RAG Works

1. Documents → chunking → embedding → vector store
2. When a user asks a question:

   - The question is embedded
   - Similar information is retrieved from the vector store
   - The context is added to the prompt
   * The model generates a more accurate answer

I realized that **the vector store is a crucial part** of the entire RAG pipeline.

---

#### AWS AI Services Introduced in the Workshop

#### Amazon Rekognition

- Image and video analysis
- Face and object detection
- Used in security and camera systems

#### Amazon Translate

- Real-time and batch text translation

#### Amazon Textract

- Extracts text from invoices, forms, and documents

#### Amazon Polly

- Converts text into natural-sounding speech

#### Amazon Comprehend

- Language analysis: sentiment, key phrases, PII detection, etc.

#### Amazon Kendra

- Intelligent search engine with semantic search and RAG support

#### Amazon Personalize

- Personalized recommendations (similar to Netflix or Shopee)

Additional tools include the **Lookout** services, **Transcribe**, and **Pipecat** for real-time AI interactions.

---

#### Amazon Bedrock AgentCore

This part was presented by **Mr. Nghi**, and it helped me understand how to create a complete AI agent.

#### Supported Ecosystem

- LangGraph
- LangChain
- Strands Agent SDK

#### Agent Development Workflow

From **idea → development → real-world deployment**
With a focus on:

- Performance
- Scalability
- Security
- Governance

#### AgentCore Components

- Runtime
- Memory
- Identity
- Gateway
- Code Interpreter
- Browser Tool
- Observability

I think AgentCore is very powerful because it allows AI agents to run reliably in enterprise environments.

---

#### Key Takeaways

- Foundation Models are extremely capable of handling multiple tasks
- Prompt engineering is more important than I expected
- RAG is ideal for building internal chatbots since it ensures factual and accurate answers
- AWS AI services help reduce development time significantly
- AgentCore supports building production-level AI agents

---

#### Applications to My Work and Studies

- I can apply **Few-shot + CoT** when solving assignments or building AI applications
- I want to try building a **RAG chatbot** to search documents for my class or workplace
- Textract + Comprehend can help automate document processing tasks
- AgentCore seems perfect for building an AI assistant for workflow automation

---

#### My Experience at the Workshop

- I learned many practical AI concepts that I previously only saw in theory
- The speakers explained everything clearly with real examples
- I now understand how large companies implement AI for their customers
- This workshop helped broaden my thinking and guide my future career direction in AI

---

#### Event Photos


