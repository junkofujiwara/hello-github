# Workshop 5: GitHub Actions DeepDive (60 min)

> 📖 [日本語版](../ja/05-github-actions.md)

## 🎯 Workshop Goals

- Understand the core concepts of GitHub Actions
- Read and write workflow syntax
- Build CI/CD pipelines
- Set up automated testing, building, and deployment

---

## 📋 Agenda

| Time | Content |
|------|---------|
| 0:00 - 0:05 | Review of Workshop 4 |
| 0:05 - 0:25 | GitHub Actions basics |
| 0:25 - 0:40 | Workflow practice |
| 0:40 - 0:55 | Advanced: CI/CD and custom Actions |
| 0:55 - 1:00 | Wrap Up |

---

## Review of Workshop 4 (5 min)

In Workshop 4, we learned project management with GitHub Projects.  
In this workshop, we'll learn **GitHub Actions** for automation and CI/CD.

---

## Part 1: GitHub Actions Basics (20 min)

### 1.1 What is GitHub Actions?

**GitHub Actions** is a system that automatically executes tasks triggered by events in your repository (Push, PR creation, etc.).  
For example, you can "auto-run tests when code is pushed" or "auto-deploy a site when a PR is merged."

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│  Event   │────▶│ Workflow │────▶│  Result  │
│ (Trigger)│     │ (Process)│     │ (Output) │
│          │     │          │     │          │
│ - Push   │     │ - Test   │     │ - ✅ Pass │
│ - PR     │     │ - Build  │     │ - ❌ Fail │
│ - Issue  │     │ - Deploy │     │ - 📊 Report│
│ - Schedule│    │ - Notify │     │          │
└──────────┘     └──────────┘     └──────────┘
```

### 1.2 Key Terminology

| Term | Description | Example |
|------|-------------|---------|
| **Workflow** | The entire automation process | `.github/workflows/ci.yml` |
| **Event** | Trigger that starts a workflow | `push`, `pull_request` |
| **Job** | Execution unit within a workflow | `build`, `test`, `deploy` |
| **Step** | Individual operation within a job | `actions/checkout@v4` |
| **Action** | Reusable step unit | `actions/setup-node@v4` |
| **Runner** | Environment where jobs run | `ubuntu-latest`, `windows-latest` |

### 1.3 Workflow File Location

Workflows are YAML files placed in the `.github/workflows/` directory.

```
hello-github/
├── .github/
│   └── workflows/
│       ├── ci.yml          # Test & Build
│       ├── deploy.yml      # Deploy
│       └── greeting.yml    # Greeting bot
├── src/
│   └── index.html
└── README.md
```

#### 🪟 Windows (PowerShell) — Create the directory

```powershell
New-Item -ItemType Directory -Path ".github\workflows" -Force
```

#### 🍎 Mac (Terminal) — Create the directory

```bash
mkdir -p .github/workflows
```

### 1.4 Your First Workflow

Here's the simplest possible workflow. Let's read through it with comments:

```yaml
# .github/workflows/hello.yml
name: Hello World                    # Workflow name

on:                                  # Trigger definition
  push:                              # Run on push
    branches: [ main ]               # main branch only

jobs:                                # Job definitions
  greeting:                          # Job name
    runs-on: ubuntu-latest           # Runner environment
    steps:                           # Step definitions
      - name: Say Hello              # Step name
        run: echo "Hello, GitHub Actions!"  # Command to run
```

### 1.5 Trigger Types (on)

| Trigger | Description | Example |
|---------|-------------|---------|
| `push` | When code is pushed | `on: push` |
| `pull_request` | When a PR is created/updated | `on: pull_request` |
| `issues` | When an Issue is created/updated | `on: issues` |
| `schedule` | Run on a schedule (cron) | `on: schedule` |
| `workflow_dispatch` | Manual execution | Button trigger |
| `release` | When a release is created | `on: release` |

#### Schedule Example

```yaml
on:
  schedule:
    - cron: '0 9 * * 1'   # Every Monday at 9:00 UTC
#          ┌─ minute (0-59)
#          │ ┌─ hour (0-23)
#          │ │ ┌─ day of month (1-31)
#          │ │ │ ┌─ month (1-12)
#          │ │ │ │ ┌─ day of week (0-6, 0=Sunday)
#          * * * * *
```

#### Manual Trigger Example

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Deployment environment'
        required: true
        default: 'staging'
        type: choice
        options:
          - staging
          - production
```

