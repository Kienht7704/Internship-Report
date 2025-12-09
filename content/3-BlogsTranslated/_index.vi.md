---
title: "Các bài blogs đã dịch"
date: "2025-09-09T19:53:52+07:00"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

###  [Blog 1 - AWS Powers Breakthrough Gaming Experiences at devcom and gamescom 2025](3.1-Blog1/)
Bài viết tường thuật vai trò và hoạt động của AWS tại các sự kiện devcom và gamescom 2025, giới thiệu chương trình, buổi hội thảo và các phiên demo dành cho nhà phát triển game. Nội dung nhấn mạnh các giải pháp của AWS cho ngành game (GameLift, GameLift Streams), ứng dụng AI cho QA và localization, hướng dẫn xây dựng pipeline phân tích trò chơi, cùng các đối tác và workshop nổi bật nhằm giúp studio tối ưu hoá phát triển và vận hành trò chơi.

###  [Blog 2 - Flexibility to Framework: Building MCP Servers with Controlled Tool Orchestration](3.2-Blog2/)
Bài này giải thích khái niệm MCP (Model Context Protocol) và thách thức khi cần buộc thứ tự thực thi công cụ (tool ordering) trong các agent AI. Tác giả mô tả giải pháp token-based orchestration: sử dụng token ngắn hạn do server cấp, xác thực ở server để đảm bảo các bước (generate → scan → create) được thực hiện tuần tự và an toàn. Bài còn trình bày chi tiết về thiết kế `@mcp.tool()` và các giới hạn/biện pháp giảm thiểu khi triển khai trong môi trường thực tế.

###  [Blog 3 - Introducing AWS Cloud Control API MCP Server](3.3-Blog3/)
Bài giới thiệu CCAPI MCP Server — một server MCP tích hợp AWS Cloud Control API giúp chuyển đổi ý định tự nhiên thành các thao tác CRUDL trên hơn 1.200 loại tài nguyên AWS. Nội dung nêu các tính năng chính (xuất IaC, ước tính chi phí qua Pricing API, quét bảo mật bằng Checkov), hướng dẫn cấu hình, chế độ read-only, và ví dụ thực hành (tạo S3 + KMS). Bài nhấn mạnh lợi ích về năng suất và khả năng tích hợp với công cụ phát triển như Amazon Q Developer.

