---
title: "Worklog Tuần 2"
date: "2025-09-09T19:53:52+07:00"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu về Budget: <br>&emsp; + Cost Budget <br>&emsp; + Usage Budget <br>&emsp; + Reservation Instance (RI) Budget <br>&emsp; + Savings Plans Budget <br> - Tìm hiểu về IAM (Identity and Access Management) <br>&emsp; + Authentication(Xác thực) & Authorization(Uỷ quyền) <br>&emsp; + IAM User <br>&emsp; + IAM Groups <br>&emsp; + IAM Policies <br>&emsp; + IAM Roles <br>**Thực hành:** <br>&emsp; - Khởi tạo Budget <br>&emsp; - Tạo IAM Groups, User, Role & Đăng nhập bằng IAM User <br>&emsp; - Chuyển đổi giữa các IAM Role| 15/09/2025 | 15/09/2025 |<https://000007.awsstudygroup.com/vi/> <br><br> <https://000002.awsstudygroup.com/vi/> |
| 3   | - Xem lại + Ôn tập lý thuyết của EC2, subnet, VPC, Internet Gateway, Nat Gateway, route table, Security Group, Region | 16/09/2025 | 16/09/2025 | <https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i> |
| 4   | - Tìm hiểu về S3: <br>&emsp; S3 Bucket: <br>&emsp; + Là 1 container chứa object <br>&emsp; + Đặt tên duy nhất trên toàn cầu <br>&emsp; S3 Object: <br>&emsp; + Là tệp/dữ liệu được lưu trữ bên trong Bucket <br>&emsp; + Kích thước từ 0 byte đến 5TB cho 1 object <br>&emsp; + Không giới hạn object trên 1 bucket <br>&emsp; S3 Versioning: <br>&emsp; + Bảo vệ dữ liệu (lưu chỉ các phiên bản object trong bucket) <br>&emsp; + Khôi phục dữ liệu của những phiên bản trước đó <br>&emsp; S3 Cross-Region Replication (CRR) <br>&emsp; + Sao chép các Bucket trong cái AWS khác nhau <br> **Thực hành:** <br>&emsp; Tạo S3 Bucket và tải dữ liệu mã nguồn <br>&emsp; + Chạy website & kết hợp với CloudFront <br>&emsp; + Di chuyển Object qua các Bucket <br>&emsp; + Sao chép S3 Object sang 1 region khác | 17/09/2025 | 17/09/2025 | <https://000057.awsstudygroup.com/vi/1-introduce/> |
| 5   | - Tìm hiểu về Amazon Relational Database Service (RDS) <br>&emsp; + Triển khai và quản lí cơ sở dữ liệu có quan hệ trên AWS <br> - Amazon RDS hỗ trợ các cơ sở dữ liệu: <br>&emsp; + Amazon Aurora <br>&emsp; + MySQL <br>&emsp; + MariaDB <br>&emsp; + Oracle <br>&emsp; + SQL Server <br>&emsp; + PostgreSQL <br>**Thực hành:** <br>&emsp; + Tạo Security Group cho DB instance, Tạo DB Subnet group <br>&emsp; Cài Git & Node.js vào EC2 <br>&emsp; + Tạo DB instance <br>&emsp; + Triển khai ứng dụng | 18/09/2025 | 18/09/2025 | <https://000005.awsstudygroup.com/> |
| 6   | - Tìm hiểu về Auto Scaling của EC2 | 19/09/2025 | 19/09/2025 | <https://000006.awsstudygroup.com/vi/> |

### Kết quả đạt được tuần 2:

#### Quản lý chi phí (AWS Budgets)
- Tìm hiểu các loại ngân sách: Cost, Usage, Reservation Instance (RI), Savings Plans.
- Thực hành khởi tạo Budget để giám sát chi phí.
#### Quản lý danh tính và truy cập (IAM)
- Nắm vững khái niệm Authentication (Xác thực) & Authorization (Uỷ quyền).
- Hiểu rõ các thành phần: IAM User, Groups, Policies, Roles.
- Thực hành:
    - Tạo IAM Groups, User, Role.
    - Đăng nhập bằng IAM User và chuyển đổi giữa các IAM Role.

#### Ôn tập hạ tầng mạng
- Hệ thống lại kiến thức về: EC2, VPC, Subnet, Internet Gateway, NAT Gateway, Route Table, Security Group.

#### Dịch vụ lưu trữ (S3)
- Nắm kiến thức về S3 Bucket, Object.
- Hiểu về tính năng Versioning (quản lý phiên bản) và Cross-Region Replication (CRR).
- Thực hành:
    - Tạo Bucket, upload dữ liệu.
    - Triển khai Static Website kết hợp với CloudFront.
    - Di chuyển Object và sao chép dữ liệu sang Region khác.
#### Cơ sở dữ liệu (RDS)
- Tìm hiểu Amazon RDS và các CSDL hỗ trợ (Aurora, MySQL, PostgreSQL,...).
- Thực hành:
    - Cấu hình Security Group và DB Subnet group.
    - Cài đặt Git & Node.js trên EC2.
    - Khởi tạo DB Instance và triển khai ứng dụng kết nối cơ sở dữ liệu.
#### Khả năng mở rộng
- Tìm hiểu cơ chế Auto Scaling của EC2.


