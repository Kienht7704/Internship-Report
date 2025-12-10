---
title: "Bản đề xuất"
date: "2025-09-09T19:53:52+07:00"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Aurora Time  
## Giải pháp AWS Serverless hợp nhất cho quản lý thời gian cá nhân  
[Tài liệu.](https://docs.google.com/document/d/17JlwJN2M94RUWqWC3MY1dweCdifvsekYCMd9Det3rSM/edit?tab=t.0)
### 1. Tóm tắt điều hành  
Bản đề xuất này trình bày kế hoạch triển khai Aurora Time, một ứng dụng quản lý thời gian trên nền tảng AWS, đơn giản, tập trung vào tính năng lập lịch, nhằm giải quyết sự phức tạp và chi phí vận hành của các giải pháp hiện tại. Aurora Time sẽ tận dụng các dịch vụ serverless và managed service của AWS để đảm bảo khả năng mở rộng cao, độ tin cậy và chi phí tối ưu, mang lại lợi tức đầu tư (ROI) nhanh chóng thông qua giảm thiểu chi phí quản lý hạ tầng.  

### 2. Tuyên bố vấn đề  
#### *Vấn đề hiện tại*  
Cá nhân gặp khó khăn trong việc quản lý các cam kết hàng ngày vì lịch trình bị phân tán (ghi chú, điện thoại) dẫn đến dễ bị nhầm lẫn và bỏ sót. Các công cụ hiện tại thường quá phức tạp, nặng về tính năng công việc, không phù hợp cho nhu cầu lập lịch nhanh chóng và đơn giản của cuộc sống cá nhân. Aurora Time giải quyết bằng cách cung cấp một nền tảng tập trung hóa, tối giản và trực quan để dễ dàng theo dõi thói quen và các mốc quan trọng.  

#### *Giải pháp*  
Nền tảng sử dụng Amazon S3 kết hợp với Amazon CloudFront để lưu trữ và phân phối ứng dụng web, sử dụng AWS Amplify để phát triển và triển khai nhanh chóng. Amazon API Gateway và AWS Lambda hoạt động như lớp xử lý Backend để xử lý các yêu cầu CRUD sự kiện. Dữ liệu được lưu trữ trong Amazon DynamoDB để đảm bảo tốc độ truy cập nhanh, độ trễ thấp. Amazon Cognito đảm bảo xác thực và ủy quyền truy cập an toàn cho từng người dùng cá nhân. Amazon EventBridge và Resend được sử dụng để kích hoạt và gửi các thông báo nhắc nhở theo lịch. Tương tự như các ứng dụng lịch khác, người dùng có thể tạo, chỉnh sửa lịch trình cá nhân, nhưng nền tảng này hoạt động ở quy mô tối giản hơn và phục vụ mục đích quản lý thời gian cá nhân hàng ngày. Các tính năng chính bao gồm giao diện lập lịch trực quan, nhắc nhở tùy chỉnh và chi phí vận hành thấp.  

#### *Lợi ích và hoàn vốn đầu tư (ROI)*  
Giải pháp Aurora Time tạo nền tảng vững chắc để người dùng cá nhân tập trung hóa mọi lịch trình, đồng thời cung cấp một mô hình kiến trúc Serverless tiết kiệm chi phí để dễ dàng mở rộng tính năng. Nền tảng này giảm bớt sự phân tán lịch trình và giảm thiểu việc bỏ sót các cam kết quan trọng thông qua hệ thống nhắc nhở tập trung, đơn giản hóa quản lý thời gian cá nhân và cải thiện sự cân bằng giữa công việc/cuộc sống.

Chi phí hạ tầng hàng tháng ước tính $16 - $50 USD, tổng cộng khoảng $192 - $600 USD cho 12 tháng (tùy thuộc vào lượng người dùng và request). Tất cả các thành phần phát triển đều dựa trên Serverless, không phát sinh chi phí mua sắm phần cứng hoặc thuê máy chủ ảo 24/7. Thời gian hoàn vốn được ước tính dưới 6 tháng nhờ tiết kiệm đáng kể thời gian tìm kiếm, sắp xếp lịch trình thủ công, và chi phí vận hành cực thấp của kiến trúc AWS Serverless.  

### 3. Kiến trúc giải pháp  
Nền tảng áp dụng kiến trúc AWS Serverless để quản lý dữ liệu lịch trình và sự kiện cá nhân, có khả năng dễ dàng mở rộng từ một người dùng lên hàng triệu người dùng cá nhân. Các yêu cầu API được tiếp nhận qua Amazon API Gateway và xử lý bởi AWS Lambda, trong khi dữ liệu được lưu trữ trong Amazon DynamoDB để đảm bảo tốc độ truy vấn nhanh chóng, độ trễ thấp. Amazon EventBridge xử lý logic nhắc nhở theo lịch, kích hoạt Lambda và gửi thông báo. AWS Amplify cung cấp giao diện web/di động trực quan, được bảo mật bởi Amazon Cognito để quản lý quyền truy cập an toàn cho từng người dùng.    

<div style="text-align: center;">
  <img src="/images/2-Proposal/Architect.jpg" style="width: 100%;" />
</div>

#### *Dịch vụ AWS sử dụng*  
- *AWS Lambda*: Xử lý logic nghiệp vụ cho các thao tác CRUD sự kiện và kích hoạt các tác vụ nhắc nhở theo lịch.  
- *Amazon API Gateway*: Cung cấp giao diện RESTful API an toàn để giao tiếp với ứng dụng web.  
- *Amazon DynamoDB*: Lưu trữ dữ liệu sự kiện, lịch trình và thông tin người dùng.  
- *Amazon S3 và CloudFront*: Lưu trữ và phân phối nội dung tĩnh của ứng dụng Frontend. 
- *Amazon EventBride*: Lên lịch và kích hoạt các sự kiện nhắc nhở tự động theo thời gian đã định của người dùng.  
- *Resend*: Gửi thông báo nhắc nhở tùy chỉnh qua email.
- *AWS Amplify*: Lưu trữ và cung cấp giao diện web trực quan.
- *Amazon Cognito*: Quản lý quyền truy cập và xác thực an toàn cho người dùng cá nhân. 

#### *Thiết kế thành phần*  
- *Giao diện người dùng*: AWS Amplify lưu trữ ứng dụng web (dự kiến sử dụng React) cung cấp giao diện lập lịch trực quan, xem lịch và đặt nhắc nhở.  
- *Xác thực Người dùng*: Amazon Cognito quản lý các tài khoản người dùng cá nhân, cấp token ủy quyền an toàn cho việc truy cập API Backend.  
- *Tiếp nhận Yêu cầu API*: Amazon API Gateway nhận và định tuyến các yêu cầu đã được xác thực (ví dụ: tạo sự kiện, truy vấn lịch) tới các hàm Lambda tương ứng. 
- *Xử lý Logic Backend*: AWS Lambda xử lý và thực thi logic nghiệp vụ. Đồng thời, các hàm Lambda khác được kích hoạt bởi EventBridge để gửi nhắc nhở. 
- *Lưu trữ dữ liệu*: Amazon DynamoDB lưu trữ dữ liệu chính (Lịch, Sự kiện, Thông tin người dùng) dưới dạng NoSQL, đảm bảo truy cập nhanh và linh hoạt.  
- *Lên lịch Nhắc nhở*: Amazon EventBridge lập lịch và kích hoạt các sự kiện theo thời gian đã định của người dùng, đảm bảo tính năng nhắc nhở hoạt động tự động.  

### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  
Dự án gồm 2 phần — thiết lập Backend Serverless và xây dựng Giao diện người dùng (Frontend) — mỗi phần trải qua 4 giai đoạn:  
1. *Nghiên cứu và vẽ kiến trúc*: Nghiên cứu sâu về DynamoDB Data Modeling cho lịch trình và thiết kế kiến trúc AWS Serverless đã xác nhận (API Gateway, Lambda, EventBridge). (Thời gian: Tháng thứ 1) 
2. *Tính toán chi phí và kiểm tra tính khả thi*: Sử dụng AWS Pricing Calculator để ước tính chi phí vận hành Serverless thực tế và kiểm tra tính khả thi của luồng xác thực (Cognito) và lưu trữ (DynamoDB). (Thời gian: Tháng thứ 1)  
3. *Điều chỉnh kiến trúc để tối ưu chi phí/giải pháp*: Tinh chỉnh các tham số Lambda (ví dụ: bộ nhớ, thời gian chờ) và DynamoDB (ví dụ: RCU/WCU) để đảm bảo hiệu quả chi phí cao nhất và hiệu năng tốt nhất cho người dùng cá nhân. (Thời gian: Tháng thứ 2)  
4. *Phát triển, kiểm thử, triển khai*: Lập trình các hàm Lambda, thiết lập CI/CD Pipeline với CodePipeline/CodeBuild/CloudFormation, và phát triển ứng dụng Frontend (React). Sau đó kiểm thử Beta và đưa vào vận hành. (Thời gian: Tháng thứ 2-3)  

*Yêu cầu kỹ thuật*  

*1. Yêu cầu Backend (Serverless)*
- *Dịch vụ cốt lõi*: Kiến thức chuyên sâu về AWS Lambda (Node.js), Amazon DynamoDB, Amazon API Gateway, và Amazon Cognito. 
- *Quản lý Sự kiện*: Nắm vững Amazon EventBridge để lên lịch và kích hoạt các hàm Lambda gửi nhắc nhở qua Resend.

*2. Yêu cần Frontend*
- *Giao diện*: Kiến thức thực tế về AWS Amplify để lưu trữ ứng React và kết nối với API Gateway
- *Tối ưu hoá*: Tận dụng khả năng xử lý của React để giảm tải cho các hàm Lambda  

### 5. Lộ trình & Mốc triển khai  
- *Thực tập (Tháng 1–3)*:  
    - Tháng 1: Học AWS và nâng cấp phần cứng.  
    - Tháng 2: Thiết kế và điều chỉnh kiến trúc.  
    - Tháng 3: Triển khai, kiểm thử, đưa vào sử dụng.  
- *Sau triển khai*: Nghiên cứu thêm trong vòng 1 năm.  

### 6. Ước tính ngân sách   

*Chi phí hạ tầng*  
- AWS Amplify: 0.35 USD/tháng 
- S3 Standard: 0.05 USD/tháng  
- CloudFront: 1.70 USD/tháng   
- API Gateway: 0.00(miễn phí) USD/tháng
- AWS Lambda: 0.00(miễn phí) USD/tháng   
- DynamoDB: 0.11 USD/tháng  
- Amazon Cognito: 0.00 USD/tháng   
- Resend(email): 0.00(miễn phí) USD/tháng
- EventBridge: 0.10 USD/tháng  
- CloudWatch Logs: 0.10 USD/tháng
- CI/CD Pipeline+Build: 0.00(miễn phí) USD/tháng

*Tổng*: 2,41 USD/tháng, 28,92 USD/12 tháng    

### 7. Đánh giá rủi ro  
*Ma trận rủi ro*  
- Mất kết nối mạng/Độ trễ cao: Ảnh hưởng trung bình, xác suất trung bình.  
- Lỗi thiết kế DynamoDB: Ảnh hưởng cao, xác suất trung bình.  
- Chi phí Serverless vượt ngân sách: Ảnh hưởng trung bình, xác suất thấp.
- Lỗi hệ thống nhắc nhở: Ảnh hưởng cao, xác suất thấp.

*Chiến lược giảm thiểu*  
- Mất kết nối mạng/Độ trễ cao: Tối ưu hóa Frontend (được lưu trữ trên S3/CloudFront) để đảm bảo tốc độ tải nhanh, sử dụng Client-side Caching để người dùng vẫn có thể xem được lịch trình gần nhất khi mất mạng.  
- Lỗi thiết kế DynamoDB: Thực hiện Proof of Concept (POC) và Load Testing nghiêm ngặt cho mô hình dữ liệu. Sử dụng Global Secondary Index (GSI) được tối ưu hóa để tránh truy vấn toàn bộ bảng và tối thiểu hóa chi phí RCU/WCU.  
- Chi phí Serverless vượt ngân sách: Thiết lập AWS Budgets với các cảnh báo chủ động (Email/SNS) khi chi tiêu gần đạt ngưỡng. Thường xuyên kiểm tra AWS Cost Explorer và tối ưu hóa tài nguyên Lambda.
- Lỗi hệ thống nhắc nhở: Giám sát chặt chẽ EventBridge và Lambda xử lý nhắc nhở thông qua CloudWatch Alarms để phát hiện lỗi ngay lập tức. Cài đặt cơ chế Retry Logic cho các hàm Lambda quan trọng.

*Kế hoạch dự phòng*  
- Sự cố AWS: Do ứng dụng chỉ dành cho cá nhân, nếu AWS gặp sự cố, hệ thống sẽ được cấu hình để tự động Rollback về phiên bản code gần nhất (thông qua CodePipeline) hoặc triển khai lại cấu hình (thông qua CloudFormation).  
- Mất Dữ liệu: Thiết lập DynamoDB Backup and Restore định kỳ để khôi phục nhanh chóng dữ liệu sự kiện trong trường hợp lỗi hệ thống hoặc lỗi người dùng.
- Vấn đề Chi phí: Sử dụng CloudFormation để khôi phục cấu hình tài nguyên (như RCU/WCU) về trạng thái chi phí thấp đã được chứng minh.

### 8. Kết quả kỳ vọng  
*Cải tiến Trải nghiệm Người dùng*: Cung cấp giao diện lập lịch trực quan và nhắc nhở thời gian thực (Real-time notifications) thay thế cho quy trình ghi chép và theo dõi lịch trình thủ công. Hệ thống được thiết kế để dễ dàng mở rộng, phục vụ từ một người dùng lên đến hàng triệu người dùng cá nhân.  

*Giá trị Dài hạn và Khả năng Tái sử dụng*: Tạo ra một nền tảng kỹ thuật Serverless vững chắc, có thể duy trì hoạt động với chi phí cực thấp trong nhiều năm. Kiến trúc này có thể được tái sử dụng cho các dự án ứng dụng cá nhân hoặc các tính năng mở rộng khác trong tương lai (ví dụ: theo dõi thói quen, quản lý công việc cá nhân đơn giản).

*Hoàn thành Triển khai*: Triển khai thành công toàn bộ kiến trúc Serverless đã được phê duyệt, bao gồm CI/CD tự động và các tính năng cốt lõi của Aurora Time trong khung thời gian dự kiến.