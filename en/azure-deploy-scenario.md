# ☁️ Bonus: Deploy a Website to Azure with GitHub Actions

> 📖 [日本語版](../ja/azure-deploy-scenario.md)

This guide is a **bonus scenario** for the workshop series.  
It walks you through deploying a static website or web app to **Microsoft Azure** using GitHub Actions in a hands-on format.

---

## 📋 Table of Contents

1. [Scenario Overview](#1-scenario-overview)
2. [Prerequisites](#2-prerequisites)
3. [Scenario A: Deploy to Azure Static Web Apps](#3-scenario-a-deploy-to-azure-static-web-apps)
4. [Scenario B: Deploy to Azure App Service](#4-scenario-b-deploy-to-azure-app-service)
5. [Managing Secrets](#5-managing-secrets)
6. [Troubleshooting](#6-troubleshooting)
7. [Cleanup (Delete Resources)](#7-cleanup-delete-resources)

---

## 1. Scenario Overview

### What You'll Learn

| Topic | Description |
|-------|-------------|
| **CI/CD Pipeline** | Automate build → test → deploy with GitHub Actions |
| **Azure Integration** | How GitHub and Azure services connect |
| **Secret Management** | Handling sensitive information securely |
| **Static Sites / Web Apps** | Two deployment targets and when to use each |

### Deployment Target Comparison

| Aspect | Azure Static Web Apps | Azure App Service |
|--------|----------------------|-------------------|
| **Use Case** | Static sites (HTML/CSS/JS), SPAs | Server-side apps (Node.js, Python, etc.) |
| **Pricing** | Free plan available | Free plan (F1) available |
| **Setup** | Very easy (built-in GitHub integration) | More steps required |
| **Custom Domain** | ✅ Supported | ✅ Supported |
| **SSL** | ✅ Automatic (free) | ✅ Supported |
| **Best For** | Portfolios, documentation sites | API servers, full-stack apps |

---

## 2. Prerequisites

### Required Accounts and Tools

| Item | Description |
|------|-------------|
| **GitHub Account** | For repositories and Actions |
| **Azure Account** | Create a free account at [azure.microsoft.com](https://azure.microsoft.com/en-us/free/) |
| **Azure CLI** (optional) | [Installation guide](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) |

> 💡 An Azure free account comes with 12 months of free services and $200 in credit.

### Prepare a Sample Website

Create the following file in the root of your repository.

**`index.html`**:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello Azure</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 40px 20px;
            background-color: #f0f6ff;
            color: #24292f;
        }
        h1 { color: #0078d4; }
        .badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 12px;
            background-color: #0078d4;
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
    <h1>🚀 Hello Azure!</h1>
    <p>Deployed with <span class="badge">GitHub Actions</span></p>
    <div class="info">
        <h2>About This Site</h2>
        <p>This website was automatically deployed to Azure using GitHub Actions.</p>
        <ul>
            <li>✅ Automated build & deploy</li>
            <li>✅ Auto-updates on push to main branch</li>
            <li>✅ Preview environments for Pull Requests</li>
        </ul>
    </div>
</body>
</html>
```

---

## 3. Scenario A: Deploy to Azure Static Web Apps

Static Web Apps is the **easiest way** to deploy static sites.  
Simply connect your GitHub repository from the Azure portal, and a workflow is automatically generated.

### Step 1: Create a Static Web App in Azure Portal

1. Sign in to the [Azure portal](https://portal.azure.com)
2. Click "**Create a resource**" → search for "**Static Web App**" → click "**Create**"
3. Configure the following:

| Setting | Value |
|---------|-------|
| **Subscription** | Your subscription |
| **Resource Group** | `rg-hello-github` (create new) |
| **Name** | `swa-hello-github` (your choice) |
| **Plan type** | Free |
| **Region** | East Asia or your nearest region |
| **Source** | GitHub |

4. Click "**Sign in with GitHub**" → select your repository
5. Build configuration:

| Setting | Value |
|---------|-------|
| **Build Presets** | Custom |
| **App location** | `/` |
| **API location** | (leave empty) |
| **Output location** | (leave empty) |

6. Click "**Review + create**" → "**Create**"

### Step 2: Review the Auto-Generated Workflow

Azure automatically creates `.github/workflows/azure-static-web-apps-xxxx.yml` in your repository.

```yaml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main

jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
          lfs: false

      - name: Build And Deploy
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: "upload"
          app_location: "/"
          api_location: ""
          output_location: ""

  close_pull_request_job:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request Job
    steps:
      - name: Close Pull Request
        id: closepullrequest
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: "close"
```

> 💡 `AZURE_STATIC_WEB_APPS_API_TOKEN` is automatically registered in your repository's Secrets by Azure.

### Step 3: Verify the Deployment

1. Check the workflow run in the **Actions** tab
2. Open the Static Web App in the Azure portal and click the **URL**
3. Confirm the site is live at `https://xxxx.azurestaticapps.net`

### Step 4: Make a Change

```bash
# Create a branch
git checkout -b feature/update-site

# Edit index.html (e.g., change the title)
# Commit & push
git add index.html
git commit -m "Update site title"
git push origin feature/update-site
```

When you create a Pull Request, a **preview environment** is automatically created!  
A preview URL will be posted as a comment on the PR — check it out.

---

## 4. Scenario B: Deploy to Azure App Service

Use App Service for server-side applications (Node.js, Python, etc.).

### Step 1: Create a Sample Node.js App

**`package.json`**:

```json
{
  "name": "hello-azure",
  "version": "1.0.0",
  "scripts": {
    "start": "node server.js"
  }
}
```

**`server.js`**:

```javascript
const http = require('http');
const port = process.env.PORT || 3000;

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
    res.end(`
        <html>
        <head><title>Hello Azure App Service</title></head>
        <body style="font-family: sans-serif; max-width: 800px; margin: 40px auto; padding: 20px;">
            <h1>🚀 Hello Azure App Service!</h1>
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

### Step 2: Create an App Service in Azure Portal

1. Sign in to the [Azure portal](https://portal.azure.com)
2. Click "**Create a resource**" → "**Web App**" → "**Create**"
3. Configure the following:

| Setting | Value |
|---------|-------|
| **Subscription** | Your subscription |
| **Resource Group** | `rg-hello-github` |
| **Name** | `app-hello-github` (must be globally unique) |
| **Publish** | Code |
| **Runtime stack** | Node 20 LTS |
| **OS** | Linux |
| **Region** | East Asia |
| **App Service Plan** | Free (F1) |

4. Click "**Review + create**" → "**Create**"

### Step 3: Get the Publish Profile and Add It as a Secret

1. Open the App Service in the Azure portal
2. Click "**Overview**" → "**Get publish profile**" (a `.publishsettings` file will download)
3. In your GitHub repository: **Settings** → **Secrets and variables** → **Actions**
4. Click "**New repository secret**":
   - **Name**: `AZURE_WEBAPP_PUBLISH_PROFILE`
   - **Secret**: paste the entire contents of the downloaded file

### Step 4: Create the Workflow

Create `.github/workflows/azure-webapp.yml`:

```yaml
name: Deploy to Azure App Service

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Deploy to Azure Web App
        uses: azure/webapps-deploy@v3
        with:
          app-name: 'app-hello-github'
          publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
          package: .
```

### Step 5: Verify the Deployment

```bash
git add .
git commit -m "Add Azure App Service deployment workflow"
git push origin main
```

1. Confirm the workflow succeeds in the **Actions** tab
2. Visit `https://app-hello-github.azurewebsites.net` to see the site

---

## 5. Managing Secrets

### How GitHub Secrets Work

| Aspect | Description |
|--------|-------------|
| **Location** | Repository Settings → Secrets and variables → Actions |
| **Encryption** | Encrypted at rest, never shown in logs |
| **Usage** | Reference in workflows with `${{ secrets.SECRET_NAME }}` |
| **Scope** | Repository-level, Organization-level, or Environment-level |

### Best Practices

| Rule | Description |
|------|-------------|
| ✅ Use Secrets | Always store connection strings and tokens in Secrets |
| ✅ Least privilege | Grant only the minimum permissions required |
| ✅ Rotate regularly | Change secret values periodically |
| ❌ No hardcoding | Never put secrets directly in workflows or code |
| ❌ No logging | Don't use `echo` or `print` to display secrets |

### Azure Service Principal (Advanced)

For better security, use a service principal (Azure AD app registration) instead of a publish profile.

```bash
# Create a service principal with Azure CLI
az ad sp create-for-rbac \
  --name "github-actions-deploy" \
  --role contributor \
  --scopes /subscriptions/{subscription-id}/resourceGroups/rg-hello-github \
  --sdk-auth
```

Register the output JSON as an `AZURE_CREDENTIALS` secret and use it with the `azure/login@v2` action.

---

## 6. Troubleshooting

### Common Errors and Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `Error: Publish profile is not valid` | Publish profile content is malformed | Re-download from Azure portal; copy entire content including line breaks |
| `Resource not found` | App Service name mismatch | Verify `app-name` in the workflow matches the Azure resource name |
| `Authentication failed` | Secret has expired | Re-download the publish profile or check the service principal expiry |
| `Build failed` | Dependency installation error | Check `package.json` and Node.js version |
| `404 Not Found` (after deploy) | Wrong app location path | Verify `app_location` or `package` path |

### Debugging Tips

1. **Check Actions tab logs** — detailed output is available for each step
2. **Azure portal "Log stream"** — view real-time App Service logs
3. **Add a debug step to your workflow**:

```yaml
- name: Debug - List files
  run: |
    echo "Current directory:"
    pwd
    echo "Files:"
    ls -la
```

---

## 7. Cleanup (Delete Resources)

After completing the hands-on, **make sure to delete resources** to avoid unexpected charges.

### Delete from Azure Portal

1. Go to [Azure portal](https://portal.azure.com) → "**Resource groups**"
2. Select `rg-hello-github`
3. Click "**Delete resource group**" → type the name to confirm → "**Delete**"

### Delete with Azure CLI

```bash
az group delete --name rg-hello-github --yes --no-wait
```

### GitHub-side Cleanup

- Remove unused secrets from repository **Settings** → **Secrets**
- For Static Web Apps: optionally delete the auto-generated workflow file

> ⚠️ Deleting a resource group removes all resources within it (Static Web App, App Service, etc.) at once.

---

## 📚 Reference Links

- [Azure Static Web Apps documentation](https://learn.microsoft.com/en-us/azure/static-web-apps/)
- [Azure App Service documentation](https://learn.microsoft.com/en-us/azure/app-service/)
- [GitHub Actions for Azure](https://github.com/Azure/actions)
- [Azure/static-web-apps-deploy action](https://github.com/Azure/static-web-apps-deploy)
- [Azure/webapps-deploy action](https://github.com/Azure/webapps-deploy)
- [Managing GitHub Actions secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)
