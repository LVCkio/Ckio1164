---
title: "Blog 2"
date: 2025-09-10
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Xây dựng ứng dụng event streaming serverless với Amazon MSK và AWS Lambda
Tác giả: Tarun Rai Madan và Masudur Rahaman Sayem | vào ngày 26 tháng 6 năm 2025 | chuyên mục Amazon Managed Streaming for Apache Kafka (Amazon MSK), Analytics, AWS Lambda, Best Practices, Serverless | Permalink | Comments | Share

Khi các tổ chức xây dựng các ứng dụng hiện đại với event-driven architectures (EDA), họ thường tìm kiếm các giải pháp vừa giảm thiểu gánh nặng quản lý hạ tầng, vừa tối đa hóa năng suất của developer. Amazon Managed Streaming for Apache Kafka (Amazon MSK) và AWS Lambda kết hợp lại sẽ cung cấp một nền tảng serverless, có khả năng mở rộng và tiết kiệm chi phí cho xử lý real-time event-driven.

Trong bài viết này, chúng tôi mô tả cách bạn có thể đơn giản hóa kiến trúc ứng dụng event-driven của mình bằng cách sử dụng AWS Lambda với Amazon MSK. Chúng tôi sẽ trình bày cách cấu hình Lambda như một consumer cho Kafka topics, bao gồm cả thiết lập cross-account, và cách tối ưu giá cả và hiệu năng cho các ứng dụng này.

Tại sao lại sử dùng lambda với Amazon MSK?

Khách hàng xây dựng các ứng dụng event-driven có một số ưu tiên chính khi đưa ra lựa chọn kiến trúc. Họ thường muốn giảm operational overhead bằng cách sử dụng Amazon Web Services (AWS) để xử lý các thành phần hạ tầng phức tạp bên dưới, từ đó đội ngũ của họ có thể tập trung vào core business logic. Ngoài ra, các developer cũng thích một trải nghiệm được tinh gọn, giảm thiểu nhu cầu viết đi viết lại boilerplate code, cho phép họ làm việc hiệu quả hơn và tập trung vào việc tạo ra giá trị. Hơn nữa, các khách hàng này muốn đạt được cả khả năng scalability và cost-effectiveness mà không phải gánh nặng quản lý trực tiếp compute infrastructure. Sự tích hợp giữa Lambda và Amazon MSK đáp ứng hiệu quả các yêu cầu này, mang lại một giải pháp toàn diện kết hợp lợi ích của serverless computing với dịch vụ managed Kafka. Ví dụ, một công ty ecommerce có thể sử dụng Amazon MSK để thu thập real-time clickstream data từ website của mình và xử lý các sự kiện đó bằng AWS Lambda. Với sự tích hợp này, họ có thể kích hoạt Lambda functions để cập nhật mô hình gợi ý, gửi ưu đãi cá nhân hóa, hoặc phân tích hành vi người dùng ngay lập tức — mà không cần triển khai hay quản lý servers. Các lợi ích chính khi sử dụng Lambda với Amazon MSK bao gồm:

Sự đơn giản nhờ native integration – AWS Lambda cung cấp native integration với Amazon MSK thông qua một connector resource gọi là event source mapping. Bạn có thể sử dụng sự tích hợp này để liên kết trực tiếp một Kafka topic — cho dù nó nằm trên Amazon MSK hay một self-managed Kafka cluster — làm event source cho một Lambda function mà không cần phải viết custom consumer logic. Chỉ với một vài bước cấu hình, event source mapping sẽ tự động xử lý partition assignment, offset tracking, và parallelized batch processing ở phía bên dưới. Nó sử dụng Kafka consumer group protocol để phân phối các topic partitions qua nhiều Lambda invocations chạy song song, hỗ trợ batch windowing, và đảm bảo at-least-once delivery semantics. Ngoài ra, nó còn tự động commit offsets sau khi function execution thành công, đồng thời xử lý retries và định tuyến tới dead-letter queue (DLQ) cho các bản ghi thất bại, từ đó giảm đáng kể operational overhead vốn gắn liền với các Kafka consumers truyền thống.

Auto scaling và throughput controls – Khi sử dụng AWS Lambda với Amazon MSK thông qua event source mapping, Lambda sẽ tự động scale bằng cách gán một dedicated event poller cho mỗi Kafka partition, cho phép xử lý song song dựa trên partition. Điều này giúp hệ thống có thể linh hoạt (elastically) xử lý lưu lượng thay đổi mà không cần can thiệp thủ công. Để kiểm soát nâng cao, provisioned concurrency sẽ khởi tạo trước các Lambda execution environments, loại bỏ cold starts và mang lại low-latency performance ổn định. Ngoài ra, với provisioned event source mapping, bạn có thể cấu hình số lượng tối thiểu và tối đa của Kafka pollers, giúp kiểm soát chính xác throughput và concurrency. Điều này đặc biệt phù hợp với các ứng dụng có traffic patterns khó dự đoán hoặc yêu cầu nghiêm ngặt về latency.

