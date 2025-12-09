---
title: "Các bài blogs đã dịch"
date: 2025-09-10
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}

Phần này liệt kê và giới thiệu ngắn gọn các blog đã được dịch trong quá trình thực tập.

---

### [Blog 1 – Bắt đầu với Amazon Nova Canvas: Virtual Try-On & Style Options](3.1-Blog1/)

Blog này giới thiệu các tính năng mới của **Amazon Nova Canvas** trên Amazon Bedrock, tập trung vào **virtual try-on (thử đồ ảo)** và **style options (tùy chọn phong cách)**. Bài viết giải thích cách người dùng có thể tải lên ảnh người + ảnh quần áo, sau đó sử dụng `VIRTUAL_TRY_ON` để tạo ra hình ảnh người mẫu mặc trang phục một cách chân thực.  
Blog cũng mô tả kỹ thuật **masking** với các kiểu như `GARMENT`, `PROMPT`, `IMAGE`, cách chuyển đổi ảnh sang Base64, và cách gọi API Bedrock Runtime để sinh ảnh bằng mã Python.

Phần thứ hai nói về **các phong cách nghệ thuật được huấn luyện sẵn** như hoạt hình 3D, photorealism, graphic novel,… Chỉ cần thay giá trị `style` trong tham số API, người dùng có thể tạo ra ảnh theo nhiều phong cách khác nhau mà không cần viết prompt phức tạp.  
Bài viết kết thúc với thông tin về vùng hỗ trợ, giá, và hướng dẫn sử dụng Nova Canvas.

---

### [Blog 2 – Xây dựng ứng dụng serverless theo sự kiện với AWS Lambda và Amazon MSK](3.2-Blog2/)

Blog này giải thích lý do **kết hợp AWS Lambda với Amazon MSK** mang lại lợi ích lớn cho kiến trúc event-driven. Một số điểm nổi bật bao gồm:

- **Tích hợp tự nhiên:** Lambda có thể nhận dữ liệu từ Kafka topic thông qua event source mapping mà không cần tự viết consumer.
- **Tự động mở rộng:** Lambda gán poller theo partition, đảm bảo xử lý song song và tự mở rộng theo tải.
- **Tiết kiệm chi phí:** Mô hình trả tiền theo thời gian chạy, không tốn chi phí khi không có sự kiện.

Blog cũng mô tả cách **Lambda xử lý message từ Kafka**, bao gồm offset commit, retry, DLQ và hành vi consumer group.  
Phần hướng dẫn chi tiết giúp người đọc thực hành:

1. Tạo MSK cluster và topic  
2. Tạo Lambda function và cấp quyền IAM  
3. Cấu hình event source mapping (networking, tham số, quyền truy cập)

Bài viết còn chia sẻ các kỹ thuật tối ưu như sử dụng provisioned concurrency, provisioned pollers, batch size, batch window và event filtering để cân bằng giữa hiệu suất và chi phí.

---

### [Blog 3 – Tăng cường khả năng giao tiếp giữa các agent với Model Context Protocol (MCP)](3.3-Blog3/)

Blog này tập trung vào **các tiêu chuẩn mở trong agentic AI**, đặc biệt là **MCP (Model Context Protocol)** và cách MCP hỗ trợ **giao tiếp agent-to-agent**. AWS thể hiện cam kết của mình với cộng đồng bằng cách tham gia ban điều hành MCP và hỗ trợ nhiều giao thức mở như MCP và A2A.

Blog giải thích vì sao MCP – vốn được thiết kế cho tool integration – lại rất phù hợp để mở rộng sang **giao tiếp giữa các agent**, nhờ các khả năng như:

- Streamable HTTP (SSE, phiên trạng thái, progress update)
- Khám phá và đàm phán khả năng (capability discovery)
- Bảo mật OAuth 2.0
- Chia sẻ ngữ cảnh qua resource capability
- Khả năng sampling cho workflow agentic

Một ví dụ kỹ thuật bằng **Java + Spring AI** minh họa cách:

1. Tạo Agent truy vấn thông tin nhân sự  
2. Expose Agent dưới dạng MCP server tool  
3. Tích hợp Agent này với một HR Agent khác thông qua MCP, tạo nên giao tiếp agent-to-agent qua HTTP

Blog cũng giới thiệu các đề xuất mở rộng MCP mà AWS đang đóng góp, như:

- Human-in-the-loop  
- Streaming partial results  
- Nâng cấp capability discovery  
- Hỗ trợ mô hình giao tiếp bất đồng bộ

Phần cuối chia sẻ phản hồi từ các đối tác lớn (Confluent, CrewAI, Elastic, IBM, LangChain, LlamaIndex, Broadcom…) về tiềm năng của MCP và khuyến khích người đọc tham gia cộng đồng MCP, xem tài liệu, và thử các ví dụ mẫu.

---
