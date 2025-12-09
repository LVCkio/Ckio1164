---
title: "Bản đề xuất"
date: 2025-09-10
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Chuyển đổi số cho Mini-market trên nền tảng đám mây AWS

## Giải pháp E-commerce .NET 3-tier áp dụng Repository và Unit of Work Pattern

### 1. Tóm tắt điều hành

Bản đề xuất này trình bày một giải pháp end-to-end nhằm "Chuyển đổi số cho Mini-market trên nền tảng đám mây AWS". Các mini-market truyền thống hiện đang đối mặt với ba thách thức lớn: (1) Quản lý kho thủ công (bằng Excel/sổ tay) gây thất thoát doanh thu và lãng phí nguồn lực; (2) Lệ thuộc 100% vào kênh bán offline, bỏ lỡ thị trường e-commerce đang phát triển và mất khả năng cạnh tranh; và (3) Quy trình vận hành chậm chạp (như tra giá thủ công), mang lại trải nghiệm khách hàng kém.

Giải pháp của nhóm là xây dựng một nền tảng e-commerce và quản lý vận hành toàn diện. Về software architecture, nhóm sẽ sử dụng kiến trúc .NET 3-lớp (ASP.NET Core MVC, EF Core) kết hợp Repository Pattern và Unit of Work Pattern. Về infrastructure architecture được thiết kế theo AWS Well-Architected Framework, chạy trên AWS Elastic Beanstalk (cho backend .NET), Amazon RDS for SQL Server (cho database), và Amazon S3 (cho static assets). Hệ thống được tối ưu hiệu năng bằng CloudFront và ElastiCache, và bảo mật bằng WAF, VPC, và NAT Gateway. Quy trình triển khai được tự động hóa hoàn toàn bằng CI/CD pipeline tích hợp với GitHub.

Business benefits là ngay lập tức, bao gồm tự động hóa quản lý kho (giảm thất thoát) và mở ra một kênh doanh thu online mới. Về investment, chi phí hạ tầng trong 12 tháng đầu tiên là gần như bằng 0 nhờ tận dụng AWS Free Tier (ví dụ: RDS Express Edition, EC2 t3.micro). Chi phí vận hành dài hạn (sau Free Tier) cũng rất thực tế, ước tính chỉ khoảng 138.06 USD/tháng cho toàn bộ hệ thống. Với investment ban đầu tối thiểu và khả năng giải quyết trực tiếp các vấn đề gây thất thoát doanh thu, ROI là rất cao và gần như tức thì.

Dự án được đề xuất triển khai trong 11 tuần, chia thành 4 phases (giai đoạn) chính: (1) Foundation & Architecture, (2) Core Feature Development, (3) AWS Integration & CI/CD, và (4) Finalization & Deployment. Kết quả mong đợi được đo lường bằng các success metrics cụ thể: giảm 90% sai sót kho vận, giảm 50% thời gian thanh toán, và đạt 20% doanh thu từ kênh online trong 6 tháng đầu. Giải pháp này không chỉ giải quyết các vấn đề trước mắt mà còn cung cấp cho mini-market một nền tảng scalable để đưa ra quyết định dựa trên dữ liệu trong tương lai.

### 2. Tuyên bố vấn đề

_Vấn đề hiện tại_  
Các doanh nghiệp bán lẻ nhỏ và vừa, đặc biệt là mô hình "mini-market" truyền thống tại Việt Nam, đang vận hành dựa trên các quy trình thủ công đã lỗi thời. Trong bối cảnh thị trường ngày càng số hóa, việc không áp dụng công nghệ vào môi trường làm việc đã tạo ra một số vấn đề, trực tiếp ảnh hưởng đến khả năng tồn tại và phát triển của họ.

_Một số vấn đề chính_

