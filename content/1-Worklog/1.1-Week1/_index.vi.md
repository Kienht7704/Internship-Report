---
title: "Worklog Tuần 1"
date: "2025-09-09T19:53:52+07:00"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 1:

* Kết nối, làm quen với các thành viên trong First Cloud Journey.
* Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Lập nhóm và kết nối với các thành viên trong nhóm <br> - Dành thời gian đọc rõ và ghi nhớ các nội quy trước khi lên văn phòng thực tập <br> - Xem qua videos giới thiệu về AWS trên youtube: <br>&emsp; + AWS, điện toán đám mây là gì? <br>&emsp; + Hạ tầng toàn cầu của AWS <br>&emsp;  + Tối ưu hoá chi phí trên AWS <br>&emsp; +... | 08/09/2025 | 08/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 3   | - Tạo tài khoản AWS Free Tier account & thiết lập MFA theo hướng dẫn của tài liệu được cung cấp <br> - Tìm hiểu cách sử dụng AWS Console: <br>&emsp; + Tạo Admin group và Admin user <br>&emsp; + Thiết lập bảo mật cho Admin user <br>&emsp; + Cấu hình region và khởi tạo dashboard widgets <br>&emsp; + Tìm hiểu và hiểu rõ các trường hợp hỗ trợ của AWS | 09/09/2025 | 09/09/2025 | <https://000001.awsstudygroup.com/> <br><br> <https://www.youtube.com/@AWSStudyGroup> |
| 4   | - Tìm hiểu về EC2: <br>&emsp; + EC2 là gì? <br>&emsp; + AMI <br>&emsp; + EBS Volume <br>&emsp; + Snapshot <br>&emsp; + instance <br>&emsp; + Security group (Giới hạn truy cập)  | 10/09/2025 | 10/09/2025 | <https://www.youtube.com/watch?v=6PqZVGoeEEA&list=PLgT9f4ZU9cncuJ5ACqQxook4yKO1xuZP6&index=3> |
| 5   | - Xem lại lý thuyết về Subnet, Route table, Internet Gateway, Nat Gateway <br> - **Thực hành:** <br>&emsp; + Tạo VPC, Subnet, Internet Gateway, Route table và Security group <br>&emsp; + Khởi tạo EC2 với instance type: t3.micro <br>&emsp; + Kết nối vào máy chủ EC2 Private <br>&emsp; + Khởi tạo Nat Gateway | 11/09/2025 | 11/09/2025 | <https://000003.awsstudygroup.com/vi/4-createec2server/> <br><br> <https://www.youtube.com/watch?v=6PqZVGoeEEA&list=PLgT9f4ZU9cncuJ5ACqQxook4yKO1xuZP6&index=4> |
| 6   | - Tìm hiểu về Site to Site VPN <br> - **Thực hành:** <br>&emsp; + Tạo VPC cho VPN <br>&emsp + Tạo VPG và Customer Gateway <br>&emsp; + Khởi tạo kết nối VPN <br>&emsp; + Cấu hình và Tuỳ chỉnh Customer Gateway & AWS VPN Tunnel | 12/09/2025 | 12/09/2025 | <https://000003.awsstudygroup.com/vi/5-vpnsitetosite/> |

## Kết quả đạt được – Tuần 1

#### Hoàn thành công việc chung
- Lập nhóm, kết nối với các thành viên.  
- Nắm rõ và ghi nhớ nội quy trước khi thực tập.  

#### Kiến thức nền tảng AWS
- Nắm được kiến thức cơ bản qua video giới thiệu:  
  - Khái niệm **Cloud & AWS**  
  - **Hạ tầng toàn cầu** của AWS  
  - **Tối ưu chi phí** trên AWS  

#### Quản trị tài khoản AWS
- Tạo thành công **AWS Free Tier account** và thiết lập **MFA**.  
- Sử dụng **AWS Console** để:  
  - Tạo **Admin group** và **Admin user**  
  - Thiết lập bảo mật cho Admin user  
  - Cấu hình **region** và **dashboard widgets**  
  - Tìm hiểu các **AWS Support Plans**  

#### EC2
- Hiểu khái niệm: **EC2, AMI, EBS, Snapshot, Instance, Security Group**.  

#### Hạ tầng mạng cơ bản
- Thực hành:  
  - Tạo **VPC, Subnet, Internet Gateway, Route Table, Security Group**  
  - Khởi tạo **EC2 (t3.micro)**  
  - Kết nối **EC2 Private**  
  - Tạo **NAT Gateway**  

#### VPN
- Tìm hiểu và thực hành cấu hình **Site-to-Site VPN**:  
  - Tạo **VPC cho VPN**  
  - Tạo **VPG** và **Customer Gateway**  
  - Thiết lập **VPN Tunnel**  
  - Tùy chỉnh kết nối giữa **Customer Gateway** và **AWS VPN Tunnel**  






