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

## 1. Create an S3 Bucket

1. Go to the S3 service in the AWS Management Console.
2. Click "Create bucket".
3. Enter a unique bucket name and select the appropriate region.
4. Uncheck "Block all public access" and acknowledge that the bucket will be public.
5. Click "Create bucket".

### Upload Resume to S3

1. Open your newly created S3 bucket.
2. Click "Upload" and select your resume files (i.e, `index.html`, `style.css`).
   ![WhatsApp Image 2024-06-27 at 11 42 50 AM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/cf442531-8915-4065-8f4d-b07a1bd232ef)


### Configure S3 Bucket for Static Website Hosting

1. In the S3 bucket properties, click on "Static website hosting" and choose "Enable".
2. On the hosting type select "Host a static website".
3. Enter `index.html` as the index document.
4. Note the endpoint provided.

   ![WhatsApp Image 2024-06-27 at 11 45 53 AM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/78556b73-f9f4-4765-9fb8-3eae759bacdc)


## 2. Obtain SSL Certificate with Certificate Manager

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


## 3. Configure Route 53 for Custom Domain

1. Go to the Route 53 service in the AWS Management Console.
2. Click on "Create Hosted zones" and enter your domain name.
3. Click on "Hosten Zones"
   
### Update Your Domain's Nameservers
After creating the hosted zone, Route 53 will provide you with a list of nameservers.
Go to your domain registrar's website (where you purchased your domain) and update the nameservers to the ones provided by Route 53. 
![WhatsApp Image 2024-06-27 at 12 46 05 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/b743f789-8d81-4d21-8dd6-d30dfe2adab3)


## 4. Set Up CloudFront Distribution

1. Go to the CloudFront service in the AWS Management Console.
2. Click "Create Distribution".
3. In the "Origin Settings", select the S3 bucket domain and choose use website endpoint.
   ![WhatsApp Image 2024-06-27 at 11 56 18 AM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/7e1436f7-6657-4880-a1b9-f27fb3b1aba9)

4. On the 'Viewer' select 'Redirect HTTP to HTTPS'
![WhatsApp Image 2024-06-27 at 12 11 06 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/9d03f55c-cd92-4c54-b909-a7a27641d2c7)
5. On the 'Web Application Firewall' select 'Do not enable security protections'.
6. Choose 'Alternate domain name' and enter your desired domain name.
7. Select Custom SSL certificate and select your SSL certificate that was issued.
   ![WhatsApp Image 2024-06-27 at 12 15 39 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/c3f6326e-7e8d-47d4-a247-7f16d0ddeb08)

9. Set the default root object to `index.html`.
10. Click "Create Distribution".
    ![WhatsApp Image 2024-06-27 at 12 17 28 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/d452a4a6-6f80-4e96-8515-860e25187932)



## 5. Update Route 53 to Direct Traffic to CloudFront

1. **Create an Alias Record in Route 53**:
   - Go back to the Route 53 dashboard and navigate to your hosted zone.
   - Click on "Create Record."
   - **Record Name**: Leave it blank to map the root domain
   - **Record Type**: Select "A – IPv4 address."
   - **Alias**: Turn on the "Alias" toggle.
   -  On the "route traffic to" choose, Select Alias to CloudFront distribution and select the distribution we created earlier.
   - Select "Simple Routing" on the routing policy"
   - Click "Create Records."
     ![WhatsApp Image 2024-06-27 at 12 58 49 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/b5464ab3-eeb7-4e45-befc-3e355aa0dd5e)


2. **Wait for DNS Propagation**:
   - DNS changes may take some time to propagate. Typically, this can take a few minutes.
     ![WhatsApp Image 2024-06-27 at 1 03 02 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/a5e3a755-d4bc-4952-8650-1ea8082d5667)

## 6. Editing S3 Bucket Policy for CloudFront to Access S3 bucket
We need to enable CloudFront to access our S3 bucket and serve content. We therefore need to edit the S3 bucket policy to allow access from the CloudFront origin.

1. **Select Your Distribution**:
   - Click on the ID of the distribution you want to configure and Copy the ID name.
     ![WhatsApp Image 2024-06-27 at 1 11 28 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/97422375-3b3c-4e5c-9f01-f8a91329e1a3)
   - Copy ARN name of the Distribution ID into a notepad

2: Update Your S3 Bucket Policy
- Navigate to S3 
- Click on the name of the bucket you are using as the origin for your CloudFront distribution.

3. **Open the Permissions Tab**:
   - Go to the "Permissions" tab.

4. **Edit the Bucket Policy**:
   - Under "Bucket policy," click on the "Edit" button.
   - Add a new policy that grants the CloudFront OAI access to the bucket. Replace `YOUR_OAI` with the actual OAI from your CloudFront distribution and `YOUR_BUCKET_NAME` with your bucket name.

Here is a sample bucket policy:

```json
{
    "Version": "2012-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Sid": "1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity YOUR_OAI"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
        }
    ]
}
```

### Step 3: Save the Policy

- After adding the policy, click "Save changes."

### Step 4: Verify the Configuration

1. **Test Access**:
   - Try to access your S3 content through the CloudFront distribution URL. You should be able to see the content being served from CloudFront.

2. **Check CloudFront**:
   - Ensure that your CloudFront distribution is working correctly and serving the files from your S3 bucket.

By following these steps, you grant CloudFront permission to access your S3 bucket using the specified Origin Access Identity (OAI). This ensures that your S3 content is only accessible through CloudFront and not directly from the S3 bucket URL, enhancing the security of your content.



Once the DNS changes have propagated, your domain should direct traffic to your CloudFront distribution, and visitors to your domain will be served content from CloudFront.


## Testing and Verification

1. Wait for the CloudFront distribution to deploy completely.
2. Access your resume website via the custom domain to ensure everything is working correctly.
3. Verify that the website is accessible over HTTPS.

## Conclusion

By following these steps, you have successfully deployed your resume on AWS using S3, CloudFront, Certificate Manager, and Route 53. This setup provides a scalable, secure, and globally distributed platform for showcasing your resume.

---

Feel free to modify and expand upon this template based on your specific project details and personal preferences!
