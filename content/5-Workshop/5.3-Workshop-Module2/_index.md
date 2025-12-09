---
title: "Module 2"
date: "2025-09-09T19:53:52+07:00"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Upload Website to Amazon S3

## Objective
In this module you will:
- Create an S3 bucket to store the website
- Upload the HTML files to S3
- Configure S3 to host a static website
- Your site will get a public URL and be accessible from anywhere!

---

## Background

### What is S3?
**Amazon S3 (Simple Storage Service)** is AWS's object storage service. You can think of S3 as a huge "cloud hard drive" for storing files.

### What is S3 Static Website Hosting?
S3 offers a feature that lets you host static websites (HTML, CSS, JS, images) without a web server. Key benefits:
- Cheap (~$0.023/GB/month)
- No server management
- Automatic scaling for traffic spikes
- 11 nines of durability (99.999999999%)

---

## Step 1: Access the AWS Console

### 1.1 Sign in to AWS

1. Open your browser and go to: https://console.aws.amazon.com
2. Sign in with either:
	 - **Root user email** + password, OR
	 - **IAM user** (if provided by your organization)
3. Select the region **Asia Pacific (Singapore) ap-southeast-1** in the top-right corner

> **Why Singapore?** It's geographically close to Vietnam, so it provides lower latency and reasonable pricing.

### 1.2 Find the S3 service

- Type "S3" into the service search box in the console header

<div style="text-align: center;">
	<img src="/images/5-Workshop/5.3-Workshop-Module2/S3.png" alt="S3" style="width:100%" />
</div>

---

## Step 2: Create an S3 Bucket

### 2.1 Start creating the bucket

1. In the **S3 Console**, click the orange **Create bucket** button
<div style="text-align: center;">
	<img src="/images/5-Workshop/5.3-Workshop-Module2/Create_S3.png" alt="Create_S3" style="width:100%" />
</div>
2. The bucket creation form includes several options

### 2.2 General configuration

**Bucket name:**
```
portfolio-yourname-2025
```

> **Important notes about bucket names:**
> - The name must be **globally unique** (no other AWS account can use the same name)
> - Use only lowercase letters, numbers and hyphens (-)
> - No Vietnamese characters, no spaces
> - Length between 3 and 63 characters
>
> **Valid examples:**
> - `portfolio-anhnguyen-2024`
> - `my-awesome-website-123`
> - `test-bucket-hcm`

**AWS Region:**
```
Asia Pacific (Singapore) ap-southeast-1
```

### 2.3 Object ownership
```
ACLs disabled (recommended)
```

> **Explanation:** ACLs are an older access control mechanism. AWS recommends using Bucket Policies for easier management.

<div style="text-align: center;">
	<img src="/images/5-Workshop/5.3-Workshop-Module2/Config1.png" alt="Config_S3" style="width:100%" />
</div>

### 2.4 Block Public Access configuration

**IMPORTANT - READ CAREFULLY:**

By default AWS blocks all public access for security. Because this bucket will host a public website, you need to disable those blocks for this bucket:

1. **Uncheck** all 4 options under "Block Public Access":
	 - Block all public access
	 - Block public access to buckets and objects granted through new access control lists (ACLs)
	 - Block public access to buckets and objects granted through any access control lists (ACLs)
	 - Block public access to buckets and objects granted through new public bucket or access point policies

2. A yellow security warning will appear after you uncheck them

> **Security warning:** Only do this for a bucket that hosts a public website. Do not disable public access on buckets that store sensitive data.

<div style="text-align: center;">
	<img src="/images/5-Workshop/5.3-Workshop-Module2/Config2.png" alt="Config_S3" style="width:100%" />
</div>

### 2.5 Finish

Click the **Create bucket** button at the bottom of the page.

---

## Step 3: Upload Files to S3

### 3.1 Open the newly created bucket

1. In the buckets list, click the name of the bucket you created
2. You'll land on the Objects view (file listing)

<div style="text-align: center;">
	<img src="/images/5-Workshop/5.3-Workshop-Module2/Config3.png" alt="Config_S3" style="width:100%" />
</div>

