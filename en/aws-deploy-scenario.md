# ☁️ Bonus: Deploy a Website to AWS with GitHub Actions

> 📖 [日本語版](../ja/aws-deploy-scenario.md)

This guide is a **bonus scenario** for the workshop series.  
It walks you through deploying a static website or web app to **Amazon Web Services (AWS)** using GitHub Actions in a hands-on format.

---

## 📋 Table of Contents

1. [Scenario Overview](#1-scenario-overview)
2. [Prerequisites](#2-prerequisites)
3. [Scenario A: Deploy a Static Site to S3 + CloudFront](#3-scenario-a-deploy-a-static-site-to-s3--cloudfront)
4. [Scenario B: Deploy a Web App to Elastic Beanstalk](#4-scenario-b-deploy-a-web-app-to-elastic-beanstalk)
5. [Managing Secrets](#5-managing-secrets)
6. [Troubleshooting](#6-troubleshooting)
7. [Cleanup (Delete Resources)](#7-cleanup-delete-resources)

---

## 1. Scenario Overview

### What You'll Learn

| Topic | Description |
|-------|-------------|
| **CI/CD Pipeline** | Automate build → test → deploy with GitHub Actions |
| **AWS Integration** | How GitHub and AWS services connect (OIDC / Access Keys) |
| **Secret Management** | Handling AWS credentials securely |
| **Static Sites / Web Apps** | Two deployment targets and when to use each |

### Deployment Target Comparison

| Aspect | S3 + CloudFront | Elastic Beanstalk |
|--------|-----------------|-------------------|
| **Use Case** | Static sites (HTML/CSS/JS), SPAs | Server-side apps (Node.js, Python, etc.) |
| **Pricing** | Free tier available (S3: 5GB, CloudFront: 1TB/mo) | Free tier available (t2.micro 750 hrs/mo) |
| **Setup** | Create S3 bucket + configure CloudFront | EB environment auto-provisions infrastructure |
| **Custom Domain** | ✅ Route 53 / CloudFront | ✅ Route 53 / CNAME |
| **SSL** | ✅ ACM (AWS Certificate Manager), free | ✅ ACM supported |
| **Best For** | Portfolios, documentation sites | API servers, full-stack apps |

---

## 2. Prerequisites

### Required Accounts and Tools

| Item | Description |
|------|-------------|
| **GitHub Account** | For repositories and Actions |
| **AWS Account** | Create a free account at [aws.amazon.com](https://aws.amazon.com/free/) |
| **AWS CLI** (optional) | [Installation guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) |

> 💡 The AWS Free Tier includes 12 months of free services and always-free tier offerings.

### Prepare a Sample Website

Create the following file in the root of your repository.

**`index.html`**:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello AWS</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 40px 20px;
            background-color: #f5f5f5;
            color: #232f3e;
        }
        h1 { color: #ff9900; }
        .badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 12px;
            background-color: #ff9900;
            color: white;
            font-size: 14px;
        }
        .info {
            background: white;
            border-radius: 8px;
            padding: 20px;
            margin-top: 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <h1>🚀 Hello AWS!</h1>
    <p>Deployed with <span class="badge">GitHub Actions</span></p>
    <div class="info">
        <h2>About This Site</h2>
        <p>This website was automatically deployed to AWS using GitHub Actions.</p>
        <ul>
            <li>✅ Automated build & deploy</li>
            <li>✅ Auto-updates on push to main branch</li>
            <li>✅ Global delivery via CloudFront CDN</li>
        </ul>
    </div>
</body>
</html>
```

---

## 3. Scenario A: Deploy a Static Site to S3 + CloudFront

Hosting static files on Amazon S3 and serving them via CloudFront (CDN) is the **most common static site architecture** on AWS.

### Step 1: Create an S3 Bucket

1. Sign in to the [AWS Management Console](https://console.aws.amazon.com)
2. Search for "**S3**" → click "**Create bucket**"
3. Configure the following:

| Setting | Value |
|---------|-------|
| **Bucket name** | `hello-github-site-<your-id>` (must be globally unique) |
| **Region** | ap-northeast-1 (Tokyo) or your nearest region |
| **Block Public Access** | **Uncheck all** (required for static hosting) |

4. Click "**Create bucket**"

### Step 2: Enable Static Website Hosting

1. Open the bucket → "**Properties**" tab
2. Scroll to "**Static website hosting**" → "**Edit**"
3. Configure:

| Setting | Value |
|---------|-------|
| **Static website hosting** | Enable |
| **Index document** | `index.html` |
| **Error document** | `index.html` |

4. Click "**Save changes**"

### Step 3: Set the Bucket Policy

1. Go to the "**Permissions**" tab → "**Bucket policy**" → "**Edit**"
2. Paste the following policy (replace `<YOUR-BUCKET-NAME>`):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::<YOUR-BUCKET-NAME>/*"
        }
    ]
}
```

### Step 4: Create an IAM User (for GitHub Actions)

1. Go to "**IAM**" → "**Users**" → "**Create user**"
2. User name: `github-actions-deploy`
3. Select "**Attach policies directly**" → create and attach the following policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::<YOUR-BUCKET-NAME>",
                "arn:aws:s3:::<YOUR-BUCKET-NAME>/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudfront:CreateInvalidation"
            ],
            "Resource": "*"
        }
    ]
}
```

4. Go to the "**Security credentials**" tab → "**Create access key**"
5. Save the **Access Key ID** and **Secret Access Key**

### Step 5: Add GitHub Secrets

In your GitHub repository: **Settings** → **Secrets and variables** → **Actions**, add:

| Secret Name | Value |
|-------------|-------|
| `AWS_ACCESS_KEY_ID` | IAM user access key ID |
| `AWS_SECRET_ACCESS_KEY` | IAM user secret access key |

### Step 6: Create the Workflow

Create `.github/workflows/deploy-s3.yml`:

```yaml
name: Deploy to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-1

      - name: Sync files to S3
        run: |
          aws s3 sync . s3://<YOUR-BUCKET-NAME> \
            --exclude ".git/*" \
            --exclude ".github/*" \
            --exclude "README.md" \
            --delete
```

> 💡 Replace `<YOUR-BUCKET-NAME>` with the bucket name from Step 1.

### Step 7: Verify the Deployment

```bash
git add .
git commit -m "Add S3 deployment workflow"
git push origin main
```

1. Check the workflow run in the **Actions** tab
2. Visit the S3 static website hosting **endpoint URL**
   - Example: `http://<YOUR-BUCKET-NAME>.s3-website-ap-northeast-1.amazonaws.com`

### (Optional) Add CloudFront

To enable CDN delivery + HTTPS, create a CloudFront distribution:

1. Go to "**CloudFront**" → "**Create distribution**"
2. **Origin domain**: enter the S3 static website endpoint
3. **Default root object**: `index.html`
4. Click "**Create distribution**"

Add CloudFront cache invalidation to the workflow:

```yaml
      - name: Invalidate CloudFront cache
        run: |
          aws cloudfront create-invalidation \
            --distribution-id ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID }} \
            --paths "/*"
```

---

## 4. Scenario B: Deploy a Web App to Elastic Beanstalk

Use Elastic Beanstalk for server-side applications (Node.js, Python, etc.).

### Step 1: Create a Sample Node.js App

**`package.json`**:

```json
{
  "name": "hello-aws",
  "version": "1.0.0",
  "scripts": {
    "start": "node server.js"
  }
}
```

**`server.js`**:

```javascript
const http = require('http');
const port = process.env.PORT || 8080;

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
    res.end(`
        <html>
        <head><title>Hello AWS Elastic Beanstalk</title></head>
        <body style="font-family: sans-serif; max-width: 800px; margin: 40px auto; padding: 20px;">
            <h1>🚀 Hello AWS Elastic Beanstalk!</h1>
            <p>A Node.js app deployed with GitHub Actions.</p>
            <p>Deployed at: ${new Date().toISOString()}</p>
        </body>
        </html>
    `);
});

server.listen(port, () => {
    console.log(`Server running on port ${port}`);
});
```

### Step 2: Create an Elastic Beanstalk Environment

1. Sign in to the [AWS Management Console](https://console.aws.amazon.com)
2. Search for "**Elastic Beanstalk**" → "**Create environment**"
3. Configure:

| Setting | Value |
|---------|-------|
| **Environment tier** | Web server environment |
| **Application name** | `hello-github` |
| **Environment name** | `hello-github-env` |
| **Platform** | Node.js |
| **Platform version** | Recommended version |
| **Application code** | Sample application |
| **Presets** | Single instance (free tier eligible) |

4. Click "**Next**" → proceed with defaults → "**Submit**"

> 💡 Environment creation takes a few minutes.

### Step 3: Add IAM Policy

Add the following policy to the IAM user created in Scenario A Step 4:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "elasticbeanstalk:*",
                "s3:PutObject",
                "s3:GetObject",
                "s3:ListBucket",
                "autoscaling:*",
                "cloudformation:*",
                "ec2:*",
                "elasticloadbalancing:*",
                "logs:*"
            ],
            "Resource": "*"
        }
    ]
}
```

> ⚠️ In production, use a least-privilege policy scoped to specific resources. Broad permissions are used here for learning purposes.

### Step 4: Create the Workflow

Create `.github/workflows/deploy-eb.yml`:

```yaml
name: Deploy to Elastic Beanstalk

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Create deployment package
        run: zip -r deploy.zip . -x ".git/*" ".github/*"

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-1

      - name: Upload to S3
        run: |
          aws s3 cp deploy.zip s3://elasticbeanstalk-ap-northeast-1-${{ secrets.AWS_ACCOUNT_ID }}/hello-github/deploy-${{ github.sha }}.zip

      - name: Create new EB application version
        run: |
          aws elasticbeanstalk create-application-version \
            --application-name hello-github \
            --version-label "ver-${{ github.sha }}" \
            --source-bundle S3Bucket="elasticbeanstalk-ap-northeast-1-${{ secrets.AWS_ACCOUNT_ID }}",S3Key="hello-github/deploy-${{ github.sha }}.zip"

      - name: Deploy to EB environment
        run: |
          aws elasticbeanstalk update-environment \
            --application-name hello-github \
            --environment-name hello-github-env \
            --version-label "ver-${{ github.sha }}"

      - name: Wait for deployment
        run: |
          aws elasticbeanstalk wait environment-updated \
            --application-name hello-github \
            --environment-names hello-github-env
```

### Step 5: Add Additional Secrets

| Secret Name | Value |
|-------------|-------|
| `AWS_ACCOUNT_ID` | Your 12-digit AWS account ID |

### Step 6: Verify the Deployment

```bash
git add .
git commit -m "Add Elastic Beanstalk deployment workflow"
git push origin main
```

1. Confirm the workflow succeeds in the **Actions** tab
2. Click the environment URL in the Elastic Beanstalk console
   - Example: `http://hello-github-env.ap-northeast-1.elasticbeanstalk.com`

---

## 5. Managing Secrets

### Secrets to Register in GitHub

| Secret Name | Purpose | Scenario |
|-------------|---------|----------|
| `AWS_ACCESS_KEY_ID` | AWS authentication (access key) | A, B |
| `AWS_SECRET_ACCESS_KEY` | AWS authentication (secret key) | A, B |
| `AWS_ACCOUNT_ID` | AWS account ID | B |
| `CLOUDFRONT_DISTRIBUTION_ID` | CloudFront cache invalidation | A (optional) |

### Best Practices

| Rule | Description |
|------|-------------|
| ✅ Dedicated IAM user | Create a separate IAM user for GitHub Actions |
| ✅ Least privilege | Allow only the minimum required actions and resources |
| ✅ Rotate access keys | Recommended every 90 days |
| ❌ Don't use root account keys | Always create and use IAM users |
| ❌ No hardcoding | Never put credentials directly in workflows or code |

### OIDC Authentication (Advanced / Recommended)

For better security, use **OpenID Connect (OIDC)** instead of access keys.  
This eliminates the need to manage long-lived credentials.

#### 1. Create an Identity Provider in AWS

IAM → Identity providers → Add provider:

| Setting | Value |
|---------|-------|
| **Provider type** | OpenID Connect |
| **Provider URL** | `https://token.actions.githubusercontent.com` |
| **Audience** | `sts.amazonaws.com` |

#### 2. Create an IAM Role

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::<ACCOUNT-ID>:oidc-provider/token.actions.githubusercontent.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
                },
                "StringLike": {
                    "token.actions.githubusercontent.com:sub": "repo:<OWNER>/<REPO>:*"
                }
            }
        }
    ]
}
```

#### 3. Use in Your Workflow

```yaml
    permissions:
      id-token: write
      contents: read

    steps:
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::<ACCOUNT-ID>:role/github-actions-role
          aws-region: ap-northeast-1
