---
title : "Phần 2"
date :  "2025-09-09T19:53:52+07:00"
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

# Upload Website lên Amazon S3

## Mục tiêu
Trong phần này, bạn sẽ:
- Tạo S3 bucket để lưu trữ website
- Upload các file HTML lên S3
- Cấu hình S3 để host website tĩnh
- Website của bạn sẽ có URL công khai và có thể truy cập từ bất kỳ đâu!

---

## Kiến thức nền

### S3 là gì?
**Amazon S3 (Simple Storage Service)** là dịch vụ lưu trữ đối tượng (object storage) của AWS. Bạn có thể nghĩ S3 như một "ổ cứng đám mây" khổng lồ để lưu file.

### S3 Static Website Hosting là gì?
S3 có tính năng đặc biệt cho phép bạn host website tĩnh (HTML, CSS, JS, hình ảnh) mà không cần web server. Điểm mạnh:
-  Rẻ (~$0.023/GB/tháng)
-  Không cần quản lý server
-  Tự động scale khi có nhiều người truy cập
-  Độ bền 99.999999999% (11 số 9)

---

## Bước 1: Truy cập AWS Console

### 1.1. Đăng nhập AWS

1. Mở trình duyệt, truy cập: https://console.aws.amazon.com
2. Đăng nhập bằng:
   - **Root user email** + password, HOẶC
   - **IAM user** (nếu công ty cấp)
3. Chọn region **Asia Pacific (Singapore) ap-southeast-1** ở góc trên bên phải

> **Tại sao chọn Singapore?** Vì gần Việt Nam nhất, độ trễ thấp, giá cả hợp lý.

### 1.2. Tìm dịch vụ S3

- Gõ "S3" vào ô tìm kiếm trên thanh menu

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.3-Workshop-Module2/S3.png" alt="S3" style="width:100%" />
</div>

---

## Bước 2: Tạo S3 Bucket

### 2.1. Bắt đầu tạo bucket

1. Trong **S3 Console**, click nút **"Create bucket"** (màu cam)
<div style="text-align: center;">
  <img src="/images/5-Workshop/5.3-Workshop-Module2/Create_S3.png" alt="Create_S3" style="width:100%" />
</div>
2. Bạn sẽ thấy form tạo bucket với nhiều options

### 2.2. Cấu hình General

**Bucket name:**
```
portfolio-yourname-2025
```

> **LƯU Ý QUAN TRỌNG về tên bucket:**
> - Tên phải **unique toàn cầu** (không ai trên thế giới dùng tên này)
> - Chỉ dùng chữ thường, số, dấu gạch ngang (-)
> - Không dấu tiếng Việt, không khoảng trắng
> - Dài từ 3-63 ký tự
> 
> **Ví dụ tên hợp lệ:**
> - `portfolio-anhnguyen-2024`
> - `my-awesome-website-123`
> - `test-bucket-hcm`

**AWS Region:**
```
Asia Pacific (Singapore) ap-southeast-1
```

### 2.3. Cấu hình Object Ownership
```
 ACLs disabled (recommended)
```

> **Giải thích:** ACL (Access Control List) là cách quản lý quyền truy cập cũ. AWS khuyên dùng Bucket Policy thay thế vì dễ quản lý hơn.

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.3-Workshop-Module2/Config1.png" alt="Config_S3" style="width:100%" />
</div>

### 2.4. Cấu hình Block Public Access 

**QUAN TRỌNG - ĐỌC KỸ:**

Mặc định AWS sẽ chặn mọi truy cập công khai để bảo mật. Nhưng vì đây là website công khai, ta cần bỏ chặn:

1. **BỎ CHỌN** (uncheck) tất cả 4 ô sau:
   -  Block all public access
   -  Block public access to buckets and objects granted through new access control lists (ACLs)
   -  Block public access to buckets and objects granted through any access control lists (ACLs)
   -  Block public access to buckets and objects granted through new public bucket or access point policies
   -  Block public and cross-account access to buckets and objects through any public bucket or access point policies

2. Sau khi bỏ chọn, sẽ xuất hiện cảnh báo màu vàng
> **Cảnh báo bảo mật:** Chỉ làm điều này cho bucket chứa website công khai. Không làm với bucket chứa dữ liệu nhạy cảm!

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.3-Workshop-Module2/Config2.png" alt="Config_S3" style="width:100%" />
</div>

### 2.5. Hoàn tất

Click nút **"Create bucket"** ở cuối trang.

---

## Bước 3: Upload File lên S3

### 3.1. Vào bucket vừa tạo

1. Trong danh sách buckets, **click vào tên bucket** bạn vừa tạo
2. Bạn sẽ vào màn hình Objects (danh sách file trong bucket)

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.3-Workshop-Module2/Config3.png" alt="Config_S3" style="width:100%" />
</div>

### 3.2. Upload files

1. Click nút **"Upload"** (màu cam)
2. Click **"Add files"**
3. Chọn **2 file**: `index.html` và `error.html` từ thư mục `my-portfolio`
4. Click **"Open"**

Bạn sẽ thấy 2 file xuất hiện trong danh sách "Files and folders":
```
📄 index.html
📄 error.html
```

