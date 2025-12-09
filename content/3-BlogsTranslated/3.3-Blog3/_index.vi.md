---
title: "Blog 3"
date: 2025-09-10
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Giao thức mở cho khả năng tương tác của Agent – Phần 2: Authentication trên MCP
Tác giả: Darin McAdams và Elie Schoppik | vào ngày 26 tháng 06 năm 2025 | chuyên mục: Artificial Intelligence, Customer Solutions, Generative AI, Open Source | Permalink | Comments | Share

Phần 1 của loạt blog Open Protocols for Agent Interoperability đã đề cập cách Model Context Protocol (MCP) có thể được sử dụng để hỗ trợ giao tiếp giữa các agent và các cải tiến trong đặc tả MCP specification mà AWS đang thực hiện để hiện thực hóa điều đó. Phần 2 này đi sâu vào authentication trong phiên bản mới nhất của MCP specification và thảo luận một số đóng góp từ AWS trong lần phát hành này.

Model Context Protocol (MCP), được tạo ra bởi Anthropic, đã có sự chấp nhận rộng rãi đáng chú ý kể từ khi ra mắt vào tháng 11/2024, thu hút sự quan tâm của các developer và tổ chức trên toàn cầu. Ban đầu, MCP được giữ đơn giản — người dùng chỉ cần tải xuống và chạy local MCP servers ngay trên workstation của mình. Vào tháng 3, MCP chính thức hóa cách tiếp cận với remote server communication bằng Streamable HTTP paradigm. Những remote servers này loại bỏ nhu cầu cài đặt và cập nhật phần mềm cục bộ, giảm rủi ro bảo mật và độ phức tạp trong triển khai, đồng thời đảm bảo người dùng luôn truy cập vào phiên bản mới nhất của dịch vụ. Tuy nhiên, việc chuyển sang remote servers đặt ra một thách thức: làm thế nào để đảm bảo truy cập an toàn đến các MCP server URLs? Mặc dù authentication cho web services là một lĩnh vực đã được thiết lập từ lâu, nhưng các mục tiêu độc đáo của MCP đã dẫn chúng tôi đi đến những hướng không ngờ tới.

Khi MCP phát triển từ tầm nhìn ban đầu của Anthropic thành một tiêu chuẩn công nghiệp rộng hơn, sự hợp tác với các nhà cung cấp cloud như AWS trở nên thiết yếu để hiện thực hóa toàn bộ tiềm năng của nó ở quy mô enterprise.

Dựa trên framework MCP nền tảng của Anthropic, AWS đã hợp tác chặt chẽ với các contributor của MCP specification và implementation để giải quyết những khoảng trống liên quan đến authentication, đóng góp vào cả các cuộc thảo luận kỹ thuật lẫn security best practices trong specification, đồng thời submit Java PR để implement authentication trong SDK. Những cải tiến này cho phép hosting từ xa các MCP servers với authenticated access, bao gồm cả trên AWS. Với bản phát hành ngày 18/06/2025 của MCP specification hiện đã bao gồm một cách tiếp cận authentication toàn diện, đây là thời điểm thích hợp để khám phá các giải pháp kỹ thuật và những ràng buộc thú vị đã định hình nên chúng.

Không chỉ là một Protocol khác

Mục tiêu của MCP là làm cho các ứng dụng được hỗ trợ bởi AI trở nên dễ sử dụng và dễ tích hợp hơn. MCP hướng tới việc cho phép một số lượng nhỏ client applications được thiết kế tốt có thể kết nối tới một loạt MCP servers, tương tự như cách mà chỉ một số ít web browsers và email clients có thể kết nối với vô số websites và người dùng. Việc kết nối tới một MCP server mới cần đơn giản như đăng nhập vào một website — không cần thiết lập phức tạp.

Khi thiết kế protocols, việc cung cấp cho các implementors sự linh hoạt trong cách xử lý authentication là điều phổ biến. OpenAPI minh họa rõ điều này – nó cho phép developer lựa chọn từ nhiều cơ chế authentication như API Keys, Bearer Tokens, hoặc Mutual TLS, để mỗi implementation có thể chọn cách phù hợp nhất với nhu cầu cụ thể.