Cost-effectiveness – AWS Lambda sử dụng pay-per-use model, trong đó bạn chỉ trả tiền cho compute time và số lượng invocations. Khi được tích hợp với Amazon MSK, sẽ không có chi phí cho idle time, khiến nó trở nên lý tưởng cho các Kafka workloads dạng bursty hoặc low-frequency. Bạn có thể tối ưu chi phí hơn nữa bằng cách điều chỉnh batch size và batch window settings. Đối với các mission-critical workloads, provisioned concurrency cung cấp performance ổn định với pricing được kiểm soát.

Event filtering – AWS Lambda hỗ trợ event filtering cho Amazon MSK event sources, có nghĩa là bạn chỉ cần xử lý những Kafka records khớp với các tiêu chí cụ thể. Điều này giúp giảm bớt các function invocations không cần thiết và tối ưu chi phí cho function của bạn. Bạn có thể định nghĩa tối đa 5 filters cho mỗi event source mapping (với tùy chọn yêu cầu tăng lên 10). Mỗi filter sử dụng một JSON-based pattern để chỉ định điều kiện mà một record phải đáp ứng để được xử lý. Các filters có thể được áp dụng thông qua AWS Management Console, AWS Command Line Interface (AWS CLI), hoặc AWS Serverless Application Model (AWS SAM) templates. Để biết thêm chi tiết và ví dụ, hãy tham khảo AWS Lambda documentation về event filtering với Amazon MSK.

Xử lý sự cố Availability Zone cho khách hàng của bạn – Amazon MSK cung cấp khả năng high availability cho các Kafka brokers bằng cách phân phối chúng trên nhiều Availability Zones trong một Region. Để duy trì high availability cho toàn bộ ứng dụng của bạn, bạn cũng cần một consumer có khả năng high availability. AWS Lambda mang lại high availability và khả năng chống chịu (resilience) bằng cách chạy các consumer functions của bạn trên nhiều Availability Zones trong một Region. Điều này có nghĩa là ngay cả khi một Availability Zone gặp sự cố (outage), Lambda function của bạn vẫn tiếp tục hoạt động trong các Availability Zones còn lại khỏe mạnh. Trong khi Lambda quản lý security patching và các kịch bản lỗi do Availability Zone gây ra, bạn có thể tập trung vào application logic của mình.

Cross-account event processing – Cross-account connectivity giữa AWS Lambda và Amazon MSK cho phép một Lambda function trong một AWS account tiêu thụ dữ liệu từ một MSK cluster trong account khác thông qua MSK multi-VPC private connectivity được hỗ trợ bởi AWS PrivateLink. Thiết lập này đặc biệt hữu ích cho các tổ chức tập trung Kafka infrastructure trong khi vẫn duy trì các account riêng biệt cho các applications hoặc teams khác nhau.

Hỗ trợ cho JSON, Avro, Protobuf và Schema Registries – AWS Lambda hỗ trợ Kafka events ở định dạng JSON, Avro và Protobuf thông qua event source mapping. Nó tích hợp với AWS Glue Schema registry, Confluent Cloud Schema registry, và self-managed Confluent Schema registry, cho phép native schema validation, filtering, và deserialization mà không cần custom code.

Cách Lambda xử lý messages từ Kafka topic của bạn

Lambda sử dụng event source mappings để xử lý records từ Amazon MSK bằng cách polling chủ động các Kafka topics thông qua event pollers, những poller này sẽ gọi các Lambda functions với từng batch records. Các mappings này là các Lambda managed resources được thiết kế cho xử lý stream-based với high-throughput. Theo mặc định, Lambda phát hiện OffsetLag cho tất cả các partitions trong Kafka topic của bạn và tự động scale pollers dựa trên lưu lượng. Đối với các ứng dụng high-throughput, bạn có thể bật provisioned mode để định nghĩa số lượng pollers tối thiểu và tối đa, và event source mapping của bạn sẽ tự động auto scale trong khoảng giá trị được định nghĩa. Trong provisioned mode, mỗi poller có thể xử lý tối đa 5 MBps và hỗ trợ các concurrent Lambda invocations.

