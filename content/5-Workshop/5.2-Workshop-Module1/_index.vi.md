---
title : "Phần 1"
date :  "2025-09-09T19:53:52+07:00"
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

## Hướng dẫn: Tạo Website Files cho Portfolio

### Mục tiêu
Trong phần này, bạn sẽ tạo 2 file HTML cơ bản cho website portfolio của mình:
- `index.html` - Trang chủ hiển thị thông tin cá nhân
- `error.html` - Trang lỗi 404 khi người dùng truy cập đường dẫn không tồn tại


---

### Bước 1: Tạo thư mục cho project

1. Mở **File Explorer** (Windows) hoặc **Finder** (Mac)
2. Tạo một thư mục mới tên `my-portfolio`
3. Mở thư mục này bằng text editor của bạn (VS Code, Notepad++, Sublime Text...)

---

### Bước 2: Tạo file `index.html`

#### 2.1. Tạo file mới
- Trong thư mục `my-portfolio`, tạo file mới tên `index.html`

#### 2.2. Copy code vào file

Mở file `index.html` và paste đoạn code sau:
```html
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portfolio - Tên Bạn</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: #333;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }
        header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 100px 0;
            text-align: center;
        }
        header h1 {
            font-size: 3em;
            margin-bottom: 10px;
        }
        header p {
            font-size: 1.2em;
            opacity: 0.9;
        }
        .section {
            padding: 60px 0;
        }
        .section:nth-child(even) {
            background: #f8f9fa;
        }
        h2 {
            color: #667eea;
            margin-bottom: 30px;
            font-size: 2em;
        }
        .skills {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }
        .skill-card {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            transition: transform 0.3s;
        }
        .skill-card:hover {
            transform: translateY(-5px);
        }
        .skill-card h3 {
            color: #667eea;
            margin-bottom: 10px;
        }
        .projects {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
            margin-top: 30px;
        }
        .project-card {
            background: white;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        .project-image {
            height: 200px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 3em;
        }
        .project-content {
            padding: 20px;
        }
        .project-content h3 {
            color: #667eea;
            margin-bottom: 10px;
        }
        footer {
            background: #333;
            color: white;
            text-align: center;
            padding: 30px 0;
        }
        .contact-btn {
            display: inline-block;
            background: white;
            color: #667eea;
            padding: 12px 30px;
            border-radius: 25px;
            text-decoration: none;
            margin-top: 20px;
            font-weight: bold;
            transition: transform 0.3s;
        }
        .contact-btn:hover {
            transform: scale(1.05);
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <h1>Xin chào, tôi là [Tên Bạn]</h1>
            <p>AWS Solutions Architect | Cloud Engineer</p>
        </div>
    </header>

    <section class="section">
        <div class="container">
            <h2>Về Tôi</h2>
            <p>Tôi là một kỹ sư cloud đam mê với AWS, chuyên về thiết kế và triển khai các giải pháp cloud scalable. 
            Với kinh nghiệm trong việc làm việc với EC2, S3, Lambda, và nhiều service AWS khác, tôi luôn tìm kiếm 
            cơ hội để học hỏi và áp dụng công nghệ mới.</p>
        </div>
    </section>

    <section class="section">
        <div class="container">
            <h2>Kỹ Năng</h2>
            <div class="skills">
                <div class="skill-card">
                    <h3>AWS Services</h3>
                    <p>EC2, S3, Lambda, RDS, VPC, CloudFront, Route 53, IAM</p>
                </div>
                <div class="skill-card">
                    <h3>Infrastructure as Code</h3>
                    <p>Terraform, CloudFormation, AWS CDK</p>
                </div>
                <div class="skill-card">
                    <h3>Containers & Orchestration</h3>
                    <p>Docker, Kubernetes, ECS, EKS</p>
                </div>
                <div class="skill-card">
                    <h3>Programming</h3>
                    <p>Python, JavaScript, Bash, SQL</p>
                </div>
            </div>
        </div>
    </section>

    <section class="section">
        <div class="container">
            <h2>Dự Án Nổi Bật</h2>
            <div class="projects">
                <div class="project-card">
                    <div class="project-image">🌐</div>
                    <div class="project-content">
                        <h3>Website Hosting trên S3</h3>
                        <p>Triển khai static website với S3, CloudFront và custom domain. 
                        Chi phí < $1/tháng, tốc độ load < 1s.</p>
                    </div>
                </div>
                <div class="project-card">
                    <div class="project-image">⚡</div>
                    <div class="project-content">
                        <h3>Serverless API</h3>
                        <p>Xây dựng RESTful API với Lambda, API Gateway và DynamoDB. 
                        Auto-scaling, không cần quản lý server.</p>
                    </div>
                </div>
                <div class="project-card">
                    <div class="project-image">🔐</div>
                    <div class="project-content">
                        <h3>Security Automation</h3>
                        <p>Tự động hóa security audit với Lambda, CloudWatch và SNS. 
                        Phát hiện và cảnh báo sớm các vấn đề bảo mật.</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <footer>
        <div class="container">
            <h2>Liên Hệ</h2>
            <p>Email: your.email@example.com</p>
            <p>GitHub: github.com/yourusername</p>
            <p>LinkedIn: linkedin.com/in/yourusername</p>
            <a href="mailto:your.email@example.com" class="contact-btn">Gửi Email</a>
            <p style="margin-top: 30px; opacity: 0.7;">© 2024 - Hosted on AWS S3 + CloudFront</p>
        </div>
    </footer>
</body>
</html>
```

