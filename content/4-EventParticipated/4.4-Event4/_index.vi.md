---
title: "Event 4"
date: "2025-09-10"
weight: 3
chapter: false
pre: " <b> 4.4. </b> "
---

# Bài thu hoạch “AWS Security Governance & Automation”

### Mục Đích Của Sự Kiện

- Giới thiệu tầm nhìn của Cloud Club và tầm quan trọng của cộng đồng.
- Chia sẻ các phương pháp quản lý danh tính tập trung (Identity Management) và bảo mật tài khoản.
- Hướng dẫn chiến lược giám sát an ninh đa lớp (Multi-Layer Visibility).
- Tự động hóa quy trình phản ứng sự cố bảo mật (Automated Alerting) với EventBridge.

### Danh Sách Diễn Giả

- **[Tên Diễn Giả]** - [Chức vụ/Tổ chức] *(Bạn điền tên diễn giả vào đây nhé)*

### Nội Dung Nổi Bật

#### Quản Lý Danh Tính & Truy Cập (IAM & Governance)

- **Single Sign-On (SSO)**: Cơ chế "One login, multiple systems". Thay vì tạo nhiều IAM User lẻ tẻ, SSO cho phép quản lý danh tính tập trung, giúp người dùng chỉ cần đăng nhập một lần để truy cập nhiều tài khoản/ứng dụng AWS.
- **Service Control Policies (SCPs)**: Một loại chính sách tổ chức (Organizational Policy) dùng để thiết lập "hàng rào bảo vệ" (guardrails). SCPs giới hạn quyền tối đa (maximum permissions) cho các tài khoản thành viên trong AWS Organization.
- **Phổ thông tin xác thực (Credentials Spectrum)**:
    - **Long-term**: Access Keys của IAM User (không hết hạn) → Rủi ro cao, cần hạn chế sử dụng.
    - **Short-term**: IAM Roles, STS tokens (hết hạn sau 15 phút - 36 giờ) → Bảo mật cao hơn, là best practice.
- **MFA (Multi-Factor Authentication)**: Lớp bảo mật bắt buộc phải có cho mọi tài khoản.

#### Khả Năng Quan Sát Bảo Mật Đa Lớp (Visibility)

- **IAM Access Analyzer**: Công cụ giúp phát hiện các tài nguyên (S3, KMS, IAM Roles...) đang bị chia sẻ công khai hoặc chia sẻ với tài khoản ngoài vùng tin cậy.
- **Phân loại sự kiện (Logging)**:
    - **Management Events**: Ai đã làm gì với tài nguyên? (VD: Tạo EC2, Xóa S3 Bucket).
    - **Data Events**: Ai đã truy cập vào dữ liệu? (VD: GetObject S3, Invoke Lambda).
    - **Network Activity Events**: Giám sát lưu lượng mạng VPC.

#### Tự Động Hóa & Cảnh Báo (Automation)

- **Amazon EventBridge**: Trung tâm xử lý sự kiện thời gian thực (Real-time Events).
    - Hỗ trợ **Cross-account Event**: Nhận sự kiện từ tài khoản con về tài khoản bảo mật trung tâm.
    - **Automated Alerting**: Tự động gửi cảnh báo khi phát hiện hành vi bất thường.
- **Detection as Code**:
    - Chuyển đổi logic phát hiện mối đe dọa thành mã (Infrastructure as Code).
    - Sử dụng **CloudTrail Lake queries** để truy vấn lịch sử.
    - Quản lý phiên bản (Version control) cho các quy tắc bảo mật.

#### Bảo Mật Mạng (Network Security)

- **Common Attack Vectors**: Các hướng tấn công mạng phổ biến (DDoS, SQL Injection, Man-in-the-middle...).
- **AWS Layered Security**: Chiến lược phòng thủ chiều sâu (Defense in Depth) - bảo vệ từ lớp ngoài (Edge), lớp mạng (VPC), đến lớp ứng dụng và dữ liệu.

#### Những Gì Học Được

#### Tư Duy Bảo Mật Hiện Đại

- **Identity is the new perimeter**: Trong môi trường Cloud, quản lý danh tính (IAM/SSO) quan trọng hơn cả tường lửa truyền thống.
- **Zero Trust**: Không tin tưởng bất kỳ ai, luôn xác thực (MFA) và cấp quyền tối thiểu (Least Privilege).

#### Chiến Lược Quản Trị

- **Shift from Long-term to Short-term**: Hiểu rõ rủi ro của Access Key lâu dài và tầm quan trọng của việc chuyển sang sử dụng Temporary Credentials.
- **Governance at Scale**: Sử dụng SCPs để quản lý hàng trăm tài khoản AWS một cách đồng bộ thay vì cấu hình thủ công từng cái.

#### Kỹ Thuật Tự Động Hóa

- **Event-Driven Security**: Thay vì rà soát log thủ công (thụ động), sử dụng EventBridge để phản ứng ngay lập tức (chủ động) khi có sự cố bảo mật xảy ra.

#### Ứng Dụng Vào Công Việc

- **Rà soát IAM**: Kiểm tra và vô hiệu hóa các IAM Access Keys cũ, bắt buộc bật MFA cho toàn bộ team.
- **Triển khai Access Analyzer**: Kích hoạt ngay để quét xem có S3 bucket nào đang public nhầm không.
- **Thiết lập cảnh báo**: Tạo rule EventBridge đơn giản để bắn thông báo về Slack/Email khi có ai đó đăng nhập bằng tài khoản Root hoặc thay đổi Security Group.
- **Học về CloudTrail**: Cấu hình CloudTrail để ghi lại cả Management và Data events cho các tài nguyên quan trọng.

#### Trải nghiệm trong event

Sự kiện thứ 3 mang đến một góc nhìn rất sâu sắc về khía cạnh **Security & Governance** - mảng thường bị xem nhẹ trong quá trình phát triển nhưng lại sống còn đối với doanh nghiệp.

#### Nhận thức về rủi ro

- Phần trình bày về **Credentials Spectrum** giúp tôi giật mình nhận ra thói quen sử dụng IAM User với Long-term keys nguy hiểm như thế nào. Việc chuyển dịch sang Short-term credentials là điều bắt buộc.

#### Sức mạnh của tự động hóa

- Tôi rất ấn tượng với khái niệm **"Detection as Code"**. Việc quản lý các quy tắc bảo mật như quản lý source code giúp quy trình vận hành trở nên minh bạch và dễ kiểm soát hơn.
- Demo về **EventBridge** cho thấy khả năng phản ứng thời gian thực tuyệt vời, giúp giảm thiểu thời gian kẻ tấn công có thể gây hại trong hệ thống.

#### Tư duy phòng thủ nhiều lớp

- Hiểu rõ hơn về **AWS Layered Security**, bảo mật không chỉ là một cánh cửa mà là nhiều lớp rào chắn phối hợp với nhau từ Network đến Identity.


  > Sự kiện này đã thay đổi tư duy của tôi từ "Làm cho chạy được" sang "Làm cho an toàn và tuân thủ chuẩn". Bảo mật không phải là rào cản, mà là nền móng để phát triển bền vững.