```

---

## 6. Troubleshooting

### Common Errors and Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `Access Denied` | Insufficient IAM permissions | Check the IAM policy includes all required actions and resources |
| `NoSuchBucket` | Wrong S3 bucket name | Verify the bucket name in the workflow |
| `InvalidParameterValue` (EB) | Application name mismatch | Confirm EB application and environment names |
| `ExpiredToken` | Credentials have expired | Recreate access keys or migrate to OIDC |
| `403 Forbidden` (viewing site) | Missing bucket policy | Check the bucket policy and public access settings |

### Debugging Tips

1. **Check Actions tab logs** — detailed output for each step
2. **AWS CloudWatch Logs** — view Elastic Beanstalk application logs
3. **EB Health Dashboard** — check environment health status and events
4. **Add a debug step to your workflow**:

```yaml
- name: Debug - Check AWS identity
  run: aws sts get-caller-identity

- name: Debug - List S3 buckets
  run: aws s3 ls
```

---

## 7. Cleanup (Delete Resources)

After completing the hands-on, **make sure to delete resources** to avoid unexpected charges.

### Scenario A Cleanup

```bash
# Empty and delete the S3 bucket
aws s3 rm s3://<YOUR-BUCKET-NAME> --recursive
aws s3 rb s3://<YOUR-BUCKET-NAME>

