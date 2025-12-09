---
title: "Event 2"
date: "2025-09-10"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}



#### Event Summary Report: “Monitoring & Observability on AWS Workshop”

#### 1. Mục tiêu của sự kiện

Buổi workshop giúp em hiểu rõ hơn về:

- Sự khác nhau giữa **Monitoring** và **Observability**
- Các dịch vụ theo dõi và giám sát hệ thống trên AWS
- Cách sử dụng **Amazon CloudWatch** để theo dõi logs, metrics, alarms và dashboards
- Giới thiệu về **AWS X-Ray** và cách công cụ này giúp phân tích hiệu năng microservices

---

#### 2. Giới thiệu về Monitoring và Observability

#### Monitoring

- Là quá trình **theo dõi hệ thống** thông qua **logs, metrics, events,…**
- Giúp biết được *tình trạng* của hệ thống trong thời gian thực
- Hữu ích cho DevOps & SRE trong việc phát hiện sớm các vấn đề

#### Observability

- Mở rộng hơn Monitoring
- Không chỉ theo dõi mà còn giúp **hiểu được nguyên nhân** khi hệ thống gặp sự cố
- Tập trung vào việc trả lời: *“Chuyện gì đang xảy ra và tại sao?”*

#### Monitoring & Observability trên AWS

Buổi workshop giới thiệu 3 dịch vụ chính:

- **Amazon CloudWatch** – logging, metrics, alarms, dashboards
- **Amazon Managed Grafana** – phân tích và hiển thị dữ liệu trực quan
- **AWS X-Ray** – distributed tracing dành cho microservices

---

#### 3. Amazon CloudWatch

#### 3.1 CloudWatch Overview

Em học được rằng CloudWatch không chỉ là công cụ giám sát mà còn cung cấp đầy đủ khả năng **observability**:

- Theo dõi tài nguyên và ứng dụng **thời gian thực**
- Thu thập **metrics và logs** để phân tích hành vi hệ thống
- Cho phép tạo **alarms** và thiết lập phản hồi tự động
- Hỗ trợ **tối ưu hóa vận hành và chi phí** thông qua dashboards

---

#### 3.2 CloudWatch Metrics

- Metrics là dữ liệu thể hiện **hiệu năng của hệ thống** (CPU, RAM, request count, error rate,…)
*CloudWatch tự thu thập metrics mặc định từ dịch vụ AWS
- Có thể tạo **custom metrics** hoặc thu thập từ hệ thống on-premises qua CloudWatch Agent
- Dễ dàng tích hợp với các dịch vụ AWS khác như Lambda, ECS, API Gateway,…

---

#### 3.3 CloudWatch Logs

- Lưu trữ và phân tích logs ứng dụng
- Hỗ trợ lọc, tìm kiếm, và tạo log-based metrics
- Giúp phát hiện lỗi, theo dõi hành vi người dùng hoặc debug ứng dụng

---

#### 3.4 CloudWatch Alarms

- Cho phép thiết lập cảnh báo dựa trên metrics hoặc logs
- Khi điều kiện được kích hoạt, CloudWatch có thể:

  - Gửi thông báo qua SNS
  - Tự động scale resources
  - Kích hoạt Lambda để xử lý sự cố

---

#### 3.5 CloudWatch Dashboards

- Giao diện trực quan hóa hệ thống
- Cho phép tạo charts, graphs để theo dõi hoạt động
- Hữu ích cho DevOps trong quá trình giám sát và ra quyết định

---

#### 4.AWS X-Ray

AWS X-Ray được giới thiệu như một công cụ mạnh để quan sát hệ thống **microservices**.

#### 4.1 Distributed Tracing

- Theo dõi **toàn bộ đường đi của một request** từ đầu đến cuối
- Tạo ra **service maps**, giúp nhìn thấy các kết nối gữa microservices
- Cần tích hợp SDK vào ứng dụng để tạo trace ID cho từng request

#### 4.2 Phân tích hiệu năng

- X-Ray giúp phát hiện:

  - Điểm gây chậm (bottlenecks)
  - Dịch vụ nào tốn thời gian xử lý
  - Phần nào của request gặp lỗi

#### 4.3 Tích hợp với CloudWatch

- Logs và metrics từ CloudWatch có thể kết hợp với X-Ray để cung cấp cái nhìn đầy đủ về hoạt động hệ thống
- Giúp DevOps và Developer **khắc phục lỗi nhanh hơn**

---

#### 5.Những điều em rút ra được

- Monitoring và Observability không giống nhau — Observability giúp hiểu sâu nguyên nhân vấn đề hơn
- CloudWatch là công cụ rất mạnh vì hỗ trợ cả **metrics → logs → alarms → dashboards**
- X-Ray giúp theo dõi request xuyên suốt microservices, rất hữu ích cho ứng dụng phức tạp
- Việc tích hợp CloudWatch và X-Ray tạo nên bộ công cụ quan sát hệ thống toàn diện

---

#### 6. Ứng dụng vào học tập và công việc

- Em có thể dùng CloudWatch để theo dõi các bài lab hoặc project chạy trên AWS
- Với ứng dụng microservices, em muốn thử tích hợp X-Ray để xem đường đi của request
- Dashboards giúp em dễ dàng trình bày trạng thái hệ thống trong các bài báo cáo
- Alarms giúp tự động phát hiện lỗi mà không cần giám sát thủ công

---

#### 7.Trải nghiệm của em tại sự kiện

- Em hiểu rõ hơn về cách giám sát hệ thống trong thực tế
- Việc xem demo về alarms, metrics và X-Ray giúp em dễ hình dung hơn so với lý thuyết
- Kiến thức này rất hữu ích cho định hướng DevOps hoặc Cloud Engineer mà em đang theo đuổi


