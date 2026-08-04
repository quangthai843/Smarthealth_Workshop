---
title: "CloudFront CDN Integration"
weight: 4
chapter: false
pre: " <b> 8.4 </b> "
---

To ensure the application loads lightning-fast for users worldwide and is served over HTTPS, we will distribute the S3 bucket through Amazon CloudFront.

### Step 1: Create the Distribution

1. Navigate to the CloudFront Console and click **Create Distribution**.
2. Origin domain: Select your new S3 frontend bucket from the dropdown.
3. Viewer protocol policy: Select **Redirect HTTP to HTTPS**.
4. Web Application Firewall (WAF): To keep workshop costs down, select **Do not enable security protections** (you can enable this later for enterprise deployments).
5. Click **Create Distribution**.

### Step 2: The React Router SPA Fix

Because React Router handles navigation entirely within the browser, CloudFront gets confused if a user directly visits or refreshes a deep link like `yourdomain.com/appointments`. It looks for a literal folder named "appointments" in your S3 bucket, fails to find it, and returns a 404 error.

We must force CloudFront to route all traffic back to React:

1. In your CloudFront Distribution, go to the **Error Pages** tab.
2. Click **Create custom error response**.
3. HTTP error code: Select **404: Not Found**.
4. Customize error response: Select **Yes**.
5. Response page path: Type `/index.html`.
6. HTTP Response code: Select **200: OK**.
7. Click **Create**.

![CloudFront Custom Error Response configuration](/images/frontend/cloudfront-error-response.png)

Once your CloudFront distribution finishes deploying, copy the **Distribution domain name** (e.g., `d12345abcdef.cloudfront.net`). Paste this URL into your browser, and you will see your fully deployed Smart Healthcare interface communicating live with your EC2 backend!