Sau khi Lambda xử lý xong từng batch, nó sẽ commit offsets của các messages trong batch đó. Nếu function của bạn trả về error cho một message trong batch, Lambda sẽ retry toàn bộ batch cho đến khi xử lý thành công hoặc khi các messages hết hạn. Bạn có thể gửi các records thất bại sau tất cả các lần retry đến một on-failure destination để xử lý sau. Để duy trì ordered processing trong một partition, Lambda giới hạn số lượng event pollers tối đa bằng với số lượng partitions trong topic. Khi thiết lập Kafka làm Lambda event source, bạn có thể chỉ định một consumer group ID để cho phép Lambda tham gia một Kafka consumer group hiện có. Nếu các consumers khác đang hoạt động trong group đó, Lambda sẽ chỉ nhận một phần messages của topic. Nếu group đã tồn tại, Lambda sẽ bắt đầu từ committed offset của group, bỏ qua StartingPosition. Sơ đồ sau minh họa luồng này.



Hướng dẫn từng bước: Xây dựng một ứng dụng Kafka serverless với AWS Lambda

Thực hiện các bước sau để xây dựng một ứng dụng serverless tiêu thụ messages từ một MSK cluster bằng AWS Lambda:

Tạo một Amazon MSK cluster. Sử dụng AWS Management Console hoặc AWS CLI để tạo MSK cluster của bạn. Khi cluster đã sẵn sàng, hãy tạo Kafka topic(s). Để biết hướng dẫn chi tiết, tham khảo Amazon MSK documentation.


Tạo một Lambda function bằng AWS Management Console hoặc AWS CLI. Để tìm hiểu thêm về cách tạo Lambda function, tham khảo mục Create your first Lambda function. Execution role của Lambda function cần có các quyền sau:

Quyền truy cập để kết nối với MSK cluster của bạn.

Quyền quản lý elastic network interfaces trong VPC của bạn.


Kết nối Lambda với Amazon MSK như một consumer. Thiết lập event source mapping để liên kết MSK topic với Lambda function. Điều này cho phép Lambda tự động poll các messages mới và xử lý chúng. Làm theo hướng dẫn về cách cấu hình event source mapping.


Tham khảo: cấu hình event source mapping bao gồm ba bước:

Network setup – Ở chế độ mặc định của event source mapping, bạn cần cấu hình networking setup sử dụng PrivateLink endpoint hoặc NAT gateway để event source mapping có thể gọi các Lambda functions. Trong provisioned mode, không cần cấu hình mạng (và bạn cũng không chịu chi phí cho các thành phần mạng).

Event source mapping parameter configuration – Bao gồm việc thiết lập các configuration parameters cần thiết để event source mapping có thể poll messages từ Kafka cluster. Điều này bao gồm MSK cluster, topic name, consumer group ID, authentication method, và tùy chọn thêm schema registry, scaling mode. Bạn có thể cấu hình scaling mode cho provisioned throughput, cùng với batch size, batch window, và event filtering cho event source mapping của bạn.


Access permissions – Bao gồm cấu hình các quyền cần thiết để truy cập các AWS resources yêu cầu, bao gồm: quyền để function thực thi code, quyền cho event source mapping truy cập MSK cluster của bạn, và quyền cho Lambda truy cập vào các tài nguyên VPC của bạn.

Ảnh chụp màn hình sau đây hiển thị giao diện console setup cho việc cấu hình Amazon MSK event source mapping, bao gồm các trường liên quan đến Amazon MSK trigger.



Ảnh chụp màn hình sau đây hiển thị cấu hình event poller.



Ảnh chụp màn hình sau đây hiển thị additional settings mà bạn có thể sử dụng, tùy thuộc vào use case của bạn.



Tối ưu hóa AWS Lambda cho stream processing với Amazon MSK

Khi xây dựng real-time data processing pipelines với Amazon MSK và AWS Lambda, việc tinh chỉnh hệ thống để đạt performance và cost-efficiency là rất quan trọng. Lambda cung cấp khả năng serverless compute mạnh mẽ, nhưng để tận dụng tối đa trong bối cảnh streaming, bạn cần thực hiện một số tối ưu hóa chính sau:

Bật provisioned concurrency cho low-latency processing – Với các workload nhạy cảm với latency, cold starts có thể gây ra độ trễ không mong muốn. Bằng cách bật provisioned concurrency, bạn có thể pre-warm một số lượng Lambda instances nhất định để chúng luôn sẵn sàng xử lý traffic ngay lập tức. Điều này loại bỏ cold starts và mang lại consistent response times, rất quan trọng cho các latency-critical use cases.

