---
title : "Giới thiệu"
date :  "2025-09-09T19:53:52+07:00" 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

## Tổng quan về Workshop

Trong workshop này, bạn sẽ xây dựng một **website portfolio cá nhân** hoàn chỉnh với kiến trúc **serverless** trên AWS, không cần quản lý server hay lo lắng về scaling.

### Kiến trúc Workshop

Workshop sử dụng **2 dịch vụ AWS chính**:

+ **Amazon S3 (Simple Storage Service)** - Đóng vai trò là web server lưu trữ các file tĩnh (HTML, CSS, images). S3 Static Website Hosting cho phép bạn host website với chi phí cực thấp và khả năng lưu trữ không giới hạn.

+ **Amazon CloudFront** - Dịch vụ CDN (Content Delivery Network - Mạng phân phối nội dung) giúp website của bạn tải nhanh hơn. Khi người dùng truy cập, họ sẽ nhận nội dung từ server gần nhất thay vì phải đi từ Singapore (nơi đặt S3). CloudFront cũng cung cấp **HTTPS miễn phí**.

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.1-Workshop-overview/Architect-Workshop.png" alt="Kiến trúc Workshop" style="width:100%" />
</div>

### Lợi ích của kiến trúc này

**Về hiệu năng:** Website được phục vụ từ edge location gần người dùng nhất giúp giảm độ trễ.

**Về chi phí:** Với Free Tier, bạn có 12 tháng miễn phí với 5GB S3 storage, 1TB CloudFront transfer và 10 triệu requests. Sau Free Tier, chi phí chỉ ~$0.60/tháng cho website traffic trung bình.

**Về vận hành:** Không cần quản lý server, không cần lo về patching, scaling tự động, uptime 99.99% được AWS đảm bảo. Bạn chỉ cần tập trung vào nội dung website.

**Về bảo mật:** HTTPS được bật mặc định, S3 bucket policy kiểm soát access, CloudFront có AWS Shield Standard tích hợp sẵn để chống DDoS attacks cơ bản.