Tuy nhiên, mục tiêu của MCP là cho phép kết nối liền mạch giữa các clients và nhiều servers khác nhau, điều này đòi hỏi một cách tiếp cận khác. Thay vì đưa ra nhiều tùy chọn, specification cần thiết lập một con đường được khuyến nghị có thể hoạt động ổn định trên tất cả các implementations. Cách tiếp cận mang tính "prescriptive" này đảm bảo rằng clients và servers có thể kết nối đáng tin cậy mà không cần phối hợp trước.

OAuth đã nổi lên như một lựa chọn tự nhiên cho cách tiếp cận tiêu chuẩn hóa này. Là protocol tiêu chuẩn trong ngành cho authorization, OAuth được hiểu rộng rãi, kiểm thử kỹ lưỡng, và hiện đã cung cấp authentication cho nhiều dịch vụ mà mọi người sử dụng hàng ngày. Quan trọng hơn, kiến trúc của OAuth mang lại nền tảng cần thiết để đạt được tầm nhìn plug-and-play của MCP.

Tuy nhiên, để đạt được khả năng kết nối "zero-configuration" thực sự, cần phải vượt qua giới hạn của các OAuth implementations truyền thống. Trong thiết lập OAuth điển hình, developer cần phải đăng ký thủ công ứng dụng của mình với từng nhà cung cấp dịch vụ, tỉ mỉ sao chép client IDs, secrets, và endpoint URLs giữa các hệ thống. Việc cấu hình thủ công này, tuy có thể quản lý được khi chỉ kết nối với một hoặc hai dịch vụ, nhưng trở nên cồng kềnh trong tầm nhìn của MCP về việc các clients dễ dàng kết nối với nhiều servers khác nhau.

Vì vậy, specification tận dụng một số phần mới và ít được sử dụng của OAuth framework — đặc biệt là xung quanh automated discovery và dynamic registration. Các khả năng này cho phép MCP clients tự động khám phá các endpoints cần thiết và tự đăng ký với servers mới, mà không cần người dùng phải sao chép và dán credentials. Để hiểu rõ cách MCP hiện thực hóa sự đơn giản này, hãy cùng xem những thành phần chính trong cách tiếp cận authentication của nó.

Các yếu tố chính của MCP Authentication

Bản MCP specification chính thức cung cấp một mô tả toàn diện về authentication flow, bao gồm cả sequence diagram chi tiết:



Hãy cùng đi qua cách mà quy trình này hoạt động trong thực tế, bắt đầu từ việc người dùng lần đầu tiên tiếp xúc với một MCP server.

Việc thêm một MCP server mới bắt đầu với một URL – một địa chỉ web cho biết MCP client cần tìm server ở đâu. Người dùng thường tìm thấy các URL này từ những nguồn đã biết, chẳng hạn như tài liệu sản phẩm hoặc các registry trực tuyến. Những gì xảy ra tiếp theo là một chuỗi bước giúp biến URL này thành một kết nối bảo mật, đã được authenticated.

Như một bước khởi tạo, quá trình bắt đầu bằng việc MCP client thu thập thông tin cần thiết về cách sử dụng và authenticate với server. Để thực hiện điều này, client trước tiên sẽ yêu cầu OAuth Protected Resource Metadata từ server (với những ai quan tâm đến chi tiết kỹ thuật, điều này được định nghĩa trong một tiêu chuẩn gần đây, RFC 9728). Metadata này giống như một “danh thiếp” của server – nó chứa thông tin quan trọng về cách thiết lập kết nối an toàn, bao gồm cả vị trí của OAuth Authorization Server, vốn đóng vai trò là cơ quan đáng tin cậy trong việc cấp và quản lý access credentials.

