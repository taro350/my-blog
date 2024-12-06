---
title: Securing Your Content with Amazon CloudFront Signed URLs
description: Signed URL is a feature that allows you to securely control access to your private content hosted on CloudFront. Ideal for scenarios requiring restricted access, such as subscription-based or time-sensitive content delivery, signed URLs ensure that only authorized users can view or download resources. The post guides you through setting up CloudFront, generating key pairs, and creating signed URLs with Python, providing detailed, step-by-step instructions to protect your digital assets effectively.
date: 2020-06-10
tags:
  - AWS
  - CloudFront
  - Networking
---

# Understanding Amazon CloudFront Signed URLs

Amazon CloudFront is a fast content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally, with low latency and high transfer speeds. One of the powerful features of CloudFront is its ability (...) more control over your content distribution.

In this blog post, we will explore the concept of CloudFront signed URLs and provide detailed steps to implement them for securing your content.

## What is a CloudFront Signed URL?

A CloudFront signed URL allows you to serve private content by providing a way to control who can access it and for how long. This URL permits access to your private content stored in Amazon S3 buckets or other origins for a specified time and by specific users. These signed URLs are often used to protect media streaming, software downloads, and API calls.

## When to Use CloudFront Signed URLs

- **Restricting Access to Paid Content**: Perfect for use cases where you want to restrict access to content that requires a subscription or one-time payment.
- **Time-Limited Access**: Provide temporary access to content, such as time-sensitive promotions or temporary user permissions.
- **User-Specific Access**: Control access to content on a per-user basis, ensuring that only authorized individuals can view or download specific resources.

## Steps to Create CloudFront Signed URLs

### Step 1: Set Up a CloudFront Distribution

1. **Log into AWS Management Console** and navigate to the CloudFront service.
2. **Create a New Distribution**:
   - Choose the Web option for web distribution.
   - Configure your origin (such as an Amazon S3 bucket containing the content you want to restrict).
   - Adjust caching behaviors and settings as required for your distribution needs.

### Step 2: Generate a CloudFront Key Pair

1. **Navigate to Security Credentials** under your AWS account.
2. **Create a New Key Pair**:
   - Important: Save the private key securely, as this private key is used to sign your URLs.

### Step 3: Prepare Your Application to Generate Signed URLs

To generate a signed URL, you need to have access to your private key from the key pair and a tool or library to help create the URL.

Here's a basic example using Python and the `boto3` library:

```python
import boto3
import datetime

cloudfront = boto3.client('cloudfront')

def generate_signed_url(url, expire_time_hours):
    cloudfront_signer = cloudfront.signer.Signer(
        key_id='Your-Access-Key-ID',  # Replace with your Access Key ID
        private_key='Your-Private-Key'  # Replace with the path or variable holding your private key
    )

    expire_time = datetime.datetime.utcnow() + datetime.timedelta(hours=expire_time_hours)
    signed_url = cloudfront_signer.generate_presigned_url(
        url,
        date_less_than=expire_time
    )

    return signed_url

# Example usage
url = 'url_here'
signed_url = generate_signed_url(url, 2)  # Valid for 2 hours
print(signed_url)
```

### Step 4: Use Signed URLs in Your Application

- Use the generated signed URLs in your website or application to access restricted content.
- Ensure that only these signed URLs are used to access secure resources, preventing unauthorized access.

### Step 5: Test Your Setup

- Attempt to access your content using the signed URL to verify setup correctness.
- Check access time limits and permissions are enforced accurately.

## Conclusion

By implementing CloudFront signed URLs, you can effectively control and protect your digital assets in the AWS cloud. This setup is highly beneficial for businesses looking to monetize content or secure sensitive data without compromising delivery performance or cost-efficiency.

Whether you're hosting media files, serving software downloads, or restricting API access, CloudFront signed URLs provide a powerful framework to secure your content while harnessing the robust capabilities of Amazon's global CDN infrastructure.
