# Resume Deployment on AWS

This project demonstrates the deployment of a personal resume on AWS using the following services: Amazon S3, CloudFront, Certificate Manager, and Route 53. This setup ensures a highly available, secure, and fast-loading resume website.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Setup Instructions](#setup-instructions)
  - [1. Create an S3 Bucket](#1-create-an-s3-bucket)
    - [Upload Resume to S3](#upload-resume-to-s3)
    - [Configure S3 Bucket for Static Website Hosting](#configure-s3-bucket-for-static-website-hosting)
  - [2. Obtain SSL Certificate with Certificate Manager](#2-obtain-ssl-certificate-with-certificate-manager)
  - [3. Configure Route 53 for Custom Domain](#3-configure-route-53-for-custom-domain)
    - [Update Your Domain's Nameservers](#update-your-domains-nameservers)
  - [4. Set Up CloudFront Distribution](#4-set-up-cloudfront-distribution)
  - [5. Update Route 53 to Direct Traffic to CloudFront](#5-update-route-53-to-direct-traffic-to-cloudfront)
  - [6. Editing S3 Bucket Policy for CloudFront to Access S3 Bucket](#6-editing-s3-bucket-policy-for-cloudfront-to-access-s3-bucket)
- [Testing and Verification](#testing-and-verification)
- [Link to Resume](#link-to-resume)


## Prerequisites

- An AWS account
- A registered domain name
- Basic understanding of AWS services (S3, CloudFront, Certificate Manager, Route 53)

## Architecture

![webimage](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/20a68cf9-c47b-435a-a90e-ec81aaf9dd2e)

When a user writes your domain name into their browser, Route 53 translates the domain name into the correct IP address and directs the request to the Cloudfront distribution. Cloudfront then quickly retrieves the resume files from the nearest edge location ensuring fast delivery. These files are stored in Amazon s3 securely and are secured over a secure HTTPS connection managed by certificate manager.

## Setup Instructions

## 1. Create an S3 Bucket

1. Go to the S3 service in the AWS Management Console.
2. Click "Create bucket".
3. Enter a unique bucket name and select the appropriate region.
4. Click "Create bucket".

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
3. In the "Origin Settings", select the S3 bucket domain.
   ![WhatsApp Image 2024-06-27 at 11 56 18 AM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/7e1436f7-6657-4880-a1b9-f27fb3b1aba9)
4. On the origin access, choose "Origin access control settings (recommended)" and create new OAC
5. Click on Create
6. On the 'Viewer' select 'Redirect HTTP to HTTPS'
![WhatsApp Image 2024-06-27 at 12 11 06 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/9d03f55c-cd92-4c54-b909-a7a27641d2c7)
7. On the 'Web Application Firewall' select 'Do not enable security protections'.
8. Choose 'Alternate domain name' and enter your desired domain name.
9. Select Custom SSL certificate and select your SSL certificate that was issued.
   ![WhatsApp Image 2024-06-27 at 12 15 39 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/c3f6326e-7e8d-47d4-a247-7f16d0ddeb08)

10. Set the default root object to `index.html`.
11. Click "Create Distribution".
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

2. **Update Your S3 Bucket Policy**:
- Navigate to S3 
- Click on the name of the bucket you are using as the origin for your CloudFront distribution.
- Go to the "Permissions" tab.
- Under "Bucket policy," click on the "Edit" button.
- Add a new policy that grants the CloudFront OAI access to the bucket. Replace `AWS:SourceArn` with the actual ARN from your CloudFront distribution and `S3 bucket name` with your bucket name.

Here is a sample bucket policy:

```json
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "AllowCloudFrontServicePrincipalReadOnly",
        "Effect": "Allow",
        "Principal": {
            "Service": "cloudfront.amazonaws.com"
        },
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::<S3 bucket name>/*",
        "Condition": {
            "StringEquals": {
                "AWS:SourceArn": "arn:aws:cloudfront::111122223333:distribution/<CloudFront distribution ID>"
            }
        }
    }
}
```
- After adding the policy, click "Save changes."


## Testing and Verification
- Access your resume website via the custom domain to ensure everything is working correctly.
![WhatsApp Image 2024-06-27 at 2 59 58 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/5ad00816-047e-4f6d-93c2-ef3d5715e1b1)


3. Verify that the website is accessible over HTTPS.
![WhatsApp Image 2024-06-27 at 2 25 59 PM](https://github.com/trintambogo/aws-cloud-resume/assets/87088123/f9224744-0a1f-468a-95ee-dd39cf272733)

## Link to resume
[cloudresume: ](trintacloud.buzz)