Sau khi đã xác định được Authorization Server, MCP client thực hiện thêm một bước chuẩn bị nữa: truy vấn tài liệu metadata của chính Authorization Server đó (một khả năng được định nghĩa trong RFC 8414). Tài liệu này cho client biết chính xác cách tương tác với Authorization Server cụ thể này – tương tự như việc nhận một “sổ tay hướng dẫn” để hoàn tất security handshake.

Với những thông tin này trong tay, MCP client có thể giới thiệu bản thân tới Authorization Server thông qua một quy trình gọi là Dynamic Client Registration. Trong bước này, client tự động cung cấp chi tiết về chính nó và nhận về một unique identifier. Cách tiếp cận tự động này đơn giản hóa quy trình vốn dĩ trước đây phải làm thủ công bởi quản trị viên — như việc sao chép và dán credentials giữa các hệ thống. MCP specification làm cho quy trình này trở nên liền mạch đối với người dùng cuối.

Đến thời điểm này, MCP client đã có tất cả những gì nó cần để kết nối an toàn đến MCP server cụ thể đó. Bước cuối cùng đi theo chuẩn OAuth authorization flow, trong đó người dùng được chuyển hướng đến trang đăng nhập của dịch vụ trên trình duyệt. Giống như bất kỳ dịch vụ được bảo vệ bởi OAuth nào, service provider sẽ xử lý user authentication và permission management thông qua implementation OAuth chuẩn của họ – ví dụ, kiểm soát loại dữ liệu nào mà MCP client được phép truy cập. Sau khi người dùng xác thực và chấp thuận các quyền đó, MCP client nhận được OAuth access tokens cần thiết để gọi đến MCP server một cách an toàn.

Security luôn là yếu tố trung tâm trong quy trình tinh gọn này, đồng thời đi kèm với những đánh đổi quan trọng. Việc làm cho kết nối dễ dàng hơn đối với server hợp pháp cũng đồng nghĩa với việc kết nối tới server độc hại cũng dễ hơn. Specification đã bao gồm các cơ chế bảo vệ kỹ thuật chống lại credential theft thông qua RFC 8707, đảm bảo rằng credentials được cấp cho một server không thể bị lạm dụng bởi một server khác. Điều này có nghĩa là ngay cả khi ai đó vô tình kết nối tới một server lừa đảo (ví dụ Examp1e.com thay vì Example.com), server đó cũng không thể truy cập vào tài khoản hợp pháp của người dùng ở nơi khác. MCP client đưa URL của server cụ thể vào trong quá trình authentication, và Authorization Server đảm bảo rằng credentials chỉ được giới hạn cho server đó.

Ngoài những cơ chế bảo vệ kỹ thuật, MCP registry working group đang phát triển các cách tiếp cận cho trusted registries của MCP servers, tương tự như cách các app store và package repository giúp người dùng nhận diện phần mềm hợp pháp. Dù công việc này vẫn đang được tiến hành, mục tiêu là hỗ trợ người dùng khám phá và xác minh các dịch vụ tin cậy thông qua các thư mục được duy trì tốt của những MCP implementation đã biết.

Bằng cách kết hợp automated discovery, dynamic registration, và các enhanced security controls, MCP đã tự động hóa quy trình vốn dĩ thủ công trước đây, đồng thời bổ sung các biện pháp bảo vệ cho thế giới động và linh hoạt hơn này.

Triển khai MCP Authentication: Các tùy chọn và sự đánh đổi

MCP specification dựa trên một số khả năng OAuth mới hơn và ít được triển khai phổ biến, điều này tự nhiên làm nảy sinh câu hỏi từ phía implementors. Một trong những câu hỏi thường gặp liên quan đến tính linh hoạt: điều gì sẽ xảy ra nếu một service provider muốn triển khai nhiều hơn, hoặc ít hơn, so với những gì specification yêu cầu?

MCP specification nên được hiểu như là việc đặt ra mức chuẩn tối thiểu để đảm bảo interoperability, chứ không phải giới hạn tối đa. Service providers hoàn toàn có thể cung cấp thêm các tùy chọn authentication ngoài những gì specification quy định. Yếu tố chính ở đây là tính thực tế: trong khi các MCP SDK triển khai những chức năng cơ bản này, việc mở rộng vượt ngoài có thể đòi hỏi custom client implementations thay vì sử dụng các MCP client chuẩn.