- **Vấn đề về quản lý kho thủ công dẫn đến tính thiếu chính xác trong số liệu và lãng phí nguồn lực:** Đa số mini-market hiện nay quản lý hàng nghìn mã sản phẩm (SKUs) bằng sổ sách hoặc file Excel. Việc nhập/xuất kho và kiểm kê cuối ngày hoàn toàn dựa vào việc đếm và nhập thủ công. Việc này có thể dẫn đến sai sót trong dữ liệu vì quy trình nhập tay rất dễ xảy ra nhầm lẫn, tiêu biểu như mã sản phẩm và số lượng, việc này gây ra chênh lệch lớn giữa dữ liệu "trên sổ" và "thực tế" trong kho. Việc kiểm hàng hóa thủ công như này cũng sẽ đòi hỏi nguồn lực cao khi mà nhân viên sẽ phải dành hàng giờ mỗi ngày để kiểm kê, đối chiếu và chỉnh sữa báo cáo, thay vì tập trung vào phần bán hàng hoặc chăm sóc khách hàng. Cuối cùng là thất thoát tài chính, khi mà dữ liệu không được nhập chính xác, chủ cửa hàng sẽ không thể kiểm soát được tình trạng của hàng hóa như là hàng hóa hết hạn, hàng hóa bị hư hỏng, hoặc là bị mất cắp, dẫn đến thất thoát 5-10% giá trị hàng tồn kho hàng tháng.
- **Vấn đề lệ thuộc vào bán hàng offline, bỏ lỡ thị trường E-commerce:** Thông thường các mini-market ở Việt Nam đa số sẽ phụ thuộc vào khách vãng lai(offline). Họ cũng bị giới hạn bởi vị trí địa lí (chỉ phục vụ trong khu vực) và tệp khách hàng quen thuộc. Các cửa hàng như này sẽ đứng ngoài thị trường E-commerce, bỏ lỡ tệp khách hàng trẻ vốn đã quen thuộc với việc mua sắm online. Họ sẽ không thể cạnh tranh về sự tiện lợi như là đặt hàng 24/7 hay là giao hàng tận nơi so với các chuỗi cửa hàng tiện lợi lớn như là Circle K hoặc 7-Eleven và các ứng dụng giao hàng như Grab và Shopee, dẫn đến mất nguy cơ mất khách hàng theo thời gian.
- **Vấn đề về vận hành và trải nghiệm của khách hàng:** Quy trình thanh toán và tra cứu thông tin ở các mini-market truyền thống thông thường sẽ rất chậm chạp. Khi khách hàng hỏi về giá, thông tin của sản phẩm, hoặc là chương trình khuyến mãi, nhân viên (đặc biệt là nhân viên mới) sẽ phải tra cứu thủ công trong sổ sách dẫn đến tốn thời gian. Việc bắt khách hàng phải chờ đợi lâu để tra cứu thông tin hoặc tính tiền sẽ tạo ra sự ức chế và thiếu chuyên nghiệp. Nhân viên sẽ mất quá nhiều thời gian cho các tác vụ đơn giản, dễ nhầm lẫn (như là đọc nhầm giá do chữ viết xấu), làm giảm số lượng khách hàng có thể phục vụ trong giờ cao điểm.

### 3. Kiến trúc giải pháp

Kiến trúc được thiết kế để giải quyết các vấn đề đã nêu, bằng cách kết hợp kiến trúc phần mềm .NET 3-lớp (Tier-3) với các dịch vụ đám mây được quản lý (Managed Services) của AWS. Kiến trúc này tuân thủ các nguyên tắc của AWS Well-Architected Framework, đảm bảo tính bảo mật, hiệu năng cao, khả năng phục hồi lỗi và tối ưu chi phí.

![Mini-market Architecture](/images/2-Proposal/project_architecture_3.1.png)

_Dịch vụ AWS sử dụng_

- _AWS Elastic Beanstalk_: Dịch vụ PaaS (Platform as a Service) được chọn để triển khai ứng dụng .NET 3-lớp (gồm Lớp Presentation WebShop và Lớp Application Services). Beanstalk tự động hóa 100% việc quản lý hạ tầng, bao gồm tự động tạo Auto Scaling Group (ASG) để đảm bảo tính co giãn (Scalability) và tiết kiệm chi phí.

- _Amazon RDS (SQL Server)_: Dịch vụ Cơ Sở Dữ Liệu (CSDL) (Managed Relational Database Service) để host Lớp Persistence. SQL Server được chọn vì ứng dụng .NET của nhóm đã được phát triển và tối ưu cho SQL Server. Việc sử dụng RDS for SQL Server cho phép di chuyển (migrate) ứng dụng lên AWS mà không cần thay đổi code ở Lớp Dữ liệu. RDS cũng sẽ tự động hóa các tác vụ phức tạp như sao lưu (backup) hàng ngày, vá lỗi (patching) và phục hồi khi có sự cố (failover). Về tính bảo mật thì RDS được đặt trong Private Subnet, không thể truy cập trực tiếp từ Internet, chỉ cho phép ứng dụng trên Beanstalk kết nối. Và về vấn đề tối ưu chi phí, để tối ưu chi phí trong giai đoạn đầu, chúng ta có thể bắt đầu với phiên bản SQL Server Express Edition trên RDS, phiên bản này nằm trong Free Tier của AWS.

