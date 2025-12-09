---
title: "Event 3"
date: "2025-09-10"
weight: 2
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài thu hoạch “DevOps on AWS”

### Mục Đích Của Sự Kiện

- Định hướng lộ trình nghề nghiệp trong lĩnh vực DevOps và Cloud.
- Hiểu sâu về quy trình CI/CD và Containerization.
- Phân tích vai trò của Infrastructure as Code (IaC) so với ClickOps.
- So sánh các giải pháp Orchestration trên AWS (ECS vs EKS) và chiến lược Monitoring/Observability.

### Nội Dung Nổi Bật

#### Lộ Trình DevOps Thế Hệ Mới

- **Các vai trò liên quan**: DevOps Engineer, Cloud Engineer, Platform Engineer, Site Reliability Engineer (SRE).
- **T-shaped Skill**: Mô hình phát triển kỹ năng với kiến thức rộng về nhiều mảng và chuyên sâu vào một mảng cụ thể.
- **Lời khuyên cho người mới**:
    - **Do**: Bắt đầu từ nền tảng, học qua dự án thực tế, viết tài liệu, tập trung làm chủ từng kỹ năng một.
    - **Don't**: Sa lầy vào "Tutorial Hell", copy-paste mù quáng, so sánh bản thân với người khác, bỏ cuộc sau thất bại.

#### Continuous Integration & Deployment (CI/CD)

- **CI (Continuous Integration)**: Quy trình tích hợp code thường xuyên vào kho lưu trữ chung.
- **Phân biệt CD**:
    - **Continuous Delivery**: Tự động hóa đến bước Acceptance Test, việc deploy lên Production cần kích hoạt thủ công.
    - **Continuous Deployment**: Tự động hóa hoàn toàn từ code đến Production.

#### Infrastructure as Code (IaC)

- **Vấn đề của ClickOps**: Chậm, dễ gây lỗi con người (Human Error), thiếu nhất quán, khó cộng tác.
- **Lợi ích của IaC**: Tự động hóa, khả năng mở rộng (Scalability), khả năng tái tạo (Reproducibility), tăng cường cộng tác.
- **AWS CloudFormation**:
    - Công cụ IaC tích hợp sẵn của AWS sử dụng JSON/YAML templates.
    - **Stack**: Tập hợp các resource được quản lý chung.
    - **Drift Detection**: Phát hiện sự sai lệch giữa cấu hình thực tế và template (khi có ai đó sửa tay resource).
- **AWS CDK (Cloud Development Kit)**:
    - Sử dụng ngôn ngữ lập trình (Python, TS...) để định nghĩa hạ tầng.
    - Các khái niệm: L1 (Mapping 1:1), L2, L3 Constructs.
    - Các lệnh CLI: **cdk init**, **cdk synth**, **cdk deploy**, **cdk diff,** **cdk destroy**.
- **Terraform**: Công cụ mã nguồn mở, hỗ trợ Multi-cloud, sử dụng ngôn ngữ HCL, quản lý trạng thái qua State file.

#### Container Ecosystem

- **Docker Fundamentals**: Dockerfile (định nghĩa build) → Image (bản thiết kế đóng gói) → Container (runtime).
- **Amazon ECR**: Private container registry của AWS, hỗ trợ image scanning, immutable tags và lifecycle policies.
- **Container Orchestration**: Quản lý vòng đời container (restart, scale, distribute traffic).
    - **Amazon ECS**: Giải pháp native của AWS. Hỗ trợ Launch types: EC2 (quản lý server) và Fargate (Serverless - dễ dàng hơn).
    - **Amazon EKS**: Dịch vụ Kubernetes được quản lý. Phù hợp cho hệ thống phức tạp, yêu cầu kinh nghiệm về K8s.
    - **Amazon App Runner**: Giải pháp đơn giản nhất để deploy web app/API nhanh chóng và tiết kiệm chi phí.

#### Monitoring & Observability

