# AWS S3 Static Portfolio with CI/CD using GitHub Actions

This project demonstrates how to host a static portfolio website on AWS S3 and automate deployments using GitHub Actions (CI/CD pipeline).

---

## Project Overview

The goal of this project is to:

- Host a static website (portfolio) using AWS S3  
- Enable public access for web hosting  
- Automate deployment using GitHub Actions  
- Sync local changes to S3 on every push to the main branch  

---

## Architecture

Developer → GitHub Repo → GitHub Actions → AWS S3 Bucket → Live Website

---

## Technologies Used

- AWS S3 (Static Website Hosting)  
- GitHub Actions (CI/CD)  
- HTML (Portfolio Website)  
- AWS CLI  

---

## S3 Bucket Setup

### 1. Create S3 Bucket
- Bucket Name: `my-s3-web-portfolio`  
- Region: `us-east-1`  

### 2. Enable Public Access
- Disable block public access settings  
- Allow public access to objects  

### 3. Enable Static Website Hosting
- Go to **Properties → Static Website Hosting**  
- Enable hosting  
- Set:
  - Index document: `index.html`  

---

## Bucket Policy

Apply the following policy to allow public read access:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-s3-web-portfolio/*"
    }
  ]
}
```
CI/CD Pipeline (GitHub Actions)

This workflow automatically deploys the website to S3 whenever changes are pushed to the main branch.

Workflow File

name: Portfolio Deployment

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v1

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Deploy static site to S3 bucket
        run: aws s3 sync . s3://my-s3-web-portfolio --delete

GitHub Secrets Configuration

Add the following secrets in your GitHub repository:

AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY

Path:
GitHub Repo → Settings → Secrets → Actions → New repository secret

Deployment Workflow
1. Make changes to your index.html or other files
2. Commit and push to the main branch
3. GitHub Actions triggers automatically
4. Files are synced to S3 bucket
5. Website updates live



Access Your Website
http://my-s3-web-portfolio.s3-website-us-east-1.amazonaws.com

## Key Learnings
- Hosting static websites on AWS S3
- Managing S3 bucket permissions and policies
- Automating deployments with GitHub Actions
- Securely handling AWS credentials using GitHub Secrets
= Using aws s3 sync for efficient deployments

## Best Practices

- Never hardcode AWS credentials in your code
- Use IAM users with least privilege access
- Consider using CloudFront with HTTPS for production setups
- Enable versioning on S3 for rollback capability


## Future Improvements
- Add CloudFront for CDN and HTTPS
- Implement custom domain using Route 53
- Add cache invalidation in CI/CD pipeline
- Use Infrastructure as Code (Terraform)