- _Amazon S3_: Dịch vụ lưu trữ đối tượng (Object Storage). Dùng để lưu trữ các static assets như hình ảnh sản phẩm, file CSS, và JavaScript. S3 cung cấp chi phí cực rẻ và khả năng mở rộng vô hạn.

- _Amazon CloudFront_: Dịch vụ Content Delivery Network - CDN. CloudFront lưu đệm (cache) các file tĩnh từ S3 tại các máy chủ (Edge Locations) trên toàn cầu, giúp người dùng tải trang nhanh hơn đáng kể. Giúp giảm tải trực tiếp cho máy chủ Beanstalk, và giúp ứng dụng .NET tập trung xử lý logic.

- _Amazon WAF và Route 53_: WAF (Web Application Firewall) và Route 53 (Dịch vụ DNS). WAF được liên kết với CloudFront để chặn các cuộc tấn công web phổ biến (như SQL injection, XSS). Route 53 cung cấp tên miền cho người dùng.

- _Amazon ElastiCache (Redis)_: Dịch vụ in-memory data stores. Giúp giảm tải tối đa cho CSDL RDS khi có các truy vấn lặp đi lặp lại (ví dụ: lấy danh sách sản phẩm trang chủ). Ứng dụng .NET sẽ cache các dữ liệu "nóng" này trên ElastiCache, giúp tăng tốc độ phản hồi. Tương tự như RDS, ElastiCache cũng được đặt trong Private Subnet để đảm bảo an toàn.

- _NAT Gateway_: Dịch vụ Network Address Translation. NAT sẽ cung cấp lối ra Internet an toàn cho các dịch vụ trong Private Subnet (như Elastic Beanstalk). Điều này cho phép máy chủ tải về các bản vá bảo mật mà không bị truy cập trực tiếp từ bên ngoài.

- _AWS CodePipeline/CodeBuild_: Dịch vụ tích hợp và triển khai liên tục (CI/CD). Các dịch vụ này được tích hợp với GitLab để tự động hóa quy trình: (1) CodeBuild build code .NET, (2) CodePipeline deploy phiên bản mới lên Elastic Beanstalk.

_Luồng dữ liệu_

- [1]-[2] Người dùng truy cập tên miền (qua Route 53) và được điều hướng đến CloudFront. Amazon WAF sẽ lọc request này.

- [3] (Luồng Tĩnh) Nếu là file tĩnh (ảnh, css), CloudFront lấy trực tiếp từ Amazon S3.

- [4]-[6] (Luồng Động) Nếu là request động, CloudFront chuyển tiếp qua Internet Gateway đến Application Load Balancer, sau đó ALB gửi request vào Elastic Beanstalk.

- [7]-[8] Ứng dụng .NET (trên Beanstalk) sẽ kiểm tra ElastiCache trước, nếu không có sẽ truy vấn Amazon RDS.

- [9]-[10] Khi Elastic Beanstalk cần ra Internet (để tải bản vá), nó sẽ đi qua NAT Gateway rồi ra Internet Gateway.

- [11]-[14] (Luồng CI/CD) Khi Dev push code lên Github, CodePipeline và CodeBuild sẽ tự động build và triển khai (deploy) phiên bản mới lên Elastic Beanstalk.

### 4. Triển khai kỹ thuật

_Các giai đoạn triển khai_  
Dự án sẽ được chia thành 4 giai đoạn chính, kéo dài trong 11 tuần để đảm bảo tiến độ và chất lượng:

1. _Xây dựng nền móng kỹ thuật_: Tập trung xây dựng nền móng kỹ thuật, bao gồm việc chốt mô hình dữ liệu cho các thực thể chính, thiết lập cấu trúc solution .NET 3-lớp (Domain, Application, Persistence, WebShop), khởi tạo repository trên Github, và tìm hiểu về các dịch vụ của AWS. (Tuần 1-4)

