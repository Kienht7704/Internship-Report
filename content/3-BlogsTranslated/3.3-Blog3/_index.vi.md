---
title: "Blog 3"
date: "2025-09-09T19:53:52+07:00"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Giới thiệu AWS Cloud Control API MCP Server: Quản lý hạ tầng bằng ngôn ngữ tự nhiên trên AWS

Tác giả: Kevon Mayers và Brian Terry
Ngày xuất bản: 13 tháng 8, 2025
Nguồn: [Introducing AWS Cloud Control API MCP Server: Natural Language Infrastructure Management on AWS | AWS DevOps & Developer Productivity Blog](https://aws.amazon.com/blogs/devops/introducing-aws-cloud-control-api-mcp-server-natural-language-infrastructure-management-on-aws/)

Hôm nay, chúng tôi chính thức công bố [AWS Cloud Control API (CCAPI) MCP Server](https://awslabs.github.io/mcp/servers/ccapi-mcp-server/). MCP Server này cách mạng hóa việc quản lý hạ tầng AWS bằng cách cho phép các nhà phát triển tạo (create), đọc (read), cập nhật (update), xóa (delete) và liệt kê (list) tài nguyên sử dụng câu lệnh ngôn ngữ tự nhiên. Đây là một phần của dự án [awslabs/mcp](https://github.com/awslabs/mcp) — công cụ sáng tạo mới giúp kết nối giữa các lệnh giao tiếp thông thường và việc triển khai, quản lý hạ tầng trên AWS.
CCAPI MCP Server được hỗ trợ bởi [AWS Cloud Control API](https://aws.amazon.com/cloudcontrolapi/) — một API chuẩn cho phép thực hiện các thao tác CRUDL (Create/Read/Update/Delete/List)  trên các tài nguyên AWS và bên thứ ba thông qua một điểm truy cập chung.

---

## Các tính năng chính

- Sử dụng AWS Cloud Control API để thực hiện các thao tác CRUDL cho hơn 1.200 tài nguyên AWS


- Cho phép các agent dựa trên mô hình ngôn ngữ lớn (LLM) và các nhà phát triển quản lý hạ tầng bằng câu lệnh ngôn ngữ tự nhiên


- Cung cấp tùy chọn xuất mẫu Infrastructure as Code (IaC) cho hạ tầng mà nó sẽ tạo ra, để vẫn có thể tích hợp với pipeline CI/CD hiện có


- Tích hợp với AWS Pricing API để cung cấp ước tính chi phí cho hạ tầng sẽ được tạo


- Tự động áp dụng các thực hành an ninh tốt nhất thông qua [Checkov](https://www.checkov.io/)


---
## Tại sao sử dụng CCAPI MCP Server?
- Quản lý hạ tầng đơn giản hơn: không còn phải đối phó với các mẫu cấu hình phức tạp hay tài liệu dài dòng


- Tăng năng suất cho nhà phát triển: tập trung vào điều bạn muốn làm, không phải cách cấu hình


- Giảm đường cong học tập: giúp các thành viên mới trong nhóm dễ làm quen hơn với các tác vụ hạ tầng thông qua lệnh tự nhiên


- Tích hợp với LLM: là người bạn đồng hành phù hợp cho quy trình phát triển hỗ trợ AI


MCP Server này cho phép các nhà phát triển quản lý hạ tầng đám mây thông qua các câu lệnh trò chuyện như:
- “Bạn có thể tạo giúp tôi một bucket S3 mới không?” hoặc
- “Tìm tất cả các instance EC2 của tôi và cho biết cái nào có kiểu instance không phải t2.large”
Nhờ vậy, áp lực cấu hình giảm đi rất nhiều và việc chuyển ý định của nhà phát triển thành hạ tầng đám mây trở nên trực tiếp hơn.


---

## Cách tạo và quản lý hạ tầng đám mây

### Yêu cầu trước

- Trình quản lý gói uv đã cài


- Python phiên bản 3.x.x


- Thông tin xác thực AWS (AWS credentials) với quyền thích hợp. Máy chủ MCP hỗ trợ nhiều cách khác nhau để xác định thông tin xác thực này. Vui lòng xem tài liệu của MCP để biết thêm chi tiết. Khuyến nghị: nên sử dụng thông tin xác thực động (dynamic credentials), chẳng hạn như được cấp thông qua SSO (Single Sign-On). Để biết thêm thông tin về cách cấu hình thông tin xác thực AWS, hãy xem tài liệu AWS CLI.


- Một ứng dụng MCP Host đã được cài đặt, có khả năng hỗ trợ cả MCP Clients và MCP Servers (ví dụ: Amazon Q Developer, Claude Desktop, Cursor, v.v.). Để thực hành theo hướng dẫn trong bài viết này, hãy cài đặt Amazon Q Developer cho CLI theo hướng dẫn cài đặt được mô tả trong tài liệu chính thức.

---

## Tích hợp với công cụ phát triển

Để bắt đầu sử dụng CCAPI MCP Server, bạn cần thiết lập cấu hình máy chủ của mình — thông thường được lưu trong một tệp có tên là mcp.json.
Trong bài viết này, chúng ta sẽ tập trung vào cách sử dụng CCAPI MCP Server với [Amazon Q Developer](https://aws.amazon.com/q/developer/). Lưu ý rằng đối với các ứng dụng MCP Host khác, đường dẫn đến tệp cấu hình MCP có thể khác nhau.Nếu tệp này chưa tồn tại trong thư mục làm việc, bạn sẽ cần tạo mới.
Cấu hình có thể được đặt tại hai vị trí chính:
Cấu hình toàn cục (Global Configuration):
~/.aws/amazon/mcp.json – Áp dụng cho tất cả workspace.


Cấu hình workspace (Workspace Configuration):
.amazonq/mcp.json – Áp dụng riêng cho workspace hiện tại.


Bạn có thể tìm thêm thông tin chi tiết trong [Amazon Q Developer User Guide.](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/what-is.html)

---

## Cấu trúc tệp cấu hình

Tệp cấu hình MCP sử dụng định dạng JSON với cấu trúc như sau: 

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.png" style="width: 100%;" />
</div>

Ví dụ cho mcp.json với cấu hình CCAPI MCP Server:

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.1.png" style="width: 100%;" />
</div>

---

## Quan trọng

Hãy đảm bảo bạn thiết lập đúng thông tin xác thực AWS trong cấu hình của máy chủ MCP. Việc cấu hình chính xác thông tin xác thực này là rất cần thiết, vì máy chủ MCP sẽ sử dụng các quyền liên quan khi gọi AWS Cloud Control API để thực hiện các thao tác CRUDL trong tài khoản AWS của bạn. Máy chủ hỗ trợ nhiều phương thức khác nhau để sử dụng các thông tin xác thực này, chẳng hạn như AWS profiles, biến môi trường (Environment Variables), token SSO, v.v. Bạn có thể xem một phần của nội dung này trong tệp aws_client.py. Tham khảo tài liệu hướng dẫn về sử dụng named profiles để biết thêm thông tin chi tiết.

---

## Chế độ chỉ đọc (read-only)

Nếu bạn muốn MCP server không thực hiện các hành động thay đổi (Create/Update/Delete Resource), bạn có thể thêm flag--readonly vào cấu hình:

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.2.png" style="width: 100%;" />
</div>

---

## Cân nhắc bảo mật

- Hãy đảm bảo rằng thông tin xác thực IAM có đủ quyền thực hiện các hành động của Cloud Control API (List, Get, Create, Update, Delete). Tham khảo tài liệu AWS CCAPI API để biết thêm chi tiết.
- Tuân thủ nguyên tắc đặc quyền tối thiểu (least privilege) khi cấu hình quyền IAM.
- Bật AWS CloudTrail để ghi lại và giám sát các hoạt động.
- Cân nhắc chạy ở chế độ chỉ đọc (read-only) bằng cách thêm --readonly flag để đảm bảo vận hành an toàn hơn.

## Ví dụ trường hợp sử dụng: Tạo một S3 Bucket với mã hóa KMS
- **Lưu ý quan trọng:** Hãy đảm bảo rằng bạn đã đáp ứng tất cả các yêu cầu tiên quyết (prerequisites) trước khi thực hiện các lệnh sau.
  -  Khi tệp mcp.json đã được cấu hình chính xác, hãy thử chạy một ví dụ . Trong terminal của bạn, chạy lệnh: q chat để bắt đầu sử dụng Amazon Q trong giao diện dòng lệnh (CLI).

 <div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.3.png" style="width: 100%;" />
</div>

- Lệnh này sẽ bắt đầu khởi tạo các máy chủ MCP ở chế độ nền, cho phép bạn bắt đầu sử dụng Q Chat ngay lập tức ngay cả khi các máy chủ vẫn đang trong quá trình tải. Lưu ý: nếu các máy chủ MCP chưa tải xong, các prompt (lệnh hoặc yêu cầu) của bạn sẽ được xử lý mà không sử dụng bất kỳ máy chủ MCP nào. Để kiểm tra trạng thái của các máy chủ, hãy chạy lệnh: /mcp

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.4.png" style="width: 100%;" />
</div>

- Khi bạn đã xác nhận rằng máy chủ MCP đã được tải thành công, hãy thử một lệnh mẫu. Chỉ cần nói với Amazon Q: Create an S3 bucket with versioning and encrypt it using a new KMS key

**Amazon Q** sẽ tự động sử dụng máy chủ để thực hiện các bước sau:
- Lấy các biến môi trường hiện tại của bạn.


- Sử dụng chúng để truy xuất thông tin phiên AWS hiện tại.


- Tạo mã (code) mô tả nội dung mà bạn đã yêu cầu trong prompt.


- Giải thích đoạn mã vừa được tạo.


- Chạy phân tích bảo mật đối với đoạn mã đã tạo (nếu tính năng này được bật).


- Giải thích kết quả của phân tích bảo mật.


- Xác thực cấu hình dựa trên AWS Cloud Control API schemas
(các schema này được xây dựng dựa trên CloudFormation Resource Provider Schemas) và chính sách IAM. Việc xác thực này đảm bảo tuân thủ các yêu cầu của Cloud Control API, điều rất quan trọng để có thể tạo tài nguyên thành công.


- Tạo các tài nguyên trực tiếp thông qua Cloud Control API.

#### Lưu ý:
Mặc dù các schema của CloudFormation được tham chiếu trong bước xác thực,
giải pháp này không sử dụng CloudFormation để quản lý tài nguyên.
Thay vào đó, Cloud Control API được dùng cho việc quản lý tài nguyên,
và các schema CloudFormation chỉ được sử dụng vì chúng định nghĩa cấu trúc chuẩn hóa của tài nguyên mà Cloud Control API yêu cầu.
- Trước hết, Amazon Q sẽ thông báo rằng nó cần kiểm tra các biến môi trường để tìm thông tin liên quan đến phiên AWS hiện tại. Sau đó, nó sẽ cho bạn biết công cụ cụ thể mà nó sẽ sử dụng và yêu cầu bạn cho phép thực hiện hành động đó. Khi được hỏi, hãy nhập y để chấp nhận và cho phép Amazon Q tiếp tục thực hiện.
<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.5.png" style="width: 100%;" />
</div>

- Tiếp theo, Amazon Q sẽ yêu cầu sử dụng hàm get_aws_session_info() để lấy thông tin về phiên AWS mà nó sẽ dùng cho các thao tác tiếp theo. Công cụ này sẽ sử dụng các giá trị liên quan được định nghĩa trong tệp cấu hình MCP, ví dụ: ~/.aws/amazon/mcp.json, để truy xuất thông tin từ các biến môi trường (environment variables) tương ứng.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.6.png" style="width: 100%;" />
</div>

- Sau đó, Amazon Q sẽ hiển thị ID tài khoản AWS và khu vực (region) mà nó sẽ sử dụng để triển khai tài nguyên. Tiếp theo, công cụ sẽ dùng hàm generate_infrastructure_code() để tạo mã mô tả các thuộc tính tài nguyên cho khóa KMS, và mã này sẽ được gửi đến Cloud Control API. Các thuộc tính này phản ánh cấu trúc được định nghĩa trong AWS CloudFormation Resource Provider Schemas (vốn là nền tảng của Cloud Control API), cho phép thực hiện kiểm tra bảo mật bằng Checkov trước khi triển khai.Khóa KMS sẽ được cấu hình tuân theo các thực hành bảo mật tốt nhất, với chính sách khóa (key policy) được giới hạn chỉ cho phép sử dụng trong tài khoản AWS hiện tại.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.7.png" style="width: 100%;" />
</div>

- Sau khi Amazon Q đã tạo xong mã cho tài nguyên, nó sẽ chạy và sử dụng công cụ explain() để giải thích đoạn mã hạ tầng (infrastructure code) vừa được tạo ra. Lưu ý rằng, các thẻ mặc định (default tags) sau sẽ được tự động thêm vào cho mọi tài nguyên được quản lý bởi CCAPI MCP Server: MANAGED_BY, MCP_SERVER_SOURCE_CODE, MCP_SERVER_VERSION Các thẻ này giúp dễ dàng nhận diện các tài nguyên đang được MCP Server quản lý. Chúng có thể được tùy chỉnh hoặc vô hiệu hóa, tuy nhiên rất khuyến khích giữ lại hoặc thêm thẻ nhận dạng, để đảm bảo bạn có tầm nhìn rõ ràng về các phần hạ tầng đang được CCAPI MCP Server quản lý.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.8.png" style="width: 100%;" />
</div>

- Sau đó, Amazon Q sẽ cố gắng sử dụng công cụ run_checkov() để kiểm tra mức độ an toàn (bảo mật) của đoạn mã đã được tạo. Công cụ này được kích hoạt tự động vì trong tệp cấu hình máy chủ của bạn, tham số SECURITY_SCANNING đã được thiết lập ở trạng thái bật.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.9.png" style="width: 100%;" />
</div>

- Sau khi Checkov hoàn tất quá trình kiểm tra, Amazon Q sẽ tiếp tục sử dụng công cụ explain() để giải thích các phát hiện bảo mật từ kết quả kiểm tra của Checkov. Nếu không có vấn đề bảo mật nào, quá trình sẽ tự động tiếp tục. Ngược lại, nếu phát hiện lỗi bảo mật, bạn sẽ được hỏi cách muốn tiếp tục, và Amazon Q sẽ đề xuất các bản sửa lỗi cần thiết. Theo mặc định, các kiểm tra đã vượt qua sẽ chỉ hiển thị tóm tắt ngắn gọn. Nếu bạn muốn xem thông tin chi tiết hơn, chỉ cần yêu cầu hiển thị thêm.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.10.png" style="width: 100%;" />
</div>

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.11.png" style="width: 100%;" />
</div>

- Công cụ tiếp theo mà Amazon Q sẽ sử dụng là create_resource(). Công cụ này sẽ thực hiện việc tạo tài nguyên thông qua AWS Cloud Control API, sau đó sử dụng get_resource_request_status() để kiểm tra trạng thái của quá trình tạo. Công cụ get_resource_request_status() sẽ dùng mã định danh yêu cầu (request token) để xác định yêu cầu đã được gửi đến Cloud Control API, và từ đó truy xuất thông tin trạng thái của quá trình tạo tài nguyên đó.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.12.png" style="width: 100%;" />
</div>

- Amazon Q sẽ tiếp tục sử dụng các công cụ của CCAPI MCP Server khi cần thiết cho đến khi hoàn tất việc tạo cả S3 Bucket và KMS Key, sau đó sẽ xuất ra một bản tóm tắt (summary) về quá trình thực hiện.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.13.png" style="width: 100%;" />
</div>

- Bây giờ, hãy yêu cầu Amazon Q thực hiện một thay đổi có thể ảnh hưởng tiêu cực đến bảo mật, ví dụ: cho phép S3 bucket được truy cập công khai (publicly accessible). Mặc dù cấu hình này thường không được khuyến khích, nhưng đôi khi nó vẫn cần thiết — chẳng hạn khi bạn muốn dùng S3 bucket để lưu trữ và phục vụ một trang web công khai. Amazon Q sẽ phản hồi bằng cách thông báo rằng yêu cầu của bạn không tuân theo các thực hành bảo mật tốt nhất và giải thích lý do vì sao. Tuy nhiên, vì trong một số trường hợp yêu cầu này vẫn hợp lệ tùy theo mục đích sử dụng, Amazon Q sẽ yêu cầu bạn xác nhận trước khi tiếp tục thực hiện thay đổi đó.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.14.png" style="width: 100%;" />
</div>

- CCAPI MCP Server cũng được tích hợp với AWS Pricing API, vì vậy bạn thậm chí có thể yêu cầu ước tính chi phí cho những tài nguyên mà nó đã triển khai.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.15.png" style="width: 100%;" />
</div>

- Cuối cùng, hãy yêu cầu Amazon Q tạo một mẫu CloudFormation (CloudFormation template) cho những gì nó đã tạo cho đến thời điểm hiện tại. Điều này giúp bạn có thể lưu bản sao dự phòng (backup) hoặc tái triển khai (redeploy) một cấu hình tương tự trong tương lai, vì bạn sẽ có sẵn một mẫu (template) để sử dụng. Amazon Q sẽ dùng công cụ create_template() để thực hiện tác vụ này.

Lưu ý: Công cụ create_template() có các thiết lập mặc định sau:
- Xuất ra định dạng YAML theo mặc định (có thể đổi sang JSON nếu muốn).


- Thiết lập DeletionPolicy = RETAIN.


- Thiết lập UpdateReplacePolicy = RETAIN.


- Cho phép tùy chọn các tham số bổ sung, như:


  - ID của template,


  - vị trí lưu tệp,


  - và vùng (region) được chỉ định.


Để biết thêm chi tiết, hãy xem phần source code của công cụ này.
<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.16.png" style="width: 100%;" />
</div>

- Hãy thử thêm một thao tác nguy hiểm nữa, cố gắng xóa tất cả các tài nguyên trong một tài khoản AWS. Các kiểm tra bảo mật sẽ chặn nỗ lực này và gợi ý các phương án thay thế khác.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.17.png" style="width: 100%;" />
</div>

-Cuối cùng, hãy yêu cầu Amazon Q xóa các tài nguyên mà nó đã tạo trước đó.
Lần này, công cụ sẽ thực hiện theo các bước sau:
- Sử dụng get_resource() để lấy thông tin về các tài nguyên hiện có mà nó đã tạo.


- Dùng explain() để giải thích những thay đổi sẽ được thực hiện.


- Và cuối cùng, sử dụng delete_resource() để xóa các tài nguyên đó.
<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.18.png" style="width: 100%;" />
</div>

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.19.png" style="width: 100%;" />
</div>
Sau khi xóa thành công các tài nguyên, Amazon Q sẽ hiển thị một bản tóm tắt cuối cùng.
<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog3.20.png" style="width: 100%;" />
</div>

## Các lệnh gợi ý mẫu để bắt đầu dễ dàng

### Lệnh mẫu
- “Create a VPC with private and public subnets”
- “List all my EC2 instances”
- “Create a serverless API for my application”
- “Set up a load-balanced web application”
### Chức năng
- Thiết lập một môi trường mạng hoàn chỉnh với các subnet riêng tư và công khai

- Hiển thị tất cả các phiên bản EC2 đang chạy trong tài khoản của bạn

- Triển khai API Gateway tích hợp với Lambda (mô hình không máy chủ)
- Tạo Ứng dụng Web có cân bằng tải (ALB) với nhóm đích và các instance

## Kết Luận
AWS Cloud Control API MCP Server là bước tiến quan trọng trong quản lý hạ tầng AWS, giúp người dùng dễ dàng thao tác tài nguyên đám mây thông qua ngôn ngữ tự nhiên. Dù bạn đang tối ưu hóa vận hành, thử nghiệm cùng AI, hay onboard thành viên mới — dù dùng Amazon Q Developer CLI hay bất cứ công cụ MCP Host nào (Claude Desktop, Cursor, v.v.) — CCAPI MCP Server cùng các công cụ đi kèm mang lại cách tương tác trực giác với AWS.
<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog2.2.png" style="width: 100%;" />
</div>