# Disable and delete CloudFront distribution (if created)
# Easiest to do from the AWS Console
```

### Scenario B Cleanup

```bash
# Terminate the Elastic Beanstalk environment
aws elasticbeanstalk terminate-environment \
  --environment-name hello-github-env

# Delete the application
aws elasticbeanstalk delete-application \
  --application-name hello-github \
  --terminate-env-by-force
```

### Delete from AWS Console

1. **Elastic Beanstalk** → Application → "**Actions**" → "**Delete application**"
2. **S3** → select bucket → "**Empty**" → "**Delete**"
3. **CloudFront** → distribution → "**Disable**" → "**Delete**"

### GitHub-side Cleanup

- Remove unused secrets from repository **Settings** → **Secrets**
- Optionally delete workflow files that are no longer needed

### IAM Cleanup

- **IAM** → **Users** → `github-actions-deploy` → "**Delete**"
- If using OIDC: also delete the **Identity provider** and **Role**

> ⚠️ Deleting an Elastic Beanstalk environment also removes associated EC2 instances and load balancers.

---

## 📚 Reference Links

- [Amazon S3 Static Website Hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- [Amazon CloudFront Documentation](https://docs.aws.amazon.com/cloudfront/)
- [AWS Elastic Beanstalk Documentation](https://docs.aws.amazon.com/elasticbeanstalk/)
- [GitHub Actions for AWS](https://github.com/aws-actions)
- [aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials)
- [Configuring OIDC in AWS](https://docs.github.com/en/actions/security-guides/configuring-openid-connect-in-amazon-web-services)
- [Managing GitHub Actions secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)