### 1.6 Runner Environments

| Runner | OS | Use Case |
|--------|-----|----------|
| `ubuntu-latest` | Ubuntu Linux | General CI/CD |
| `windows-latest` | Windows Server | Windows builds |
| `macos-latest` | macOS | iOS/macOS apps |

> 💡 **Public repositories** have unlimited free minutes. Private repositories have a monthly free allowance.

### ✅ Hands-on: Create Your First Workflow

#### 🪟 Windows (PowerShell)

```powershell
# Create directory
New-Item -ItemType Directory -Path ".github\workflows" -Force

# Create workflow file
@"
name: Hello World

on:
  push:
    branches: [ main ]

jobs:
  greeting:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello, GitHub Actions!"
      - name: Show Date
        run: date
      - name: Show Runner Info
        run: |
          echo "Runner OS: `$RUNNER_OS"
          echo "Runner Arch: `$RUNNER_ARCH"
"@ | Out-File -FilePath ".github\workflows\hello.yml" -Encoding utf8
```

#### 🍎 Mac (Terminal)

```bash
# Create directory
mkdir -p .github/workflows

# Create workflow file
cat << 'EOF' > .github/workflows/hello.yml
name: Hello World

on:
  push:
    branches: [ main ]

jobs:
  greeting:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello, GitHub Actions!"
      - name: Show Date
        run: date
      - name: Show Runner Info
        run: |
          echo "Runner OS: $RUNNER_OS"
          echo "Runner Arch: $RUNNER_ARCH"
EOF
```

#### Common Steps

```bash
git add .github/workflows/hello.yml
git commit -m "Add hello world workflow"
git push origin main
```

After pushing, check the **Actions** tab in your GitHub repository to see the workflow running.

---

## Part 2: Workflow Practice (15 min)

### 2.1 Practical CI Workflow

#### HTML/CSS Static Site Validation

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Check file existence
        run: |
          echo "📂 Checking project files..."
          ls -la
          if [ -f "index.html" ]; then
            echo "✅ index.html found"
          else
            echo "❌ index.html not found"
            exit 1
          fi

      - name: Validate HTML structure
        run: |
          echo "🔍 Checking HTML structure..."
          if grep -q "<html" index.html && grep -q "</html>" index.html; then
            echo "✅ HTML structure is valid"
          else
            echo "❌ HTML structure is invalid"
            exit 1
          fi

      - name: Check for broken links
        run: |
          echo "🔗 Checking for potential issues..."
          if grep -q "href=\"\"" index.html; then
            echo "⚠️ Warning: Empty href found"
          else
            echo "✅ No empty hrefs"
          fi
```

### 2.2 Multiple Jobs

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Lint check
        run: echo "Running lint..."

  test:
    runs-on: ubuntu-latest
    needs: lint                    # Runs after lint succeeds
    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        run: echo "Running tests..."

  deploy:
    runs-on: ubuntu-latest
    needs: [lint, test]            # Runs after both succeed
    if: github.ref == 'refs/heads/main'  # main branch only
    steps:
      - uses: actions/checkout@v4
      - name: Deploy
        run: echo "Deploying..."
```

```
┌────────┐     ┌────────┐     ┌────────┐
│  Lint  │────▶│  Test  │────▶│ Deploy │
└────────┘     └────────┘     └────────┘
    ✅              ✅             ✅
```

### 2.3 Environment Variables and Secrets

#### Environment Variables

You can define values as environment variables within a workflow. There are job-level variables (shared across all steps) and step-level variables.

```yaml
jobs:
  example:
    runs-on: ubuntu-latest
    env:
      APP_NAME: hello-github       # Job-level env var
    steps:
      - name: Use environment variable
        env:
          GREETING: "Hello"        # Step-level env var
        run: |
          echo "$GREETING from $APP_NAME"
          echo "Branch: ${{ github.ref_name }}"
          echo "Actor: ${{ github.actor }}"
```

#### Secrets

#### Secrets

Store sensitive information like API keys and passwords as "secrets" instead of writing them directly in code.

```yaml
steps:
  - name: Deploy with secret
    env:
      API_KEY: ${{ secrets.API_KEY }}
    run: |
      echo "Deploying with API key..."
      # Secrets are masked in logs
