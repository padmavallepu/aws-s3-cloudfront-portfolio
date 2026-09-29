# 🌐 Static Website Hosting on AWS (S3 + CloudFront)

A personal portfolio website hosted on **AWS S3** and delivered globally via **CloudFront CDN**, secured with HTTPS using AWS Certificate Manager, and mapped to a custom domain via Route 53.

---

## 🏗️ Architecture

```
User → Route 53 (DNS) → CloudFront (CDN + HTTPS) → S3 Bucket (Static Files)
                              ↑
                     ACM SSL Certificate
```

> See `architecture.png` for the full diagram.

---

## ✨ Features

- Static website hosted on **AWS S3** with public access and bucket policy
- **CloudFront** distribution for global content delivery with low latency
- **HTTPS enforced** via HTTP-to-HTTPS redirect policy
- **Custom domain** configured using Route 53 (A Alias record)
- **SSL/TLS certificate** provisioned via AWS Certificate Manager (ACM)
- **Automated deployments** using AWS CLI with cache invalidation

---

## 🧰 AWS Services Used

| Service | Purpose |
|---|---|
| S3 | Store and serve static website files |
| CloudFront | Global CDN + HTTPS termination |
| Route 53 | Custom domain DNS management |
| ACM | Free SSL/TLS certificate |
| IAM | Bucket policy for public read access |

---

## 🚀 Deployment

### Prerequisites

- AWS account (free tier works)
- AWS CLI installed and configured (`aws configure`)
- A static website (HTML/CSS/JS files)

### 1. Create and configure S3 bucket

```bash
# Create bucket
aws s3 mb s3://your-bucket-name

# Enable static website hosting
aws s3 website s3://your-bucket-name \
  --index-document index.html \
  --error-document error.html
```

Apply this bucket policy to allow public read access:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```

### 2. Upload website files

```bash
aws s3 sync ./your-folder s3://your-bucket-name --delete
```

### 3. Create CloudFront distribution

- Origin: your S3 website endpoint
- Viewer Protocol Policy: Redirect HTTP to HTTPS
- Default Root Object: `index.html`

### 4. Deploy updates

```bash
# Sync files to S3
aws s3 sync ./your-folder s3://your-bucket-name --delete

# Invalidate CloudFront cache
aws cloudfront create-invalidation \
  --distribution-id YOUR_DISTRIBUTION_ID \
  --paths "/*"
```

---

## 📁 Project Structure

padma-responsive-portfolio
├── index.html
├── about.html
├── education.html
├── skills.html
├── projects.html
├── contact.html
│
└── assets
    ├── profile.jpg  
    ├── style.css
    └── script.js

---

## 💡 What I Learned

- Configuring S3 for static website hosting and managing bucket policies
- Setting up a CloudFront distribution with custom origin and HTTPS
- Using ACM to provision and attach an SSL certificate to CloudFront
- Managing DNS with Route 53 and creating Alias records
- Automating deployments with AWS CLI and cache invalidation

---

## 📌 Live Demo

🔗 [your-domain.com](https://d1l92miu16h8b3.cloudfront.net)  
🔗 [CloudFront URL](https://d1l92miu16h8b3.cloudfront.net)
