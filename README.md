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


<img width="1000" height="439" alt="image" src="https://github.com/user-attachments/assets/635a1ae4-4d65-4bf4-aef5-710873c3d48d" />


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

<img width="1353" height="688" alt="image" src="https://github.com/user-attachments/assets/cbe342f3-9104-4655-821d-91d3bd1c769f" />


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

<img width="1339" height="250" alt="image" src="https://github.com/user-attachments/assets/54c8d669-2cff-472e-8625-0edbe8d24a29" />


<img width="1337" height="699" alt="image" src="https://github.com/user-attachments/assets/472eeea8-4270-4816-a563-2ee654b8748c" />


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

<img width="1345" height="690" alt="image" src="https://github.com/user-attachments/assets/ba91da53-0ba7-4232-94a8-f8a9ef533165" />


### 2. Specify the Origin type as S3

<img width="1363" height="623" alt="image" src="https://github.com/user-attachments/assets/9569f18b-fae3-47dc-915d-7be30086cce5" />


<img width="1363" height="623" alt="image" src="https://github.com/user-attachments/assets/c8b34829-a05b-4c76-accb-c86cc807d565" />


### 3. Considering cost i am avoiding enabling WAF

<img width="1363" height="623" alt="image" src="https://github.com/user-attachments/assets/4e3601cc-39b0-4dcc-bd24-69978ba5bc33" />


### 4. Click on create distribution

<img width="1363" height="623" alt="image" src="https://github.com/user-attachments/assets/c7e37bf8-95fa-4174-aa6f-40171dd55644" />


### 5. Provide Public access to the Origin in the Origin section

Select the origin and click on edit 

<img width="1363" height="623" alt="image" src="https://github.com/user-attachments/assets/2cdb1cc3-4b87-40d6-9bef-62f8a4927962" />


<img width="1363" height="623" alt="image" src="https://github.com/user-attachments/assets/d2a63a69-2b78-4e63-8458-7746f91ae168" />


### 6. Provide the **Default root object to the static html page**

<img width="1363" height="623" alt="image" src="https://github.com/user-attachments/assets/e032673d-ead8-4fac-9b81-880e2baabecf" />


<img width="1346" height="310" alt="image" src="https://github.com/user-attachments/assets/11e1f163-4dc7-4bf0-8197-7d8449341b55" />


<img width="1344" height="617" alt="image" src="https://github.com/user-attachments/assets/3caa23ad-0428-40d2-b78e-1ae1e6136f0b" />


<img width="1344" height="617" alt="image" src="https://github.com/user-attachments/assets/16cc8e71-d06e-4731-95da-de8e1fce00bf" />


<img width="1344" height="617" alt="image" src="https://github.com/user-attachments/assets/f2d1b4f8-9b2d-4dfd-a061-bae3968893fa" />


<img width="1362" height="353" alt="image" src="https://github.com/user-attachments/assets/6bf807c1-8e4f-4685-9bc3-496725818dbf" />


<img width="1345" height="690" alt="image" src="https://github.com/user-attachments/assets/03448cdf-a78b-456b-b3b3-0c864554d173" />

