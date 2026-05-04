# **Project : Static Website Hosting on AWS S3 with HTTPS using CloudFront**

## 📌 Overview

This project demonstrates how to host a **static website** using AWS S3 and deliver it securely over HTTPS using CloudFront

---

## Concept

A **static website** is an application that does not require:

- Server-side code execution
- Backend APIs
- Database interactions

It consists only of:

- HTML
- CSS
- JavaScript
- Images

---

## Requirements

To build this architecture, we need:

- **Application** → Static website files
- **Storage** → Store website files
- **Public Access** → Allow users to access the website

---

## AWS Services Used

- **Amazon S3** → Store static website files
- **CloudFront** → Content delivery (CDN) + HTTPS

---

## Architecture

```
User → DNS → CloudFront → S3 → CloudFront → User
```

---

## Project Workflow

### 1. Build the Application

You create a static application (e.g., HTML/CSS/JS).

---

### 2. Store in S3

- Upload your application files to an S3 bucket
- Ensure files are at the **root level** (not inside folders like `dist/`)

---

### 3. Enable Public access to the bucket

To provide the public access to the S3 bucket 

1. Uncheck the Block Public access option
2. Define the rule Bucket policy rule providing User with the public access can access what ?

S3 Get object

<img width="1114" height="550" alt="image" src="https://github.com/user-attachments/assets/de948ee3-c067-412a-a3fd-ea306b9ad321" />


![image.png](attachment:f1c987d5-21e9-40e9-ac05-d7cd2a9f0d28:image.png)

### 4. Enable Static Website Hosting

- Enable **Static Website Hosting** in S3
- Set:
    - Index document → `index.html`

S3 provides a URL:

```
http://bucket-name.s3-website-region.amazonaws.com
```

When a user opens this:

> S3 directly returns the static files
> 

![image.png](attachment:60a1616a-7b3c-4867-b4ee-72f4df25728e:image.png)

---

## Enabling SSL (HTTPS)

S3 alone does **not support HTTPS for static websites**, so we use CloudFront.

---

## CloudFront Role

CloudFront acts as:

- CDN (faster delivery via edge locations)
- Security layer (HTTPS/SSL)

### Key Responsibilities:

- Fetch content from S3 (origin)
- Deliver content globally with low latency
- Encrypt traffic using SSL

---

## CloudFront Configuration

### 1. Set Origin

- Origin = S3 bucket

---

### 2. Viewer Protocol Policy

- Redirect HTTP → HTTPS

---

### 3. Default Root Object

```
index.html
```

This ensures:

```
/ → /index.html
```

---

1. Browse the Distribution Domain name 

![image.png](attachment:dba85d3e-3db2-4164-9033-d1ff5214370f:image.png)

![image.png](attachment:90ebed55-e903-48d6-99bf-e384dca4b2ef:image.png)

## Request Flow (Important)

```
1. User → https://www.mywebsite.com
2. DNS → points to CloudFront
3. CloudFront:
4. Secure connection established
5. CloudFront fetches content from S3
6. CloudFront returns content to user
```

---

## Key Learnings

- S3 can host **only static content**
- CloudFront is required for **HTTPS + performance**
- Default root object is critical (`index.html`)
- Files must be at **bucket root**, not inside folders
- CloudFront acts as a **secure proxy** between user and S3

---

## Final Outcome

You achieve:

- Fully hosted static website
- HTTPS enabled
- Global content delivery
- Scalable & cost-effective architecture

---

## Future Improvements

- Add custom domain with Route 53
- Use Origin Access Control (OAC) for secure S3 access
- Automate using Terraform
- Add CI/CD pipeline

---

## Summary

> Static website hosting in S3 allows you to serve frontend applications efficiently. By integrating CloudFront and SSL, you enhance security, performance, and user experience.
> 

---

## CloudFront Configuration Screenshots

### 1. Create a distribution

![image.png](attachment:7a97837b-9bee-48ae-9dde-bdc964ad3a60:image.png)

### 2. Specify the Origin type as S3

![image.png](attachment:d981b7f0-979e-4299-b50e-f377fe854a7a:image.png)

![image.png](attachment:a01f5c6c-dc93-4577-a6c9-de4266d02fbc:image.png)

### 3. Considering cost i am avoiding enabling WAF

![image.png](attachment:1f948a3a-b935-402b-afba-26c896d77ff3:image.png)

### 4. Click on create distribution

![image.png](attachment:87e1fa92-6635-4b17-985b-7c01fcf119f3:image.png)

### 5. Provide Public access to the Origin in the Origin section

Select the origin and click on edit 

![image.png](attachment:ddf659e0-e79a-4197-932a-35faab3e4881:image.png)

![image.png](attachment:255e9d54-bd42-4299-a60c-dae1324f99b1:image.png)

### 6. Provide the **Default root object to the static html page**

![image.png](attachment:030afa91-5ddc-4486-bf53-c2faeb258ba7:image.png)

![image.png](attachment:353d2dcf-6975-4cbd-9cf7-519030171d32:image.png)

![image.png](attachment:792230d8-d7e2-405f-9de2-29a7c0b66932:image.png)

![image.png](attachment:23c0b623-94ea-4d43-8f26-6b231fda234c:image.png)

![image.png](attachment:7ad61cdc-f9b9-45de-b18e-c1bfaf7d3ed1:image.png)

![image.png](attachment:6baf61fd-d363-443d-b5fe-646499831784:image.png)

![image.png](attachment:fec7c8b4-2263-46b5-af1a-310c121b3235:image.png)
