---
title: "AWS Cloud Mastery Series #2"
date: 2025-11-17
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

### AWS Cloud Mastery Series #2

**- Ngày:** 17 tháng 11, 2025
**- Địa điểm:** Văn phòng AWS Việt Nam, Bitexco Financial Tower, TP.HCM


---

#### Tổng quan sự kiện

Hội thảo chuyên sâu cả ngày về văn hóa và công cụ DevOps trên nền tảng AWS, bao trùm toàn bộ vòng đời phát triển phần mềm từ tư duy quản trị đến vận hành hạ tầng và giám sát hệ thống.

**Mục tiêu chính:**
- **Định hình Tư duy DevOps:** Hiểu rõ văn hóa DevOps và các chỉ số đo lường hiệu quả then chốt (DORA metrics, MTTR).

- **Tự động hóa Quy trình (CI/CD):** Thiết lập đường ống tích hợp và triển khai liên tục sử dụng bộ công cụ AWS Code Series.

- **Hiện đại hóa với Container:** Nắm vững chiến lược triển khai Microservices trên Amazon ECS, EKS và App Runner.

- **Giám sát toàn diện:** Thiết lập khả năng quan sát (Observability) full-stack để phát hiện và xử lý sự cố tức thời.

---

#### Bài học và Erkenntnisse chính

- **Tự động hóa là xương sống:** Một quy trình CI/CD mạnh mẽ không chỉ là build code mà còn phải bao gồm các chiến lược triển khai an toàn như Blue/Green hoặc Canary để giảm thiểu rủi ro downtime.

- **IaC ngăn chặn "Drift":** Việc sử dụng CloudFormation hoặc CDK giúp đảm bảo môi trường nhất quán, tránh hiện tượng "cấu hình trôi dạt" (configuration drift) thường gặp khi thao tác thủ công.

- **Lựa chọn công cụ Container phù hợp:** Không có giải pháp duy nhất; ECS phù hợp cho sự tích hợp sâu với AWS, trong khi EKS dành cho chuẩn Kubernetes mở rộng, và App Runner dành cho sự đơn giản hóa tối đa.

- **Observability > Monitoring:** Giám sát (Monitoring) chỉ cho biết hệ thống đang hỏng, trong khi Khả năng quan sát (Observability - với AWS X-Ray) giúp trả lời câu hỏi tại sao và ở đâu hệ thống bị lỗi trong kiến trúc phân tán.

---

#### Ứng dụng vào Công việc

- **Đo lường hiệu suất team:** Áp dụng ngay bộ chỉ số **DORA** (Deployment Frequency, Lead Time for Changes...) để đánh giá hiện trạng quy trình phát triển của team.

- **Nâng cấp Pipeline:** Rà soát lại pipeline hiện tại, tích hợp thêm AWS CodeDeploy để thực hiện chiến lược **Blue/Green Deployment** cho các dịch vụ quan trọng.

- **Chuyển đổi sang CDK:** Bắt đầu thử nghiệm chuyển đổi một module hạ tầng nhỏ từ CloudFormation thuần (YAML/JSON) sang AWS CDK để tận dụng khả năng tái sử dụng code (Constructs).

- *Tối ưu giám sát:** Tích hợp **AWS X-Ray** vào các ứng dụng Microservices hiện có để có được bản đồ truy vết (trace map) chi tiết, giúp giảm thời gian điều tra lỗi (MTTR).

---
#### Ảnh sự kiện
<div style="text-align: center;">
  <img src="/images/4-Event/Event2.jpeg"  style="width:100%" />
</div><div style="text-align: center;">
  <img src="/images/4-Event/Event2.1jpeg.jpeg"  style="width:100%" />
</div><div style="text-align: center;">
  <img src="/images/4-Event/Event2.2jpeg.jpeg"  style="width:100%" />
</div><div style="text-align: center;">
  <img src="/images/4-Event/Event2.3jpeg.jpeg"  style="width:100%" />
</div><div style="text-align: center;">
  <img src="/images/4-Event/Event2.4jpeg.jpeg"  style="width:100%" />
</div><div style="text-align: center;">
  <img src="/images/4-Event/Event2.5jpeg.jpeg"  style="width:100%" />
</div>