```

> ⚠️ Configure secrets in **Settings** → **Secrets and variables** → **Actions**.

### 2.4 Artifacts

Save files created by a workflow (such as test reports) for later download.

```yaml
steps:
  - name: Create report
    run: |
      echo "Test Report" > report.txt
      echo "All tests passed!" >> report.txt

  - name: Upload artifact
    uses: actions/upload-artifact@v4
    with:
      name: test-report
      path: report.txt
      retention-days: 30
```

### ✅ Hands-on: Create a CI Workflow

#### 🪟 Windows (PowerShell)

```powershell
@"
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Check project files
        run: |
          echo "Checking project structure..."
          ls -la
          echo "✅ Project structure check complete"

      - name: Validate HTML
        run: |
          if [ -f "index.html" ]; then
            echo "✅ index.html exists"
          else
            echo "ℹ️ No index.html found (not required)"
          fi
"@ | Out-File -FilePath ".github\workflows\ci.yml" -Encoding utf8
```

#### 🍎 Mac (Terminal)

```bash
cat << 'EOF' > .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Check project files
        run: |
          echo "Checking project structure..."
          ls -la
          echo "✅ Project structure check complete"

      - name: Validate HTML
        run: |
          if [ -f "index.html" ]; then
            echo "✅ index.html exists"
          else
            echo "ℹ️ No index.html found (not required)"
          fi
EOF
```

#### Common Steps

```bash
git add .github/workflows/ci.yml
git commit -m "Add CI workflow"
git push origin main
```

---

## Part 3: Advanced Topics (15 min)

### 3.1 Deploy to GitHub Pages

**GitHub Pages** lets you host HTML files from your repository as a free website.  
Combined with GitHub Actions, you can create a setup where "your site updates automatically every time you push."

```yaml
# .github/workflows/deploy-pages.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 3.2 Issue & PR Automation

#### Auto-label New Issues

```yaml
# .github/workflows/auto-label.yml
name: Auto Label Issues

on:
  issues:
    types: [opened]

jobs:
  label:
    runs-on: ubuntu-latest
    permissions:
      issues: write
    steps:
      - name: Add triage label
        uses: actions/github-script@v7
        with:
          script: |
            github.rest.issues.addLabels({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              labels: ['triage']
            })
```

#### Auto-assign Reviewers on PR

```yaml
# .github/workflows/auto-assign.yml
name: Auto Assign Reviewers

on:
  pull_request:
    types: [opened]

jobs:
  assign:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    steps:
      - name: Auto assign
        uses: actions/github-script@v7
        with:
          script: |
            const pr = context.payload.pull_request;
            console.log(`PR #${pr.number} created by ${pr.user.login}`);
```

### 3.3 Matrix Builds

Run tests across multiple OS × version combinations at once.  
For example, the configuration below runs 3 operating systems × 3 Node.js versions = 9 test patterns simultaneously.

```yaml
name: Matrix Test

on: push

jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [18, 20, 22]
    steps:
      - uses: actions/checkout@v4
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: node --version
      - run: npm --version
```

```
┌─────────────────────────────────────┐
│           Matrix (9 jobs)           │
├────────────┬────────────┬───────────┤
│ Ubuntu/18  │ Ubuntu/20  │ Ubuntu/22 │
│ Windows/18 │ Windows/20 │ Windows/22│
│ macOS/18   │ macOS/20   │ macOS/22  │
└────────────┴────────────┴───────────┘
```

### 3.4 Reusable Workflows

To avoid writing the same logic repeatedly, you can reuse workflows as "components."

```yaml
# .github/workflows/reusable-deploy.yml (reusable workflow)
name: Reusable Deploy

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to ${{ inputs.environment }}
        run: echo "Deploying to ${{ inputs.environment }}..."
```

```yaml
# .github/workflows/production-deploy.yml (caller)
name: Production Deploy

on:
  push:
    branches: [ main ]

jobs:
  call-deploy:
    uses: ./.github/workflows/reusable-deploy.yml
    with:
      environment: production