Bật provisioned mode cho event source mapping để đạt high-throughput processing – Với các Kafka workloads có yêu cầu nghiêm ngặt về throughput, hãy kích hoạt provisioned mode. Cấu hình tối ưu của minimum và maximum event pollers cho Kafka event source mapping phụ thuộc vào yêu cầu hiệu năng của ứng dụng. Bắt đầu với giá trị mặc định của minimum event pollers để thiết lập baseline cho performance profile và điều chỉnh dựa trên observed message processing patterns cũng như yêu cầu hiệu năng. Với các workload có spiky traffic và cần hiệu năng nghiêm ngặt, hãy tăng minimum event pollers để xử lý các đột biến. Bạn có thể tinh chỉnh minimum event pollers bằng cách đánh giá desired throughput, observed throughput (phụ thuộc vào số lượng ingested messages per second và average payload size) và sử dụng khả năng throughput capacity của một event poller (tối đa 5 MB/s) làm tham chiếu. Để duy trì ordered processing trong một partition, Lambda sẽ giới hạn maximum event pollers bằng với số lượng partitions trong topic.

Tối ưu message batching bằng size và windowing – Khi tích hợp Lambda với Amazon MSK, bạn có thể kiểm soát cách các message được batch trước khi gửi đến function của mình. Các tham số cần tinh chỉnh như batch size (số lượng record mỗi lần invocation: 1–10,000 records) và maximum batching window (thời gian chờ để gom đủ batch: 0–300 giây) có thể ảnh hưởng lớn đến hiệu năng. Batch lớn hơn nghĩa là ít invocations hơn, giảm overhead và cải thiện throughput. Tuy nhiên, cần cân bằng — batch hoặc window quá lớn có thể tạo ra độ trễ xử lý không mong muốn. Hãy theo dõi hành vi của stream và điều chỉnh các thông số này dựa trên yêu cầu throughput và mức latency chấp nhận được.

Áp dụng filters để giảm các invocations không cần thiết – Không phải mọi record trong Kafka topic đều cần được xử lý. Để tránh Lambda invocations không cần thiết (và chi phí liên quan), hãy áp dụng filtering logic trực tiếp khi cấu hình event source mapping. Với Lambda, bạn có thể định nghĩa tối đa 10 filters để chỉ những record liên quan mới kích hoạt function. Điều này giúp giảm thời gian compute, hạn chế noise và tối ưu chi phí, đặc biệt hữu ích với các high-throughput topics có nội dung hỗn hợp. Đối với Amazon MSK, Lambda sẽ commit offsets cho cả message matched và unmatched sau khi function được invoke thành công.

Kết Luận

Bằng cách kết hợp Amazon MSK với AWS Lambda, bạn có thể xây dựng liền mạch các ứng dụng modern, serverless, event-driven. Sự tích hợp này loại bỏ nhu cầu phải quản lý consumer groups, compute infrastructure, hoặc scaling logic, nhờ đó các team có thể tập trung vào việc mang lại business value nhanh hơn.

Cho dù bạn đang integrating Kafka into microservices, transforming data pipelines, hay xây dựng reactive applications, Lambda với Amazon MSK là một giải pháp serverless mạnh mẽ và linh hoạt. Để xem tài liệu chi tiết về cách cấu hình Lambda với Amazon MSK, hãy tham khảo AWS Lambda Developer Guide. Để tìm thêm tài nguyên học tập về serverless, hãy truy cập Serverless Land.

Giới thiệu về tác giả

Tarun Rai Madan là Principal Product Manager tại Amazon Web Services (AWS). Anh chuyên về serverless technologies và dẫn dắt product strategy để giúp khách hàng đạt được accelerated business outcomes với event-driven applications, sử dụng các dịch vụ như AWS Lambda, AWS Step Functions, Apache Kafka, và Amazon SQS/SNS. Trước khi gia nhập AWS, anh từng là engineering leader trong ngành semiconductor, và đã dẫn dắt việc phát triển các high-performance processors cho ứng dụng wireless, automotive, và data center.

Masudur Rahaman Sayem là Streaming Data Architect tại AWS với hơn 25 năm kinh nghiệm trong ngành IT. Anh hợp tác với khách hàng của AWS trên toàn cầu để thiết kế và triển khai các giải pháp data streaming phức tạp, nhằm giải quyết những thách thức kinh doanh phức tạp. Là một chuyên gia trong lĩnh vực distributed computing, Sayem tập trung vào việc thiết kế large-scale distributed systems architecture để đạt hiệu năng và khả năng scalability tối đa. Anh có niềm đam mê đặc biệt với distributed architecture, và áp dụng điều đó vào việc xây dựng các giải pháp enterprise-grade ở quy mô internet scale.

TAGS: Amazon MSK, AWS Lambda, Kafka, serverless
