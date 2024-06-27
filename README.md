# Resume Deployment on AWS

This project demonstrates the deployment of a personal resume on AWS using the following services: Amazon S3, CloudFront, Certificate Manager, and Route 53. This setup ensures a highly available, secure, and fast-loading resume website.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Setup Instructions](#setup-instructions)
  - [1. Create an S3 Bucket](#1-create-an-s3-bucket)
  - [2. Upload Resume to S3](#2-upload-resume-to-s3)
  - [3. Configure S3 Bucket for Static Website Hosting](#3-configure-s3-bucket-for-static-website-hosting)
  - [4. Set Up CloudFront Distribution](#4-set-up-cloudfront-distribution)
  - [5. Obtain SSL Certificate with Certificate Manager](#5-obtain-ssl-certificate-with-certificate-manager)
  - [6. Configure Route 53 for Custom Domain](#6-configure-route-53-for-custom-domain)
- [Testing and Verification](#testing-and-verification)
- [Conclusion](#conclusion)

## Prerequisites

- An AWS account
- A registered domain name
- Basic understanding of AWS services (S3, CloudFront, Certificate Manager, Route 53)

## Architecture

![webimage](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/20a68cf9-c47b-435a-a90e-ec81aaf9dd2e)

The deployment architecture includes:

- **Amazon S3**: Stores the static resume files.
- **CloudFront**: Distributes the content globally with low latency.
- **Certificate Manager**: Manages SSL/TLS certificates for secure connections.
- **Route 53**: Manages the DNS settings for the custom domain.

## Setup Instructions

### 1. Create an S3 Bucket

1. Go to the S3 service in the AWS Management Console.
2. Click "Create bucket".
3. Enter a unique bucket name and select the appropriate region.
4. Uncheck "Block all public access" and acknowledge that the bucket will be public.
5. Click "Create bucket".

### 2. Upload Resume to S3

1. Open your newly created S3 bucket.
2. Click "Upload" and select your resume files (i.e, `index.html`, `style.css`).
   ![WhatsApp Image 2024-06-27 at 11 42 50 AM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/cf442531-8915-4065-8f4d-b07a1bd232ef)


### 3. Configure S3 Bucket for Static Website Hosting

1. In the S3 bucket properties, click on "Static website hosting" and choose "Enable".
2. On the hosting type select "Host a static website".
3. Enter `index.html` as the index document.
4. Note the endpoint provided.

   ![WhatsApp Image 2024-06-27 at 11 45 53 AM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/78556b73-f9f4-4765-9fb8-3eae759bacdc)


### 4. Obtain SSL Certificate with Certificate Manager

1. Go to the Certificate Manager in the AWS Management Console.
2. Click "Request a certificate".
3. Choose "Request a public certificate" and click "Request a certificate".
4. Click on "Request a Public Certificate" and choose "Next"
   ![WhatsApp Image 2024-06-27 at 11 59 29 AM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/cf4ce88f-271f-4c24-b77e-dfb924c195d3)
5. Enter your domain name.
   ![WhatsApp Image 2024-06-27 at 12 02 50 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/f4e0324b-8c3c-4dee-97ce-089a72e73dfc)

6. Click on Validate the domain ownership via DNS.
7. Click on "Request".
   ![WhatsApp Image 2024-06-27 at 12 05 31 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/5c2c928c-20cf-4110-aad9-848c91e13b23)

   
### 4. Set Up CloudFront Distribution

1. Go to the CloudFront service in the AWS Management Console.
2. Click "Create Distribution".
3. In the "Origin Settings", select the S3 bucket domain and choose use website endpoint.
   ![WhatsApp Image 2024-06-27 at 11 56 18 AM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/7e1436f7-6657-4880-a1b9-f27fb3b1aba9)

5. Configure the distribution settings as desired, ensuring to set the default root object to `index.html`.
6. Click "Create Distribution".



### 6. Configure Route 53 for Custom Domain

1. Go to the Route 53 service in the AWS Management Console.
2. Click on "Hosted zones" and select your domain.
3. Create a new A record and select "Alias" to your CloudFront distribution.
4. Ensure your domain's name servers are set correctly with your domain registrar.

## Testing and Verification

1. Wait for the CloudFront distribution to deploy completely.
2. Access your resume website via the custom domain to ensure everything is working correctly.
3. Verify that the website is accessible over HTTPS.

## Conclusion

By following these steps, you have successfully deployed your resume on AWS using S3, CloudFront, Certificate Manager, and Route 53. This setup provides a scalable, secure, and globally distributed platform for showcasing your resume.

---

Feel free to modify and expand upon this template based on your specific project details and personal preferences!