2. _Xây dựng các tính năng cốt lõi_: Hoàn thiện Lớp Persistence (Repositories, Unit of Work) và Lớp Application (Services) cho các nhiệm vụ chính như quản lý sản phẩm, người dùng và đơn hàng. Song song đó, Lớp WebShop (Controllers, Views) sẽ được xây dựng cho các luồng đăng nhập, giỏ hàng, thanh toán và bắt đầu viết Unit Test cho Services. (Tuần 5-7)

3. _Tích hợp các dịch vụ của AWS_: Tích hợp Amazon S3 cho ảnh sản phẩm, ElastiCache (Redis) để cache. Nhóm cũng sẽ hoàn thiện CI/CD Pipeline để tự động deploy lên môi trường Staging trên Elastic Beanstalk và thực hiện Integration Testing. (Tuần 8-10)

4. _Hoàn thiện và triển khai_: Cấu hình các dịch vụ bảo mật như CloudFront, WAF, và Route 53. Sau đó, triển khai phiên bản 1.0 lên Elastic Beanstalk, thực hiện UAT cuối cùng, và thiết lập giám sát cơ bản qua CloudWatch. (Tuần 11)

_Yêu cầu kỹ thuật_

- _Backend_: ASP.NET Core MVC 9.0.
- _ORM_: Entity Framework Core.
- _Database_: MS SQL Server 2022 (Local) và Amazon RDS for SQL Server (Cloud).
- _Frontend_: Bootstrap 5, jQuery, và Bootstrap Icons.
- _Cloud Platform (AWS)_: Elastic Beanstalk, RDS, S3, CloudFront, WAF, Route 53, ElastiCache, VPC, NAT Gateway, CodePipeline, CodeBuild.
- _Source Control_: Git.
- _Tools_: Visual Studio 2022, Docker Desktop.

_Phương pháp phát triển_

Áp dụng phương pháp Agile (Scrum-like) để linh hoạt điều chỉnh theo yêu cầu và đảm bảo tiến độ, bám sát 4 giai đoạn triển khai đã đề ra. Mọi công việc (features, bugs) sẽ được theo dõi và quản lý thông qua Kanban board, giúp nhóm dễ dàng nắm bắt tiến độ của từng tác vụ (ví dụ: To Do, In Progress, Done). Mọi code mới phải được review thông qua merge requests trên Github trước khi được merge vào nhánh main, đảm bảo chất lượng code nhất quán.

_Chiến lược kiểm thử_

Để đảm bảo chất lượng và tính ổn định, nhóm sẽ thực hiện 3 cấp độ kiểm thử. Đầu tiên là Unit Testing, tập trung 100% vào Lớp Application (ví dụ: ProductService, OrderService) bằng cách giả lập các repository để cách ly Business Logic, sử dụng các framework kiểm thử tiêu chuẩn của .NET. Cấp độ thứ hai là Integration Testing, thực hiện trên môi trường Staging (trên Elastic Beanstalk) để kiểm tra sự tương tác giữa Lớp Application và Lớp Persistence (EF Core) với CSDL Amazon RDS thật. Cuối cùng, User Acceptance Testing sẽ được thực hiện trên môi trường Production để nhóm kiểm tra các luồng chức năng hoàn chỉnh trên giao diện người dùng như "Đăng ký, Đăng nhập, Thanh toán".

_Kế hoạch triển khai_

Áp dụng quy trình CI/CD tự động hóa hoàn toàn. Quy trình được kích hoạt tự động mỗi khi Dev push code lên Github. Github sẽ gửi webhook kích hoạt AWS CodePipeline, dịch vụ này sẽ lấy code và ra lệnh cho AWS CodeBuild thực hiện biên dịch dự án .NET, chạy Unit Test, và đóng gói ứng dụng thành file .zip. Nếu CodeBuild thành công, CodePipeline sẽ lấy file .zip và tự động triển khai phiên bản mới này lên môi trường Staging trên Elastic Beanstalk.

### 5. Lộ trình & Mốc triển khai

Dự án được lên kế hoạch thực hiện trong 11 tuần, chia thành 4 giai đoạn chính. Tiến độ này đảm bảo thời gian cho việc phát triển, integration, và testing kỹ lưỡng.