- **Phân biệt**:
    - **Monitoring**: Theo dõi Logs, Metrics (Hệ thống đang hoạt động ra sao?).
    - **Observability**: Hiểu sâu nguyên nhân sự việc (Tại sao hệ thống lại hoạt động như vậy?).
- **Amazon CloudWatch**:
    - Thu thập Metrics (CPU, RAM, Network...) và Logs theo thời gian thực.
    - **Alarms**: Cảnh báo và phản ứng tự động.
    - **Dashboards**: Trực quan hóa dữ liệu vận hành.
- **AWS X-Ray**: Distributed tracing cho microservices, giúp vẽ bản đồ dịch vụ (Service maps) và phân tích điểm nghẽn hiệu năng (Performance bottlenecks).

### Những Gì Học Được

#### Tư Duy Hệ Thống

- **Infrastructure Drift**: Hiểu được rủi ro khi sửa đổi tài nguyên thủ công ngoài IaC và tầm quan trọng của việc dùng Drift Detection.
- **Trade-offs**: Biết cách lựa chọn công cụ IaC (CDK cho AWS-centric, Terraform cho Multi-cloud) và công cụ Orchestration (ECS cho người mới/đơn giản, EKS cho tính năng cao cấp).

#### Kỹ Năng Vận Hành

- **Quy trình chuẩn**: Sự khác biệt cốt lõi giữa Continuous Delivery và Continuous Deployment nằm ở bước duyệt thủ công lên Production.
- **Quản lý Container**: Hiểu rõ workflow từ Dockerfile lên ECR và cách ECS/EKS điều phối container hoạt động.

### Ứng Dụng Vào Công Việc

- **Chuyển đổi sang IaC**: Bắt đầu viết CloudFormation hoặc CDK cho dự án hiện tại thay vì thao tác trên Console.
- **Tối ưu Pipeline**: Rà soát lại quy trình CI/CD, tích hợp thêm các bước test tự động trước khi deploy.
- **Triển khai Observability**: Tích hợp AWS X-Ray vào ứng dụng để trace request qua các microservices, kết hợp với CloudWatch Alarms để giám sát chủ động.
- **Refactor Docker**: Tối ưu hóa Dockerfile và sử dụng ECR Lifecycle Policies để quản lý dung lượng lưu trữ image.

### Trải nghiệm trong event

Tham gia sự kiện **“Next-Generation DevOps & Cloud Architecture”** là một bước đệm quan trọng giúp mình định hình rõ ràng con đường phát triển kỹ năng DevOps.

#### Định hướng rõ ràng

- Diễn giả đã vẽ ra một roadmap rất thực tế với lời khuyên "Don't stay in Tutorial Hell" - điều mà mình rất tâm đắc.
- Mô hình **T-shaped skill** giúp mình nhận ra không cần phải biết tất cả mọi thứ ngay lập tức mà nên tập trung chuyên sâu vào một mảng trước.

#### Kiến thức chuyên sâu về công cụ

- Sự so sánh chi tiết giữa **ECS và EKS** giúp mình tự tin hơn trong việc lựa chọn giải pháp compute phù hợp cho dự án (với người mới bắt đầu như mình, ECS Fargate là lựa chọn tối ưu).
- Phần trình bày về **IaC và Drift Detection** đã thay đổi hoàn toàn tư duy của mình về việc quản lý hạ tầng: "Use code, not clicks".

#### Tầm quan trọng của Observability

- Mình nhận ra rằng chỉ Monitoring là chưa đủ, mà cần phải đạt được **Observability** thông qua các công cụ như AWS X-Ray để giải quyết triệt để các vấn đề trong hệ thống phân tán.

#### Một số hình ảnh khi tham gia sự kiện

- Thêm các hình ảnh của các bạn tại đây
  > Sự kiện này không chỉ cung cấp kiến thức kỹ thuật mà còn truyền cảm hứng về tư duy làm nghề, từ việc xây dựng văn hóa CI/CD đến việc tự động hóa mọi thứ có thể.