Nhưng nếu triển khai ít hơn so với yêu cầu của specification thì sao? Ở đây có sự đánh đổi rõ ràng. Các dịch vụ không tuân thủ đầy đủ specification sẽ không thể tham gia vào trải nghiệm plug-and-play vốn là điểm nổi bật của MCP. Điều này có thể khiến dịch vụ khó được chấp nhận hơn và kém hấp dẫn hơn. Nó cũng có thể hạn chế khả năng tương thích với open source SDKs và các công cụ đang được cộng đồng phát triển.

Ảnh hưởng của những đánh đổi này thay đổi tùy theo ngữ cảnh. Trong các môi trường được kiểm soát, như mạng doanh nghiệp nơi cả client và server đều được quản lý nội bộ, việc lệch khỏi specification có thể không gây hậu quả đáng kể. Tuy nhiên, đối với public APIs phục vụ khách hàng bên ngoài, hoặc các dịch vụ hướng tới sự chấp nhận rộng rãi, việc không tuân thủ trở nên quan trọng hơn nhiều.

Một mối quan ngại đặc biệt thường đến từ các service providers vốn không sử dụng OAuth. Ví dụ, nhiều dịch vụ hiện có chỉ dựa hoàn toàn vào API Keys, dẫn đến lo ngại rằng việc hỗ trợ MCP sẽ đòi hỏi phải xây dựng lại toàn bộ kiến trúc authentication của họ – điều này rõ ràng là một viễn cảnh đáng lo ngại.

May mắn thay, thực tế lại dễ quản lý hơn. Authentication có thể được xử lý theo lớp, với yêu cầu OAuth của MCP chỉ áp dụng cho front door của MCP server. Ẩn sau lớp OAuth này, server có thể chuyển đổi các token thành bất kỳ loại credential nào mà backend services của bạn mong đợi, từ đó bảo toàn kiến trúc authentication hiện có.

Đúng là việc triển khai front door này cần nỗ lực, ngay cả đối với các tổ chức đã quen thuộc với OAuth. Tuy nhiên, khoản đầu tư này vào automated discovery và dynamic registration chính là yếu tố hiện thực hóa tầm nhìn plug-and-play được mô tả trước đó – nơi mà các clients có thể kết nối liền mạch tới bất kỳ MCP server nào mà không cần cấu hình thủ công.

Quan trọng hơn, thiết kế của MCP cho phép nỗ lực triển khai này được tập trung và thực hiện một lần cho toàn bộ tổ chức. Điều này đạt được nhờ một tùy chọn kiến trúc mạnh mẽ khác trong specification: sự tách biệt giữa MCP servers và Authorization Servers. Cách tiếp cận này phù hợp với nguyên tắc cốt lõi của OAuth về việc tách biệt trách nhiệm giữa resource và authorization. Trong khi một MCP server chỉ cần xác thực các token đến, thì những khía cạnh phức tạp hơn của authentication có thể được ủy quyền cho một Authorization Server riêng biệt tại một URL khác.

Sự tách biệt này đặc biệt có giá trị trong môi trường doanh nghiệp, nơi các tổ chức có thể chỉ định một centralized Authorization Server duy nhất để xử lý authentication cho tất cả MCP servers, theo đúng OAuth security best practices về quản lý danh tính tập trung. Cách tiếp cận này cho phép doanh nghiệp tích hợp hạ tầng Single Sign-On (SSO) hiện có, giúp người dùng truy cập bất kỳ MCP server nào bằng corporate credentials tiêu chuẩn. Các đội ngũ bảo mật được hưởng lợi nhờ có một điểm kiểm soát duy nhất để quản lý access policies, audit logging, và credential lifecycle management trên toàn bộ hệ thống MCP. Trong khi đó, các nhóm xây dựng MCP servers có thể tập trung vào năng lực cốt lõi của dịch vụ, vì authentication đã được xử lý bởi hạ tầng quản lý danh tính doanh nghiệp đã được kiểm chứng. Mô hình này tương tự như các triển khai OAuth thành công trong doanh nghiệp cho web applications và APIs, nơi mà quản lý danh tính tập trung đã trở thành tiêu chuẩn de facto để cân bằng giữa bảo mật và khả năng sử dụng.