Phase 1 (Tuần 1 - 4): Giai đoạn này tập trung xây dựng nền tảng kỹ thuật, bao gồm việc chốt data models, thiết lập Solution Architecture .NET 3-lớp, khởi tạo Github Repository, và tìm hiểu về các dịch vụ AWS. Milestone của giai đoạn này là Solution Architecture và Repository được thiết lập, cùng với môi trường AWS (VPC, Subnets).

Phase 2 (Tuần 5 - 7): Sau khi Phase 1 hoàn thành, nhóm sẽ xây dựng các core features, hoàn thiện Lớp Persistence và Application (Quản lý Sản phẩm, Đơn hàng) và các feature flows cơ bản trên WebShop (Auth, Giỏ hàng). Milestone là các feature flows chính (Đăng nhập, Xem sản phẩm, Giỏ hàng, Thanh toán) hoạt động ổn định trên local, và Unit Test cho Services.

Phase 3 (Tuần 8 - 10): Phụ thuộc vào Phase 2, giai đoạn này sẽ integrate các dịch vụ AWS như Amazon S3 cho ảnh sản phẩm và ElastiCache (Redis) để cache. Milestone là CI/CD pipeline hoạt động, tự động deploy lên Staging environment trên Elastic Beanstalk thành công và Integration Testing hoàn tất.

Phase 4 (Tuần 11): Giai đoạn cuối cùng này tập trung vào hoàn thiện và triển khai, phụ thuộc vào bản build Staging ổn định từ Phase 3. Nhóm sẽ cấu hình các dịch vụ bảo mật (CloudFront, WAF, Route 53). Milestone là Version 1.0 được triển khai thành công lên Production environment (Elastic Beanstalk), User Acceptance Testing cuối cùng hoàn tất, và hệ thống được monitoring qua CloudWatch.

### 6. Ước tính ngân sách

Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=d9984dcf32da018859c676f29d2d4d255a2933ca)  
Hoặc tải [tệp ước tính ngân sách](/attachments/pricing.pdf).

_Chi phí hạ tầng_

Bản ước tính từ AWS Pricing Calculator cho thấy chi phí vận hành hàng tháng của architecture này là 138.06 USD, với chi phí trả trước là 0.00 USD. Chiến lược tối ưu chi phí của nhóm tập trung vào việc tận dụng tối đa AWS Free Tier và các managed services. Con số 138.06 USD/tháng là một chi phí thực tế cho việc vận hành dài hạn (sau 12 tháng) một hệ thống e-commerce hoàn chỉnh, có khả năng scale và bảo mật cao.

Chi phí cho Elastic Beanstalk được chia nhỏ thành các resources mà nó quản lý: Amazon EC2 (1 instance t3a.small) với chi phí 19.15 USD/tháng và Elastic Load Balancing (1 Application Load Balancer) với chi phí 18.69 USD/tháng. Dịch vụ Amazon VPC có chi phí 46.02 USD/tháng, đây là chi phí cho 1 NAT Gateway, một thành phần bắt buộc cho thiết kế bảo mật Private Subnet của Elastic Beanstalk. Về database và cache, Amazon RDS for SQL Server (phiên bản Express Edition trên db.t3.micro) có chi phí 25.86 USD/tháng, và Amazon ElastiCache (cache.t4g.micro) là 17.52 USD/tháng. Các dịch vụ bảo mật và CI/CD như WAF (7.20 USD/tháng), CloudFront (2.43 USD/tháng), Route 53 (0.90 USD/tháng), S3 (0.17 USD/tháng), và CodeBuild (0.12 USD/tháng) chiếm phần chi phí còn lại.

Chiến lược tối ưu chi phí quan trọng nhất là tận dụng AWS Free Tier trong 12 tháng đầu. Mặc dù tổng ước tính là 138.06 USD/tháng, nhiều services cốt lõi trong đây (bao gồm EC2 t3a.small, RDS db.t3.micro, ElastiCache t4g.micro, S3, và CloudFront) đều nằm trong Free tier miễn phí 12 tháng. Do đó, chi phí vận hành thực tế trong năm đầu tiên sẽ thấp hơn đáng kể, chủ yếu chỉ bao gồm chi phí cho NAT Gateway (46.02 USD) và WAF (7.20 USD).