```

### 3.5 Commonly Used Actions

| Action | Purpose |
|--------|---------|
| `actions/checkout@v4` | Check out repository code |
| `actions/setup-node@v4` | Set up Node.js environment |
| `actions/setup-python@v5` | Set up Python environment |
| `actions/upload-artifact@v4` | Upload build artifacts |
| `actions/download-artifact@v4` | Download build artifacts |
| `actions/cache@v4` | Cache dependencies |
| `actions/github-script@v7` | Run JavaScript with GitHub API |
| `actions/configure-pages@v4` | Configure GitHub Pages |
| `actions/deploy-pages@v4` | Deploy to GitHub Pages |

### ✅ Hands-on: Deploy to GitHub Pages

**Step 1: Create index.html (if not already created)**

#### 🪟 Windows (PowerShell)

```powershell
@"
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitHub Workshop</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 40px 20px;
            background-color: #0d1117;
            color: #c9d1d9;
        }
        h1 { color: #58a6ff; }
        .card {
            background-color: #161b22;
            border: 1px solid #30363d;
            border-radius: 6px;
            padding: 16px;
            margin: 16px 0;
        }
    </style>
</head>
<body>
    <h1>🎉 Welcome to the GitHub Workshop!</h1>
    <div class="card">
        <h2>Auto-deployed with GitHub Actions</h2>
        <p>This page is automatically deployed every time you push to GitHub.</p>
    </div>
</body>
</html>
"@ | Out-File -FilePath "index.html" -Encoding utf8
```

#### 🍎 Mac (Terminal)

```bash
cat << 'EOF' > index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitHub Workshop</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 40px 20px;
            background-color: #0d1117;
            color: #c9d1d9;
        }
        h1 { color: #58a6ff; }
        .card {
            background-color: #161b22;
            border: 1px solid #30363d;
            border-radius: 6px;
            padding: 16px;
            margin: 16px 0;
        }
    </style>
</head>
<body>
    <h1>🎉 Welcome to the GitHub Workshop!</h1>
    <div class="card">
        <h2>Auto-deployed with GitHub Actions</h2>
        <p>This page is automatically deployed every time you push to GitHub.</p>
    </div>
</body>
</html>
EOF
```

**Step 2: Create the deploy workflow**

#### 🪟 Windows (PowerShell)

```powershell
@"
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: `${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
"@ | Out-File -FilePath ".github\workflows\deploy-pages.yml" -Encoding utf8
```

#### 🍎 Mac (Terminal)

```bash
cat << 'EOF' > .github/workflows/deploy-pages.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
EOF
```

**Step 3: Push & Verify**

```bash
git add .
git commit -m "Add GitHub Pages deploy workflow"
git push origin main
```

1. Check the **Actions** tab for successful workflow execution
2. Go to **Settings** → **Pages** to find your site URL
3. Visit the site to verify it's working

---

## Wrap Up (5 min)

### What We Learned Across 6 Workshops

| Workshop | Theme | Key Features |
|----------|-------|-------------|
| Preparation | Getting ready | Account, Git setup |
| WS 1 | Development cycle overview | Repository, Issue, Branch, PR |
| WS 2 | Repository & Issue | Repo settings, Issue management |
| WS 3 | Branch & Pull Request | Branch strategy, code review |
| WS 4 | Project management | Boards, automation, sprints |
| WS 5 | GitHub Actions | CI/CD, deployment, automation |
| WS 6 | GitHub Copilot | Chat, Agent, AI-assisted development |
| WS 7 | Release & Deployment | Tags, Releases, Pages, Packages |
| WS 8 | Security | Dependabot, scanning, protection |
| WS 9 | Administration | Repo, Org, Enterprise management |

### The Complete GitHub Development Cycle

```
1. Create Issue       → Define what needs to be done
2. Project management → Organize and prioritize tasks
3. Create Branch      → Work safely in isolation
4. Code & Push        → Record your changes
5. Pull Request       → Get code reviewed
6. GitHub Actions     → Automated tests & checks
7. Merge              → Integrate into main branch
8. Deploy             → Auto-deploy to production
9. Close Issue        → Task complete
```

### Next Steps

- 🤖 **AI**: GitHub Copilot (learn in the next Workshop 6!)
- 🔐 **Security**: Dependabot, Code scanning
- 📦 **Packages**: GitHub Packages, Container Registry
- 📊 **Analytics**: Insights, Actions usage metrics
- 🏢 **Organization**: Teams, Organization settings

---

## 📚 Reference Links

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax Reference](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub Skills](https://skills.github.com/) - Interactive learning courses