### 2.3. Tùy chỉnh thông tin cá nhân

Thay đổi các thông tin sau cho phù hợp với bạn:
- Dòng 27: `[Tên Bạn]` → Thay bằng tên của bạn
- Dòng 28: Chức danh nghề nghiệp
- Dòng 35-37: Mô tả về bản thân
- Dòng 124-126: Thông tin liên hệ (email, GitHub, LinkedIn)

### 2.4. Lưu file

**Ctrl + S** (Windows) hoặc **Cmd + S** (Mac) để lưu file.

---

## Bước 3: Tạo file `error.html`

### 3.1. Tạo file mới
- Trong cùng thư mục `my-portfolio`, tạo file mới tên `error.html`

### 3.2. Copy code vào file
```html
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>404 - Không tìm thấy trang</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            text-align: center;
        }
        h1 { 
            font-size: 5em; 
            margin: 0; 
        }
        p { 
            font-size: 1.5em; 
        }
        a { 
            color: white; 
            text-decoration: underline; 
        }
    </style>
</head>
<body>
    <div>
        <h1>404</h1>
        <p>Oops! Không tìm thấy trang bạn đang tìm kiếm.</p>
        <p><a href="/">Quay về trang chủ</a></p>
    </div>
</body>
</html>
```

### 3.3. Lưu file

**Ctrl + S** (Windows) hoặc **Cmd + S** (Mac).

---

## Bước 4: Kiểm tra trước khi upload

1. Mở File Explorer/Finder
2. Tìm file `index.html`/ `error.html`
3. **Click đúp** vào file → Website sẽ mở trong trình duyệt mặc định

---

## Bước 5: Chuẩn bị cho bước tiếp theo

### Cấu trúc thư mục cuối cùng:
```
my-portfolio/
├── index.html
└── error.html
```

### Những gì bạn đã hoàn thành:

✅ Tạo được 2 file HTML cần thiết  
✅ Website có giao diện đẹp, responsive  
✅ Đã tùy chỉnh thông tin cá nhân  
✅ Test trên local browser thành công

---

## Lưu ý quan trọng

### Về encoding
- File phải được lưu với encoding **UTF-8** để hiển thị tiếng Việt đúng
- Hầu hết text editor hiện đại đã mặc định UTF-8

### Về tên file
- **QUAN TRỌNG:** Tên file phải là `index.html` (chữ thường, không dấu)
- S3 sẽ tự động tìm file này làm trang chủ