### 3.3. Cấu hình upload (giữ mặc định)

1. Scroll xuống cuối → Click **"Upload"**
2. Đợi progress bar chạy đến 100%
3. Thấy thông báo "Upload succeeded" với 2 file
4. Click **"Close"** để quay lại bucket

### 3.5. Xác nhận upload thành công

Bạn sẽ thấy 2 file trong bucket:
```
Name                Type        Size        Last modified
index.html          text/html   ~8 KB       Just now
error.html          text/html   ~1 KB       Just now
```

 **Checkpoint:** Files đã được upload lên S3!

---

## Bước 4: Enable Static Website Hosting

### 4.1. Mở tab Properties

1. Đang ở trong bucket, click vào tab **"Properties"** (ở trên cùng)
2. Scroll xuống cuối cùng

### 4.2. Tìm Static website hosting

Scroll xuống sẽ thấy section **"Static website hosting"** (gần cuối cùng)

Hiện tại status là: `Disabled`

### 4.3. Enable và cấu hình

1. Click nút **"Edit"** trong section "Static website hosting"
2. Cấu hình như sau:

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.3-Workshop-Module2/Config4.png" alt="Config_S3" style="width:100%" />
</div>

3. Click **"Save changes"**

### 4.4. Lưu lại Website Endpoint URL

Sau khi save, scroll xuống lại section "Static website hosting", bạn sẽ thấy:

**Bucket website endpoint:**
```
http://portfolio-yourname-2025.s3-website-ap-southeast-1.amazonaws.com
```

> 📝 **QUAN TRỌNG:** Copy URL này và lưu lại (dán vào Notepad/Notes)
> 
> **Cấu trúc URL:** `http://[bucket-name].s3-website-[region].amazonaws.com`

✅ **Checkpoint:** Website hosting đã được bật!

---

## Bước 5: Cấu hình Bucket Policy (Cho phép truy cập công khai)

### 5.1. Hiện tại website chưa truy cập được

Nếu bạn thử mở URL từ bước 4.4, sẽ gặp lỗi:
```
403 Forbidden
Access Denied
```

**Tại sao?** Vì chúng ta chưa cho phép mọi người truy cập file trong bucket.

### 5.2. Mở tab Permissions

1. Click vào tab **"Permissions"** (ở trên cùng)
2. Scroll xuống tìm **"Bucket policy"**

### 5.3. Thêm Bucket Policy

1. Click nút **"Edit"** trong section "Bucket policy"
2. Bạn sẽ thấy một text editor trống
3. **Copy đoạn JSON sau và paste vào:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

4. **QUAN TRỌNG:** Thay `YOUR-BUCKET-NAME` bằng tên bucket của bạn

**Ví dụ:** Nếu bucket của bạn là `portfolio-anhnguyen-2025`, thì dòng Resource sẽ là:
```json
"Resource": "arn:aws:s3:::portfolio-anhnguyen-2025/*"
```

### 5.4. Hiểu về Bucket Policy

**Giải thích từng phần:**
```json
"Effect": "Allow"           → Cho phép truy cập
"Principal": "*"            → Tất cả mọi người (*)
"Action": "s3:GetObject"    → Được phép đọc/download file
"Resource": "arn:aws:s3:::portfolio-anhnguyen-2025/*"  
    → Áp dụng cho tất cả file trong bucket (dấu /*)
```

> **Bảo mật:** Policy này CHỈ cho phép đọc file, không cho phép xóa, sửa, hoặc upload.

### 5.5. Lưu policy

Click **"Save changes"**
**Checkpoint:** Bucket policy đã được cấu hình!

---

## Bước 6: Test Website

### 6.1. Truy cập website

1. Mở trình duyệt mới (hoặc tab mới)
2. Paste URL từ bước 4.4:
```
   http://portfolio-yourname-2025.s3-website-ap-southeast-1.amazonaws.com
```
3. Nhấn Enter

### 6.2. Kết quả mong đợi

Bạn sẽ thấy website portfolio với:
- Header màu gradient tím-xanh
- Tên của bạn
- Các section: Về tôi, Kỹ năng, Dự án, Liên hệ
- Footer ở cuối

### 6.3. Test trang lỗi 404

Thử truy cập URL không tồn tại:
```
http://portfolio-yourname-2025.s3-website-ap-southeast-1.amazonaws.com/test123
```

Bạn sẽ thấy trang 404 đẹp với:
- Số "404" lớn
- Thông báo "Oops! Không tìm thấy trang..."
- Link quay về trang chủ

### 6.4. Share với bạn bè!

Website của bạn giờ đã online và có thể truy cập từ bất kỳ đâu trên thế giới!

Copy URL và gửi cho bạn bè test thử.

---

## Chúc mừng - Phần 2 hoàn thành!

### Những gì bạn đã làm được:

- Tạo S3 bucket với tên unique  
- Upload 2 file HTML lên S3  
- Enable Static Website Hosting  
- Cấu hình Bucket Policy cho public access  
- Website đã online với URL công khai  
- Test thành công cả trang chủ và trang 404