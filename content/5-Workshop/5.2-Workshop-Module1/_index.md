---
title: "Module 1"
date: "2025-09-09T19:53:52+07:00"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## Guide: Create Website Files for the Portfolio

### Objective
In this module you will create two basic HTML files for your portfolio website:
- `index.html` - the home page showing personal information
- `error.html` - the 404 error page when users access a non-existing path

---

### Step 1: Create a project folder

1. Open **File Explorer** (Windows) or **Finder** (Mac).
2. Create a new folder named `my-portfolio`.
3. Open that folder with your text editor (VS Code, Notepad++, Sublime Text, etc.).

---

### Step 2: Create the `index.html` file

#### 2.1. Create a new file
- Inside the `my-portfolio` folder, create a new file named `index.html`.

#### 2.2. Copy the code into the file

Open `index.html` and paste the following code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portfolio - Your Name</title>
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
            <h1>Hello, I'm [Your Name]</h1>
            <p>AWS Solutions Architect | Cloud Engineer</p>
        </div>
    </header>

    <section class="section">
        <div class="container">
            <h2>About Me</h2>
            <p>I am a cloud engineer passionate about AWS, specializing in designing and deploying scalable cloud solutions. With experience working with EC2, S3, Lambda, and other AWS services, I am always looking to learn and apply new technologies.</p>
        </div>
    </section>

    <section class="section">
        <div class="container">
            <h2>Skills</h2>
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
            <h2>Featured Projects</h2>
            <div class="projects">
                <div class="project-card">
                    <div class="project-image">🌐</div>
                    <div class="project-content">
                        <h3>Website Hosting on S3</h3>
                        <p>Deploy a static website with S3, CloudFront, and a custom domain. Low cost (&lt; $1/month) and fast load times (&lt; 1s).</p>
                    </div>
                </div>
                <div class="project-card">
                    <div class="project-image">⚡</div>
                    <div class="project-content">
                        <h3>Serverless API</h3>
                        <p>Build a RESTful API using Lambda, API Gateway, and DynamoDB. Auto-scaling with no server management.</p>
                    </div>
                </div>
                <div class="project-card">
                    <div class="project-image">🔐</div>
                    <div class="project-content">
                        <h3>Security Automation</h3>
                        <p>Automate security audits using Lambda, CloudWatch, and SNS. Early detection and alerts for security issues.</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <footer>
        <div class="container">
            <h2>Contact</h2>
            <p>Email: your.email@example.com</p>
            <p>GitHub: github.com/yourusername</p>
            <p>LinkedIn: linkedin.com/in/yourusername</p>
            <a href="mailto:your.email@example.com" class="contact-btn">Send Email</a>
            <p style="margin-top: 30px; opacity: 0.7;">© 2024 - Hosted on AWS S3 + CloudFront</p>
        </div>
    </footer>
</body>
</html>
```

#### 2.3. Customize personal information

Replace the following with your own details:
- Line with `[Your Name]` → replace with your name
- Job title line
- About section paragraph
- Contact info (email, GitHub, LinkedIn)

#### 2.4. Save the file

Use **Ctrl + S** (Windows) or **Cmd + S** (Mac) to save.

---

## Step 3: Create the `error.html` file

### 3.1. Create a new file
- In the same `my-portfolio` folder, create a file named `error.html`.

### 3.2. Copy the code into the file

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>404 - Page Not Found</title>
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
        <p>Oops! The page you're looking for cannot be found.</p>
        <p><a href="/">Return to the home page</a></p>
    </div>
</body>
</html>
```

### 3.3. Save the file

Use **Ctrl + S** (Windows) or **Cmd + S** (Mac).

---

## Step 4: Quick local check

1. Open File Explorer/Finder and locate `index.html` and `error.html` inside `my-portfolio`.
2. Double-click a file to open it in your default browser and verify layout and content.

---

## Step 5: Final folder structure

```
my-portfolio/
├── index.html
└── error.html
```

### What you've completed

- Created two required HTML files
- Built a simple responsive layout
- Customized personal information placeholder
- Verified local testing steps

---

## Important notes

- Encoding: Save files as **UTF-8** to avoid character issues.
- Filenames: Use lowercase `index.html` (S3 looks for this name as the default root file).

If you want, I can also create a committed patch for these files or add translated versions of other workshop pages.
