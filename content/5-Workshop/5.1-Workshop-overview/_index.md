---
title: "Introduction"
date: "2025-09-09T19:53:52+07:00"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## Workshop Overview

In this workshop you will build a complete **personal portfolio website** using a **serverless** architecture on AWS — no server management or scaling worries required.

### Workshop Architecture

The workshop leverages two main AWS services:

- **Amazon S3 (Simple Storage Service)** — serves as the static web server for files such as HTML, CSS and images. S3 Static Website Hosting allows low-cost hosting and virtually unlimited storage.

- **Amazon CloudFront** — a Content Delivery Network (CDN) that accelerates your site by delivering content from the edge location nearest to the user, instead of fetching it from a centralized origin. CloudFront also provides **free HTTPS** via AWS Certificate Manager.

<div style="text-align: center;">
  <img src="/images/5-Workshop/5.1-Workshop-overview/Architect-Workshop.png" alt="Workshop Architecture" style="width:100%" />
</div>

### Benefits of this Architecture

**Performance:** Content is served from the edge location closest to the user, reducing latency and improving page load times.

**Cost:** With the AWS Free Tier you get 12 months that include 5 GB of S3 storage, 1 TB of CloudFront transfer, and 10 million requests. After the Free Tier, hosting costs are typically very low (roughly ~$0.60/month for light traffic).

**Operations:** No server management required — no patching or manual scaling. AWS handles availability and scaling for you.

**Security:** HTTPS is available by default, S3 bucket policies control access, and CloudFront includes AWS Shield Standard for basic DDoS protection.