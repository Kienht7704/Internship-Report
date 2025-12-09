---
title: "Module 3"
date: "2025-09-09T19:53:52+07:00"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Add CloudFront - HTTPS & Global CDN

## Objective
In this module you will:
- Create a CloudFront distribution to deliver your website globally
- Enable free HTTPS for your website
- Speed up page load times by 5-10x
- Make your website more professional with a green lock icon in the address bar

---

## Background

### What is CloudFront?

**Amazon CloudFront** is AWS's Content Delivery Network (CDN) service.

**How it works:**
```
User in Hanoi → CloudFront Edge (Hanoi) → Return content instantly
User in Tokyo → CloudFront Edge (Tokyo) → Return content instantly
User in London → CloudFront Edge (London) → Return content instantly
```

Instead of all users accessing S3 in Singapore, they access the CloudFront edge location nearest to them.

### Why do you need CloudFront?

#### 1. Free HTTPS (Security)
- S3 static websites only support HTTP
- CloudFront provides free HTTPS
- Green lock in browser → Increased trust
- Google prioritizes HTTPS websites in search results

#### 2. Faster load times
**Without CloudFront:**
- Users in Vietnam accessing S3 Singapore: ~50-100ms
- Users in Europe accessing S3 Singapore: ~300-500ms
- Users in the US accessing S3 Singapore: ~200-400ms

**With CloudFront:**
- All users: ~20-50ms (accessing the nearest edge location)
- **5-10x improvement!**

#### 3. More professional
- Shorter URL: `d123abc.cloudfront.net` vs `bucket.s3-website-region.amazonaws.com`
- Support for custom domains
- Advanced features (geo-blocking, custom headers, Lambda@Edge)

---

## Step 1: Access CloudFront Console

### 1.1 Open CloudFront

From the AWS Console:
- Type "CloudFront" in the service search box:

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.4-Workshop-Module3/CloudFront.png" alt="CloudFront" style="width:100%" />
</div>

### 1.2 Welcome screen

If this is your first time using CloudFront, you'll see a welcome page.

Click the orange **Create distribution** button.

---

## Step 2: Configure Origin (Content Source)

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

## Step 7: Test CloudFront

### 7.1 Copy Distribution Domain Name

In the distributions list or in the distribution details:
```
Domain name: d1234567890abcd.cloudfront.net
```

Copy this domain.

### 7.2 Access the website via HTTPS

Open your browser and enter:
```
https://d1234567890abcd.cloudfront.net
```

>  **Note:** Remember to add `https://` at the beginning!

### 7.3 Expected result

 **Website displays correctly:**
- Purple-blue gradient header
- All content renders properly
- Layout is not broken

 **Green lock in the address bar:**
```
 https://d1234567890abcd.cloudfront.net
```

Click the lock to see:
```
Connection is secure
Certificate is valid
```

### 7.4 Test HTTP to HTTPS redirect

Try accessing with HTTP (no 's'):
```
http://d1234567890abcd.cloudfront.net
```

 The browser will **automatically redirect** to:
```
https://d1234567890abcd.cloudfront.net
```

### 7.5 Test 404 page

Access a non-existent URL:
```
https://d1234567890abcd.cloudfront.net/test-404
```

 Your error.html page will display (large "404" with gradient background)

---

##  Understanding CloudFront Better

### How caching works

**First request:**
```
User → CloudFront Edge → (MISS) → S3 → CloudFront → User
        └─ Save to cache
```

**Subsequent requests:**
```
User → CloudFront Edge → (HIT) → User
        (Serve from cache instantly)
```

### Cache duration

Default: **24 hours** (using CachingOptimized policy)

After 24 hours, CloudFront checks S3 to see if files have changed.

### What is Invalidation?

When you update files on S3, CloudFront still serves the old cached version.

**How to force CloudFront to update immediately:**
1. Go to CloudFront Console → Select your distribution
2. Go to "Invalidations" tab → Click "Create invalidation"
3. Object paths: `/*` (invalidate all)
4. Click "Create invalidation"
5. Wait 1-2 minutes → Fresh cache!

> **Note:** The Free Tier includes 1,000 free invalidation paths per month.
