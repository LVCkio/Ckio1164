---
title: "Event 1"
date: "2025-09-10"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


#### Báo cáo Tóm tắt Sự kiện: “GenAI & Bedrock AI Services Workshop"

#### Mục tiêu của sự kiện

Buổi workshop giúp em hiểu rõ hơn về:

- Cách các **Foundation Models** được xây dựng và hoạt động
- Những kỹ thuật **Prompt Engineering** thường dùng
- Kiến trúc **Retrieval-Augmented Generation (RAG)** - mô hình AI phổ biến hiện nay
- Các dịch vụ AI có sẵn của AWS
- Cách xây dựng **AI Agent** bằng Amazon Bedrock AgentCore

---

## **2. Diễn giả**

- **Lâm Tuấn Kiệt** – FPT Software (từng làm việc trong lĩnh vực ngân hàng)
- **Danh Hoành Hiếu Nghị** – Chuyên gia Bedrock AgentCore
- **Đinh Lê Hoàng Anh** – AI/ML Specialist

---

#### Những nội dung quan trọng em học được

#### Foundation Models

- Các mô hình nền được train bằng **self-supervised learning**, tức là học từ lượng dữ liệu rất lớn mà không cần gán nhãn.
- Một mô hình có thể xử lý nhiều nhiệm vụ khác nhau.
- Trên Amazon Bedrock có những mô hình mạnh như **Titan, Luma, DeepSeek,…**

#### Prompt Engineering

Em hiểu được rằng viết prompt đúng cách giúp mô hình trả lời tốt hơn rất nhiều.

#### Các kỹ thuật chính:

- **Zero-shot prompting**: chỉ đưa câu hỏi, không cần ví dụ.
- **Few-shot prompting**: đưa một vài ví dụ mẫu → mô hình trả lời theo đúng format.
* **Chain of Thought**: mô hình được hướng dẫn suy luận từng bước, giúp trả lời logic và chính xác hơn.

Em thấy Chain of Thought rất hay, vì giúp mô hình giải thích lý do tại sao đưa ra đáp án.

---

#### Retrieval-Augmented Generation (RAG)

Đây là phần em thích nhất vì được ứng dụng rất nhiều trong thực tế (đặc biệt là ngân hàng).

#### Kiến trúc RAG gồm 3 bước:

1. **Retrieval** – tìm thông tin liên quan
2. **Augmentation** – thêm thông tin này vào prompt
3. **Generation** – dùng LLM tạo câu trả lời

#### Embedding

- Chuyển nội dung thành vector để tìm kiếm theo ngữ nghĩa
- Amazon Titan Embedding hỗ trợ nhiều ngôn ngữ

#### Cách RAG hoạt động

1. Tài liệu → chia nhỏ → tạo embedding → lưu vào vector store
2. Khi người dùng đặt câu hỏi:

   - Câu hỏi cũng được đưa vào embedding
   - Tìm thông tin phù hợp
   - Ghép vào prompt
   - Mô hình tạo câu trả lời chính xác hơn

Em nhận ra rằng **vector store là phần cực kỳ quan trọng** của RAG.

---

#### Các dịch vụ AI có sẵn trên AWS

Em được giới thiệu nhiều dịch vụ thú vị:

#### Amazon Rekognition

- Phân tích ảnh và video
- Nhận diện khuôn mặt, vật thể
- Dùng nhiều trong camera an ninh

#### Amazon Translate

- Dịch văn bản theo thời gian thực hoặc theo tài liệu

#### Amazon Textract

 Trích xuất văn bản từ hóa đơn, giấy tờ

#### Amazon Polly

- Chuyển văn bản thành giọng nói tự nhiên

#### Amazon Comprehend

- Phân tích ngôn ngữ: sentiment, PII, key phrase,…

#### Amazon Kendra

- Công cụ tìm kiếm thông minh, hỗ trợ semantic search và RAG

#### Amazon Personalize

- Gợi ý cá nhân hóa như hệ thống của Netflix hoặc Shopee

- Ngoài ra còn có bộ Lookout, Transcribe và framework Pipecat cho ứng dụng real-time AI.

---

#### Amazon Bedrock AgentCore

- Phần này do anh **Nghị** trình bày, giúp em hiểu cách tạo một AI Agent hoàn chỉnh.

#### Hệ sinh thái hỗ trợ:

* LangGraph
* LangChain
* Strands Agent SDK

#### Quy trình xây dựng Agent

- Từ **ý tưởng → phát triển → triển khai thực tế**
Trong đó cần chú ý đến:

* Performance
* Scalability
* Security
* Governance

#### Các thành phần của AgentCore

* Runtime
* Memory
* Identity
* Gateway
* Code Interpreter
* Browser Tool
* Observability

Em thấy AgentCore rất mạnh vì giúp xây dựng AI agent chạy ổn định trong môi trường doanh nghiệp.

---

#### Những điều em rút ra được

- Foundation Models có khả năng xử lý đa nhiệm rất mạnh
- Viết prompt đúng cách quan trọng hơn em nghĩ
- RAG là giải pháp lý tưởng để xây chatbot nội bộ vì đảm bảo thông tin chính xác
- Các dịch vụ AI của AWS giúp tiết kiệm thời gian xây dựng hệ thống từ đầu
- AgentCore hỗ trợ triển khai AI agent ở quy mô lớn

---

# **5. Ứng dụng vào công việc và học tập**

- Em có thể áp dụng **Few-shot + CoT** khi làm bài tập hoặc lập trình AI
- Em muốn thử xây **RAG chatbot** để tìm tài liệu trong lớp hoặc công ty
- Textract + Comprehend có thể dùng để tự động hóa xử lý tài liệu
- AgentCore rất phù hợp để xây một trợ lý AI hỗ trợ quy trình làm việc

---

#### Trải nghiệm của em tại sự kiện

- Em học được nhiều kiến thức AI thực tế mà trước đây chỉ đọc qua lý thuyết
- Các anh diễn giả giải thích dễ hiểu, có ví dụ rõ ràng
- Em hiểu cách mà các doanh nghiệp lớn triển khai AI phục vụ khách hàng
- Đây là buổi workshop giúp em mở rộng tư duy và định hướng nghề nghiệp trong AI

---

#### Hình ảnh sự kiện




