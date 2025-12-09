---
title: "Blog 2"
date: "2025-09-09T19:53:52+07:00"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# Tính linh hoạt đến khung làm việc: Xây dựng máy chủ MCP với điều phối công cụ có kiểm soát

Tác giả: Kevon Mayers
Ngày xuất bản: 13 tháng 8, 2025
Nguồn: [Flexibility to Framework: Building MCP Servers with Controlled Tool Orchestration | AWS DevOps & Developer Productivity Blog](https://aws.amazon.com/blogs/devops/flexibility-to-framework-building-mcp-servers-with-controlled-tool-orchestration/)

[MCP (Model Context Protocol)](https://modelcontextprotocol.io/specification/2025-03-26) là một giao thức được thiết kế để chuẩn hóa cách tương tác với các mô hình Generative AI, giúp việc xây dựng và quản lý ứng dụng AI dễ dàng hơn. Giao thức này cung cấp một cách nhất quán để truyền ngữ cảnh giữa ứng dụng và mô hình, bất kể mô hình được triển khai ở đâu hay theo cách nào. Giao thức giúp thu hẹp khoảng cách giữa việc triển khai mô hình và phát triển ứng dụng bằng cách cung cấp một giao diện thống nhất cho các tương tác với mô hình.
Mặc dù giao thức MCP mang lại tính linh hoạt trong lựa chọn công cụ, vẫn có những thách thức quan trọng khi cần áp đặt thứ tự sử dụng công cụ. Trong bài viết này, bạn sẽ tìm hiểu cách tôi thiết kế chức năng này và triển khai nó trong [máy chủ AWS Cloud Control API (CCAPI) MCP.](https://awslabs.github.io/mcp/servers/ccapi-mcp-server/)

---

## Thách thức – Áp đặt thứ tự công cụ trong MCP

Khi nghĩ đến MCP, bạn dễ tưởng tượng đến lựa chọn. Một trong những lý do chính khiến bạn muốn dùng máy chủ MCP là để cho một mô hình ngôn ngữ lớn (LLM), thông qua agent, có thể truy cập tập các công cụ như đọc từ cơ sở dữ liệu, gửi email, hay các chức năng tương tự. Tuy nhiên, framework MCP không cung cấp cơ chế nội tại để ép thứ tự mà các công cụ phải được gọi.
Ví dụ: có hai công cụ fetch_weather_data() và send_email(). Bạn muốn đảm bảo rằng email gửi ra có kèm theo thời tiết hiện tại — nghĩa là fetch_weather_data() phải chạy trước send_email(). Hoặc một ví dụ khác: getOrderId() và getOrderDetail(), trong đó OrderId cần được lấy trước để sau đó mới lấy được thông tin chi tiết đơn hàng. Vì MCP hiện tại không có cách định nghĩa sở thích thứ tự công cụ (tool ordering), những mối liên kết theo trình tự như vậy rất khó ép buộc.
Các công cụ trong MCP được thiết kế như những hàm độc lập mà LLM có thể gọi khi cần. Không có khái niệm “luồng công việc (workflow)” hay “sequence” bên trong framework MCP tự nó. Mỗi lần gọi công cụ được xem như một tác vụ riêng biệt, không có kiến thức nội tại về cái gì đã diễn ra trước hay sẽ diễn ra sau. Kết quả là, theo mặc định, LLM có thể gọi các công cụ theo bất kỳ thứ tự nào, bất chấp logic mà bạn mong muốn.
Trong khi LLM rất giỏi đưa ra quyết định linh hoạt, một số trường hợp như quản lý hạ tầng (infrastructure) đòi hỏi thứ tự hoạt động nghiêm ngặt. Điều này đặt ra thách thức khi xây dựng MCP server: làm thế nào để duy trì tính linh hoạt của LLM nhưng vẫn đảm bảo thứ tự các công cụ quan trọng?
Khi bạn nghĩ đến Infrastructure as Code (IaC), bạn nghĩ đến khả năng tái lặp, tính nhất quán, quản lý phiên bản, và tích hợp/triển khai liên tục (CI/CD). Trong CI/CD có một thứ tự cố định:

- Pull request được tạo


- Pipeline CI/CD được kích hoạt


- Một loạt bước chạy để linting, kiểm tra bảo mật, kiểm thử đơn vị, kiểm thử đầu-cuối, v.v.


- Nếu bất kỳ bước nào thất bại thì pipeline dừng


Điều này gây khó khăn với bản chất không xác định của AI: AI sinh là không định trước — cùng một prompt có thể không luôn cho ra kết quả giống nhau. Nếu kết quả lệch quá nhiều so với mong muốn, đó được xem là một “ảo giác” (hallucination). Vậy làm thế nào để hướng dẫn LLM làm điều bạn mong muốn? Hãy xem cách vấn đề này được giải quyết trong MCP server CCAPI.

---

## Hiểu về khám phá và khởi tạo công cụ trong MCP
Trước khi đi vào giải pháp, ta cần hiểu cách MCP server liên lạc với các AI Agent. Trong quá trình khởi tạo, MCP định nghĩa các giai đoạn vòng đời mà khả năng và công cụ được khám phá.
Mô hình ngữ cảnh MCP định nghĩa một vòng đời có cấu trúc cho kết nối giữa client và server nhằm đảm bảo đàm phán khả năng (capability) và quản lý trạng thái. Các giai đoạn gồm:

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog2.png" style="width: 100%;" />
</div>

- Initialization (Khởi tạo): đàm phán khả năng và phiên bản giao thức


- Operation (Hoạt động bình thường): giao tiếp thông thường qua protocol


- Shutdown (Kết thúc): đóng kết nối một cách mềm mại


Giai đoạn khởi tạo xác lập khả năng tương thích và chia sẻ chi tiết triển khai. Đây là lúc AI Agent biết về các công cụ có sẵn thông qua các định nghĩa schema, đồng thời nhận hướng dẫn sử dụng công cụ. Quá trình khởi tạo cực kỳ quan trọng vì công cụ được khám phá ngay từ lúc này. Trong giai đoạn này, client gửi thông tin về phiên bản protocol, khả năng và chi tiết triển khai.
Ví dụ: công cụ như Amazon Q CLI nhận thông tin về phiên bản MCP server, công cụ khả dụng và hướng dẫn sử dụng thông qua schema gửi từ server.
Chú thích: xem thêm thông tin qua các [tài liệu này](https://modelcontextprotocol.io/specification/2025-06-18/basic/lifecycle).
---

## Giải pháp – Tổ chức công cụ dựa trên token: một mẫu mới cho AI Agent trong MCP

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog2.1.png" style="width: 100%;" />
</div>

MCP gặp thách thức: các công cụ không thể trực tiếp giao tiếp với nhau để ép thứ tự thực thi. Máy chủ CCAPI MCP giải quyết vấn đề này bằng cách sử dụng mẫu truyền tin token (token messenger pattern), nơi server sinh và kiểm soát các token xác minh, và AI Agent (client của MCP) chuyển các token đó giữa các lần gọi công cụ.                                 |

---

## Triển khai chính:
- **Mở rộng hàm (Function Enhancement)**
Decorator @mcp.tool() biến mỗi hàm thành một thực thể mạnh hơn. Nó bao bọc hàm với schema xác định các đầu vào bắt buộc và quy tắc xác minh, đồng thời giữ nguyên docstring (hướng dẫn chi tiết). Mỗi hàm được mở rộng rõ ràng về yêu cầu của nó và đưa ra lỗi rõ ràng nếu phụ thuộc không thỏa.


- **Khám phá phụ thuộc (Dependency Discovery)**
Trong giai đoạn khởi tạo MCP, AI Agent (client) nhận bản đồ đầy đủ các công cụ và schema từ MCP server. LLM, như thành phần trong Agent, sử dụng schema này để hiểu phụ thuộc thông qua cả mô tả tham số và đầu vào bắt buộc.
 Ví dụ: nếu một công cụ yêu cầu tham số mô tả “Kết quả từ get_aws_session_info()” và định nghĩa security_scan_token là đầu vào bắt buộc, LLM sẽ hiểu rằng nó cần cả hai token hợp lệ trước khi tiếp tục. Sự kết hợp giữa mô tả văn bản và yêu cầu đầu vào rõ ràng cho phép Agent thực thi chuỗi như get_aws_session_info() → generate_infrastructure_code() → run_checkov() → create_resource().


- **Kiểm tra và xác minh token (Token Validation Control)**
Server sinh và kiểm soát tất cả token luồng công việc qua hệ thống lưu trữ tập hợp (_workflow_store). Mỗi công cụ trong workflow sinh token bảo mật và token này được lưu phía server với dữ liệu liên quan.

 AI Agent giữ các token trong ngữ cảnh trò chuyện và chuyển chúng giữa các lần gọi công cụ. Mỗi token do Agent sử dụng phải được xác minh với kho lưu trữ phía server. Vì token có thời hạn ngắn, chúng được lưu trong bộ nhớ (RAM), được quản lý chủ động, và bị xóa sau khi dùng để giữ tính mới. Bất kỳ token nào còn sót sẽ bị xoá khi tiến trình server kết thúc hoặc khởi động lại.

 Nếu token không tồn tại trong kho lưu trữ (vì không hợp lệ hoặc đã bị dùng), thao tác sẽ ngay lập tức thất bại với lỗi. Cách này áp dụng với mọi loại token, đảm bảo rằng Agent không thể tạo hoặc sửa token.

 Khi workflow tiến triển, công cụ tiêu thụ token hiện có và sinh token mới. Ví dụ, khi explain() nhận properties_token, nó xác minh nó có tồn tại và khớp với _workflow_store, sau đó tiêu thụ nó và sinh token mới explained_properties_token. Cách làm này tạo ra một chuỗi bảo mật mật mã (cryptographic chain) các thao tác, ép thứ tự workflow (generate → scan → create), kèm xác minh server tại mỗi bước.

 Kết quả là một hệ thống workflow có thể đoán trước với kiểm soát bảo mật mạnh — token phải được sinh bởi server và xác minh với kho lưu trữ phía server tại mỗi bước — giúp đảm bảo thứ tự công cụ và tính toàn vẹn dữ liệu trong thao tác hạ tầng. Cách tiếp cận này mang lại khả năng thực thi workflow đáng tin cậy trong giới hạn của framework FastMCP hiện tại.

 Mặc dù việc định nghĩa phụ thuộc rõ ràng như @mcp.tool(depends_on=["run_checkov"]) như đề cập trong một issue GitHub có thể lý tưởng trong các phiên bản tương lai, cách tiếp cận token kết hợp tên tham số mô tả và xác minh rõ ràng hiện tại đã cung cấp thứ tự công cụ mà LLM tuân theo một cách ổn định.

 ## Giới hạn tiềm ẩn và cách khắc phục

- **Quản lý phiên làm việc (Session Management)**
 Khi phiên Agent kết thúc hoặc làm mới, bất kỳ workflow đang tiến hành nào cũng phải bắt đầu lại. Điều này là theo thiết kế — token có thời hạn ngắn và gắn với chuỗi workflow cụ thể. Credentials AWS tự nhiên hết hạn trong vài giờ như một phần của chính sách bảo mật chuẩn, cung cấp ngưỡng ranh giới tự nhiên cho phiên workflow.


- **Workflow đồng thời (Concurrent Workflows)**
 Mỗi tương tác của Agent hoạt động độc lập, điều này phù hợp để giữ ranh giới bảo mật giữa các phiên workflow khác nhau. Dù điều này có nghĩa mỗi phiên bắt đầu mới, nó đảm bảo sự phân tách rõ ràng giữa các thao tác hạ tầng khác nhau.


- **Tùy chọn triển khai (Implementation Options)**
 Với tổ chức cần lưu trữ trạng thái workflow lâu dài, có thể dùng cơ sở dữ liệu truyền thống để lưu trạng thái phiên giữa các khởi động lại. Tuy nhiên, vì token được thiết kế là kiểm soát bảo mật có thời hạn ngắn, hầu hết triển khai có thể dựa vào lưu trữ trong bộ nhớ (in-memory) với ranh giới phiên tự nhiên.


Mẫu token messenger cung cấp nền tảng vững chắc cho điều phối workflow bảo mật, với token ngắn hạn (ephemeral) nhằm đảm bảo thứ tự công cụ và toàn vẹn dữ liệu khi thao tác với hạ tầng.


---

## Tương lai của MCP

Mặc dù giải pháp trên hoạt động, quy trình này cũng gợi tôi suy nghĩ về tương lai của MCP và cách nó có thể và nên phát triển. Có nhiều cập nhật cho framework mà tôi đã thấy gần đây, và rất vui khi thấy hoạt động cộng đồng. Với Agentic AI nhìn chung, có những dấu hiệu mạnh mẽ cho thấy trong tương lai các nền tảng agentic có thể mang tính xác định hơn (deterministic), như được nhấn mạnh bởi [“hooks” trong Claude Code](https://docs.claude.com/en/docs/claude-code/hooks). Theo tài liệu, “Hooks cung cấp kiểm soát xác định đối với hành vi Claude Code, đảm bảo một số hành động luôn xảy ra thay vì phụ thuộc vào LLM chọn thực hiện.” Với IaC và các công nghệ xác định khác mà ta muốn tích hợp AI, điều này là điều thiết yếu để được chấp nhận quy mô lớn.

---

## Kết luận

Hành trình của Model Control Protocol (MCP) và lĩnh vực mới tận dụng AI để quản lý hạ tầng đám mây tiếp tục tiến hóa, mang cả cơ hội và thách thức trong thế giới điện toán đám mây và trí tuệ nhân tạo. Các cách tiếp cận hiện nay như prompt loading và phụ thuộc tham số đã giúp giải quyết các thách thức ban đầu về thứ tự công cụ và giao thức bảo mật, cho thấy MCP có thể được dùng hiệu quả trong ứng dụng doanh nghiệp.
Trong khi cách triển khai hiện tại sử dụng token workflow và kiểm tra xác minh cung cấp một giải pháp khả thi, chúng ta tiếp tục khám phá cách nâng cao khả năng của giao thức. Nếu bạn quan tâm đóng góp vào sự tiến hoá của MCP, bạn có thể xem đề xuất cải tiến quản lý phụ thuộc, trong tổ chức GitHub của modelcontextprotocol cũng như kho FastMCP.
Nếu bạn muốn tìm hiểu thêm về máy chủ AWS MCP Cloud Control API đề cập trong bài viết, bạn có thể xem tài liệu và kho mã GitHub liên quan. Nếu bạn muốn thực hành với nó và các máy chủ MCP khác, hãy thử workshop AWS được liên kết. Chúc bạn lập trình vibe coding vui vẻ, bạn tôi!

---

## Giới thiệu về tác giả

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog2.2.png" style="width: 100%;" />
</div>

---


