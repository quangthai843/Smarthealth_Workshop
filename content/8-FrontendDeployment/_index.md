---
title: "Frontend Deployment (React/Vite)"
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

With your backend API running securely on EC2 behind a Load Balancer, it is time to deploy the React frontend. Because Vite bundles your React application into static HTML, CSS, and JavaScript files, you do not need a traditional web server like Node.js or Nginx to serve it. Instead, we will host the files natively on Amazon S3 and distribute them globally using Amazon CloudFront.