Vì vậy, MCP authentication specification có thể đáp ứng một loạt kịch bản triển khai khác nhau. Specification đặt ra các yêu cầu cơ bản rõ ràng, đồng thời cung cấp nhiều cách tiếp cận để đáp ứng những yêu cầu này – từ layered authentication giúp giữ nguyên backend system hiện tại, cho đến centralized Authorization Servers có thể phục vụ nhiều MCP implementations. Mỗi service provider có thể lựa chọn cân bằng tối ưu giữa compatibility và customization.

Điều gì tiếp theo cho MCP Authentication?

Bản phát hành 2025-06-18 của MCP authentication specification đã đặt nền tảng cho các kịch bản interactive, giúp người dùng dễ dàng kết nối với các dịch vụ MCP. Tuy nhiên, khi việc áp dụng MCP ngày càng mở rộng, những thách thức mới xuất hiện đòi hỏi phải có sự cân nhắc bổ sung.

Khi AWS tiếp tục xây dựng dựa trên nền tảng MCP của Anthropic, một trọng tâm lớn là làm thế nào để cloud infrastructure có thể hỗ trợ tốt nhất cho sự phát triển của giao thức này. Một lĩnh vực đáng chú ý là hỗ trợ cho các autonomous agents — những kịch bản không có người dùng tương tác trực tiếp. Các tương tác workload-to-workload thường dựa trên các mẫu OAuth khác, chẳng hạn như client credentials flow, cho phép dịch vụ lấy access tokens mà không cần sự tham gia của người dùng. Mặc dù specification hiện tại có đề cập đến OAuth client secrets như một cách tiếp cận, nhưng ngành công nghiệp đã rút ra nhiều bài học khó khăn về những thách thức bảo mật khi quản lý long-lived credentials.

Một hướng đi hứa hẹn hơn là tận dụng JWT-based assertions thay cho client secrets, như được định nghĩa trong RFC 7523 – JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants. Cách tiếp cận này ngày càng phổ biến vì nhiều lý do: JWTs thường có vòng đời ngắn hơn so với client secrets và có thể tự động xoay vòng (automatic rotation), giảm thiểu rủi ro bảo mật liên quan đến việc lộ credentials. Chúng cũng phù hợp với các chuẩn nhận dạng workload hiện đại như SPIFFE, nơi JWTs tự động có sẵn cho workloads với cơ chế quản lý vòng đời tích hợp. Việc gia tăng sử dụng JWT-based authentication còn được phản ánh qua hoạt động của IETF WIMSE (Workload Identity in Multi-System Environments) working group, nơi đang phát triển thêm các tiêu chuẩn cho workload identity representation sử dụng JWTs. Những tiến triển này mở ra con đường để MCP clients có thể authenticate với MCP servers bằng các thực tiễn hiện đại, an toàn và tránh được các thách thức của việc quản lý long-lived credentials.

Xa hơn nữa, các triển khai MCP được kỳ vọng sẽ phát triển theo hướng multi-hop scenarios, nơi nhiều agents hợp tác để xử lý các tác vụ phức tạp. Những triển khai này sẽ cần propagate context quan trọng qua từng hop – ví dụ như duy trì mối quan hệ “on-behalf-of” để theo dõi xem hành động bắt nguồn từ một người dùng tương tác hay một hệ thống tự động, đồng thời duy trì audit trails nhất quán trên toàn bộ chuỗi ủy quyền (delegation chain). Điều này sẽ đòi hỏi các OAuth profiles mới, được thiết kế riêng cho nhu cầu độc đáo của agent-based ecosystems.

Kết luận