### 3.2 Upload files

1. Click the orange **Upload** button
2. Click **Add files**
3. Select the two files: `index.html` and `error.html` from your `my-portfolio` folder
4. Click **Open**

You should see both files listed:
```
📄 index.html
📄 error.html
```

### 3.3 Keep default upload settings

1. Scroll to the bottom and click **Upload**
2. Wait for the progress bar to reach 100%
3. You should see "Upload succeeded" for both files
4. Click **Close** to return to the bucket view

### 3.5 Confirm upload

You should now see the two files in the bucket:
```
Name                Type        Size        Last modified
index.html          text/html   ~8 KB       Just now
error.html          text/html   ~1 KB       Just now
```

**Checkpoint:** Files are uploaded to S3!

---

## Step 4: Enable Static Website Hosting

### 4.1 Open Properties tab

1. While inside the bucket, click the **Properties** tab
2. Scroll to the bottom

### 4.2 Find Static website hosting

Locate the **Static website hosting** section — it should display `Disabled` initially

### 4.3 Enable and configure

1. Click **Edit** inside the Static website hosting section
2. Configure as shown in the screenshots

<div style="text-align: center;">
	<img src="/images/5-Workshop/5.3-Workshop-Module2/Config4.png" alt="Config_S3" style="width:100%" />
</div>

3. Click **Save changes**

### 4.4 Save the Website Endpoint URL

After saving, the **Bucket website endpoint** will appear. Example:
```
http://portfolio-yourname-2025.s3-website-ap-southeast-1.amazonaws.com
```

> 📝 **Important:** Copy this URL and save it (Notepad/Notes). The URL format is:
> `http://[bucket-name].s3-website-[region].amazonaws.com`

✅ **Checkpoint:** Static website hosting is enabled.

---

## Step 5: Configure Bucket Policy (Allow Public Read)

### 5.1 Why the site may still be inaccessible

If you open the endpoint URL now you may get:
```
403 Forbidden
Access Denied
```

This happens because objects are not yet publicly readable.

### 5.2 Open the Permissions tab

1. Click the **Permissions** tab
2. Scroll to **Bucket policy**

### 5.3 Add a Bucket Policy

1. Click **Edit** in the Bucket policy section
2. Paste the following JSON into the editor:

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

3. Replace `YOUR-BUCKET-NAME` with your actual bucket name (e.g. `portfolio-anhnguyen-2025`)

### 5.4 What the policy does

Explanation:
```
"Effect": "Allow"           → Allows access
"Principal": "*"            → Everyone
"Action": "s3:GetObject"    → Read/download objects
"Resource": "arn:aws:s3:::portfolio-anhnguyen-2025/*"
```

> **Security note:** This policy only grants read access; it does not allow delete, write, or upload operations.

### 5.5 Save the policy

Click **Save changes**

**Checkpoint:** Bucket policy configured for public read access.

---

## Step 6: Test the Website

### 6.1 Open the website

1. Open a new browser tab
2. Paste the endpoint URL saved in step 4.4:
```
http://portfolio-yourname-2025.s3-website-ap-southeast-1.amazonaws.com
```
3. Press Enter

### 6.2 Expected result

You should see the portfolio website with:
- A purple-blue gradient header
- Your name displayed
- Sections: About, Skills, Projects, Contact
- A footer at the bottom

### 6.3 Test the 404 page

Open a non-existing path, e.g.:
```
http://portfolio-yourname-2025.s3-website-ap-southeast-1.amazonaws.com/test123
```

You should see the 404 page with:
- A large "404" number
- Message "Oops! The page you're looking for cannot be found."
- A link back to the homepage

### 6.4 Share with others

Your site is now live and accessible worldwide — copy the URL and share it with friends for testing.

---

## 🎉 Congratulations — Module 2 Complete!

### What you accomplished:

- Created a globally unique S3 bucket
- Uploaded two HTML files to S3
- Enabled Static Website Hosting
- Configured a public Bucket Policy
- Verified the site is online and tested the 404 page

If you want, I can also create an English version of other workshop pages or make small edits to improve wording for documentation.

