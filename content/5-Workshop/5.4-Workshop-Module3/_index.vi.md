---
title : "Phần 3"
date :  "2025-09-09T19:53:52+07:00"
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

# Thêm CloudFront - HTTPS & CDN Toàn Cầu

## Mục tiêu
Trong phần này, bạn sẽ:
- Tạo CloudFront distribution để phân phối website toàn cầu
- Bật HTTPS miễn phí cho website
- Tăng tốc độ load trang gấp 5-10 lần
- Website chuyên nghiệp hơn với ổ khóa xanh trên thanh địa chỉ

---

## Kiến thức nền

### CloudFront là gì?

**Amazon CloudFront** là dịch vụ CDN (Content Delivery Network - Mạng phân phối nội dung) của AWS.

**Cách hoạt động:**
```
User ở Hà Nội → CloudFront Edge (Hà Nội) → Trả về nội dung ngay lập tức
User ở Tokyo    → CloudFront Edge (Tokyo)    → Trả về nội dung ngay lập tức
User ở London   → CloudFront Edge (London)   → Trả về nội dung ngay lập tức
```

Thay vì tất cả user phải truy cập S3 ở Singapore, họ truy cập server CloudFront gần nhất.

### Tại sao cần CloudFront?

#### 1. HTTPS miễn phí (Bảo mật)
- S3 static website chỉ hỗ trợ HTTP
- CloudFront cung cấp HTTPS miễn phí
- Ổ khóa xanh trên trình duyệt → Tăng độ tin cậy
- Google ưu tiên website HTTPS trong kết quả tìm kiếm

#### 2. Tăng tốc độ load
**Không có CloudFront:**
- User ở Việt Nam truy cập S3 Singapore: ~50-100ms
- User ở Châu Âu truy cập S3 Singapore: ~300-500ms
- User ở Mỹ truy cập S3 Singapore: ~200-400ms

**Có CloudFront:**
- Tất cả user: ~20-50ms (vì truy cập edge location gần nhất)
- **Cải thiện 5-10 lần!**

#### 4. Professional hơn
- URL ngắn hơn: `d123abc.cloudfront.net` vs `bucket.s3-website-region.amazonaws.com`
- Có thể dùng custom domain
- Hỗ trợ nhiều tính năng nâng cao (geo-blocking, custom headers, lambda@edge)

---

## Bước 1: Truy cập CloudFront Console

### 1.1. Mở CloudFront

Từ AWS Console:
- Gõ "CloudFront" vào ô tìm kiếm:
<div style="text-align: center;">
  <img src="/images/5-Workshop/5.4-Workshop-Module3/CloudFront.png" alt="CloudFront" style="width:100%" />
</div>


### 1.2. Màn hình Welcome

Nếu đây là lần đầu dùng CloudFront, bạn sẽ thấy trang welcome.

Click nút **"Create distribution"** (màu cam)

---

## Bước 2: Tạo và cấu hình Distribuition

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.4-Workshop-Module3/CloudFront.Confgi.png" alt="Config CloudFront" style="width:100%" />
</div>

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.4-Workshop-Module3/CloudFront.Config.png" alt="Config CloudFront" style="width:100%" />
</div>

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.4-Workshop-Module3/CloudFront.Config2.png" alt="Config CloudFront" style="width:100%" />
</div>

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.4-Workshop-Module3/CloudFront.Config3.png" alt="Config CloudFront" style="width:100%" />
</div>

Click ` Create Distribution `

## Bước 3: Test CloudFront

### 3.1. Copy Distribution Domain Name

Trong danh sách distributions hoặc trong chi tiết distribution:
```
Domain name: d1234567890abcd.cloudfront.net
```
> Thay phần "d1234567890abcd" thành của bạn hoặc có thể bấm trực tiếp trên CloudFront Console
<div style="text-align: center;">
  <img src="/images/5-Workshop/5.4-Workshop-Module3/TestCloudFront.png" alt="Test" style="width:100%" />
</div> 

Copy domain này.

### 3.2. Truy cập website qua HTTPS

Mở trình duyệt, nhập: domain bạn đã copy.

### 3.3. Kết quả mong đợi

**Website hiển thị bình thường:**
- Header gradient tím-xanh
- Tất cả nội dung hiển thị đúng
- Layout không bị vỡ

**Có ổ khóa xanh trong thanh địa chỉ:**

### 3.4. Test redirect HTTP → HTTPS

Thử truy cập bằng HTTP (không có 's'):
```
Ví dụ: http://d1234567890abcd.cloudfront.net
```

Trình duyệt sẽ **tự động chuyển** sang:
```
=> https://d1234567890abcd.cloudfront.net
```

### 3.5. Test trang 404

Truy cập URL không tồn tại:
```
Ví Dụ: https://d1234567890abcd.cloudfront.net/test-404
```

Trang error.html sẽ hiển thị (số 404 lớn với background gradient)

---

## Hiểu thêm về CloudFront

### Cache hoạt động như thế nào?

**Lần truy cập đầu tiên:**
```
User → CloudFront Edge → (MISS) → S3 → CloudFront → User
        └─ Lưu vào cache
```

**Lần truy cập tiếp theo:**
```
User → CloudFront Edge → (HIT) → User
        (Trả về từ cache ngay lập tức)
```

### Cache bao lâu?

Default: **24 giờ** (theo CachingOptimized policy)

Sau 24h, CloudFront sẽ kiểm tra S3 xem file có thay đổi không.

### Invalidation là gì?

Khi bạn update file trên S3, CloudFront vẫn serve bản cũ từ cache.

**Cách force CloudFront update ngay:**
1. Vào CloudFront Console → Chọn distribution
2. Tab "Invalidations" → Click "Create invalidation"
3. Object paths: `/*` (invalidate tất cả)
4. Click "Create invalidation"
5. Đợi 1-2 phút → Cache mới!

> **Lưu ý:** Free Tier có 1,000 invalidation paths miễn phí mỗi tháng.