Về phần tính toán ROI, và đầu tư lúc ban đầu gần như bằng 0 (do chi phí hạ tầng được nằm trong Free Tier và development costs là công sức của nhóm trong kỳ thực tập). Phần lợi nhuận gần như là ngay lập tức, vì giải pháp giải quyết trực tiếp các vấn đề thất thoát doanh thu (từ quản lý kho thủ công) và bỏ lỡ thị trường (do chỉ bán offline) đã nêu trong Problem Statement (Phần 1). Do đó, ROI (lợi tức đầu tư) là rất cao.

### 7. Đánh giá rủi ro

Một số rủi ro tiềm ẩn nằm trong ba lĩnh vực: Technical, Business, và Operational. Vậy nên một kế hoạch giảm thiểu và kế hoạch dự phòng đã được chuẩn bị cho các rủi ro có tác động cao.

1. Technical: Quá tải Hệ thống (Performance Bottleneck):

   - Impact: **High** | Probability: **Medium**
   - Đây là rủi ro hệ thống bị chậm hoặc sập khi có lượng traffic cao. Chiến lược giảm thiểu của nhóm là sử dụng ElastiCache để giảm tải query cho RDS, cấu hình Auto Scaling (trong Elastic Beanstalk) với các trigger hợp lý (ví dụ: CPU > 70%), và dùng CloudFront để cache file tĩnh. Kế hoạch dự phòng là sử dụng CloudWatch Alarms để cảnh báo ngay lập tức, và nếu RDS quá tải, nhóm sẽ thực hiện vertical scaling instance của RDS ngay lập tức.

2. Business: Người dùng không chấp nhận (Low User Adoption):

   - Impact: **High** | Probability: **Medium**
   - Đây là rủi ro các chủ mini-market thấy giải pháp quá phức tạp và không sử dụng. Để giảm thiểu rủi ro này, nhóm sẽ bám sát thiết kế frontend (Bootstrap) đơn giản, thu thập feedback của chủ tiệm sớm từ Phase 2, và cung cấp tài liệu hướng dẫn. Kế hoạch dự phòng là nếu adoption thấp sau khi deploy, nhóm sẽ thực hiện một Sprint bổ sung (Phase 5) để ưu tiên điều chỉnh các features dựa trên feedback thu được.

3. Operational: Mất hoặc rò rỉ dữ liệu (Data Loss / Breach):

   - Impact: **Critical** | Probability: **Low**
   - Chiến lược giảm thiểu của nhóm là cấu hình RDS tự động backup hàng ngày, đặt RDS và Beanstalk trong Private Subnet, sử dụng WAF để chặn tấn công, và quản lý connection string qua environment variables của Beanstalk. Kế hoạch dự phòng là nếu mất dữ liệu, nhóm sẽ thực hiện Point-in-Time Recovery (PITR) ngay lập tức từ backup của RDS.

### 8. Kết quả kỳ vọng

Mục tiêu của giải pháp này là giải quyết trực tiếp các vấn đề đã nêu trong phần Tuyên bố vấn đề. Về business metrics, nhóm kỳ vọng giảm 90% sai sót trong quản lý kho (so với Excel/sổ tay), giảm 50% thời gian thanh toán tại quầy, và đạt được ít nhất 20% doanh thu mới từ kênh online trong 6 tháng đầu. Về technical metrics, mục tiêu là duy trì uptime (thời gian hoạt động) 99.9%, đảm bảo page load time (thời gian tải trang) trung bình dưới 2 giây (nhờ CloudFront và ElastiCache), và deployment frequency (tần suất triển khai) ổn định qua CI/CD pipeline.

Các benefits được kỳ vọng là sẽ gia tăng theo thời gian. Trong short-term (0-6 tháng), chủ mini-market sẽ thấy trải nghiệm người dùng được cải thiện ngay lập tức và phần vận hành được cải thiện đáng kể (quản lý kho tự động). Trong medium-term (6-18 tháng), giá trị đến từ việc mở rộng thị trường (tiếp cận khách hàng online) và bắt đầu thu thập được business data có giá trị. Long-term value và strategic capabilities thu được chính là khả năng đưa ra quyết định dựa trên dữ liệu (data-driven decisions) (ví dụ: biết sản phẩm nào bán chạy) và khả năng scale hệ thống dễ dàng (thêm nhiều cửa hàng mới) nhờ Solution Architecture trên Elastic Beanstalk và RDS.

### 9. File Word đính kèm
[Mini-Market AWS Project Plan (DOCX)](/attachments/MiniMarket_AWS_ProjectPlan.docx)