MCP authentication phản ánh sự cân bằng cẩn trọng giữa simplicity và security. Bắt đầu từ thách thức ban đầu là làm cho kết nối trở nên dễ dàng với người dùng, qua những cân nhắc thực tiễn khi triển khai, đến nhu cầu mới của autonomous systems, specification này vẫn đang tiếp tục phát triển. Dù hiện tại nó đã thực hiện được lời hứa cốt lõi về sự đơn giản “plug-and-play”, nền tảng mà nó tạo ra sẽ hỗ trợ cho các trường hợp sử dụng ngày càng tinh vi khi hệ sinh thái MCP mở rộng.

Cách tiếp cận hợp tác giữa Anthropic (người tạo MCP) và AWS (đối tác triển khai quan trọng) cho thấy cách mà open standards có thể phát triển nhờ sự đóng góp của cộng đồng, đồng thời vẫn duy trì tầm nhìn cốt lõi của chúng. Khi MCP ngày càng trưởng thành, mô hình hợp tác này giữa các protocol designers và infrastructure providers sẽ là yếu tố then chốt để xây dựng các hệ thống AI vừa mạnh mẽ vừa dễ tiếp cận.

Đối với những ai quan tâm muốn tìm hiểu sâu hơn về các chủ đề này, MCP Summit gần đây đã tổ chức nhiều phiên thảo luận hữu ích mở rộng các khái niệm được đề cập ở đây. Các buổi nói chuyện này cung cấp ví dụ thực tiễn và bối cảnh kỹ thuật giúp hiểu rõ cả “cách thức” và “lý do” của MCP authentication:

Intro to OAuth for MCP Servers: cung cấp hướng dẫn chi tiết về các authorization flows.

From Experiment to Enterprise: How Block Operationalized MCP at Scale: chia sẻ một câu chuyện triển khai thực tế, nhấn mạnh tầm quan trọng của simple authentication để đạt được sự chấp nhận rộng rãi trong doanh nghiệp.

MCP 201: The Protocol in Depth mang đến góc nhìn nội bộ từ một trong những đồng tác giả của MCP, giải thích các quyết định thiết kế và sự tiến hóa của specification.

Khi các MCP SDKs bổ sung hỗ trợ cho các cải tiến authentication mới nhất, điều này sẽ cho phép remote hosting của MCP servers, bao gồm cả trên AWS. Với các nhóm đang tìm cách triển khai những khả năng này, tài liệu MCP Authorization và Security Best Practices cung cấp hướng dẫn thiết yếu. Community feedback đã đóng vai trò quan trọng trong việc định hình công nghệ này, và chúng tôi tiếp tục học hỏi từ khách hàng về nhu cầu của họ đối với secure AI integrations và agent interoperability. Bạn được chào đón chia sẻ ý kiến và trải nghiệm của mình trong MCP Discussions.

Darin McAdams

Darin McAdams là Senior Principal Engineer tại AWS. Trong suốt 25 năm làm việc tại Amazon, Darin đã tham gia nhiều mảng khác nhau của Amazon.com và AWS, từ các hệ thống đặt hàng đến identity and access management (IAM). Ông là đồng tác giả của ngôn ngữ Cedar Policy mã nguồn mở và hiện tập trung vào việc hỗ trợ IAM cho agentic workloads. Ông cũng 
tích cực tham gia các nhóm làm việc của OpenID Foundation và IETF OAuth, đóng góp vào việc phát triển các tiêu chuẩn về nhận dạng (identity standards).


Elie Schoppik

Elie Schoppik dẫn dắt mảng live education tại Anthropic với vai trò Head of Technical Training. Ông đã có hơn một thập kỷ trong lĩnh vực giáo dục kỹ thuật, làm việc với nhiều trường đào tạo lập trình và từng tự thành lập một trường của riêng mình. 
Với nền tảng về consulting, education, và software engineering, Elie mang đến cách tiếp cận thực tiễn trong việc giảng dạy Software Engineering và AI. Ông đã chia sẻ những hiểu biết của mình tại nhiều hội nghị kỹ thuật cũng như các trường đại học danh tiếng như MIT, Columbia, Wharton, và UC Berkeley.


