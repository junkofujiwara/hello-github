# Workshop 8: Security DeepDive (60 min)

> 📖 [日本語版](../ja/08-security.md)

## 🎯 Workshop Goals

- Understand GitHub's security features and which are free vs paid
- Set up Dependabot to manage dependency vulnerabilities
- Learn about secret scanning and push protection
- Understand code scanning and CodeQL basics
- Know when GitHub Advanced Security (paid) is needed

---

## 📋 Agenda

| Time | Content |
|------|---------|
| 0:00 - 0:05 | Review of Workshop 7 |
| 0:05 - 0:20 | Security overview & free features |
| 0:20 - 0:35 | Dependabot & Supply chain security |
| 0:35 - 0:50 | Secret scanning, Code scanning & hands-on |
| 0:50 - 1:00 | Wrap Up |

---

## Review of Workshop 7 (5 min)

In Workshop 7, we learned about Tags, Releases, GitHub Pages, and GitHub Packages.  
In this workshop, we'll focus on **Security** — how to protect your code, secrets, and supply chain using GitHub's built-in features.

---

## Part 1: Security Overview (15 min)

### 1.1 Why Security Matters

Security isn't just for security experts — it's part of every developer's job. GitHub integrates security directly into the development workflow so you can catch problems **early**, before they reach production.

```
┌──────────────────────────────────────────────────────────┐
│              GitHub Security                              │
│                                                           │
│  🔗 Supply Chain     🔑 Secrets        🔍 Code           │
│  Dependency mgmt     Leak prevention   Vulnerability scan │
│                                                           │
│  🛡️ Branch Rules     📋 Advisories     📊 Overview       │
│  Protection rules    CVE database      Risk dashboard     │
└──────────────────────────────────────────────────────────┘
```

### 1.2 Free vs Paid Features

GitHub offers many security features **for free on all plans**. Some advanced features require purchasing **GitHub Advanced Security** products (paid).

> 💰 For the latest pricing, see the [GitHub Advanced Security pricing page](https://github.com/security/plans).

#### Free Features (All Plans)

| Feature | Description |
|---------|-------------|
| **Security policy** | `SECURITY.md` — tell users how to report vulnerabilities |
| **Dependency graph** | Visualize all dependencies in your project |
| **Dependabot alerts** | Get notified about vulnerable dependencies |
| **Dependabot security updates** | Auto-generate PRs to fix vulnerable dependencies |
| **Dependabot version updates** | Auto-generate PRs to keep dependencies up-to-date |
| **Security advisories** | Privately discuss and fix vulnerabilities |
| **Repository rulesets** | Enforce code standards and security rules |
| **Secret scanning (partner alerts)** | Notify service providers about leaked secrets (public repos) |
| **Push protection for users** | Block accidental secret commits (public repos) |
| **SBOM export** | Export Software Bill of Materials |

#### Free for Public Repos Only

Some features in the paid products are available **free for public repositories**:

| Feature | Public Repo | Private Repo |
|---------|:-----------:|:------------:|
| Code scanning (CodeQL) | ✅ Free | 🔒 Paid |
| Secret scanning (user alerts) | ✅ Free | 🔒 Paid |
| Push protection | ✅ Free | 🔒 Paid |
| Copilot Autofix | ✅ Free | 🔒 Paid |
| Dependency review | ✅ Free | 🔒 Paid |

#### Paid: GitHub Secret Protection 🔒

> ⚠️ **Paid** — Requires purchasing GitHub Secret Protection (GitHub Team or Enterprise plan).

| Feature | Description |
|---------|-------------|
| **Secret scanning (user alerts)** | Detect leaked tokens/credentials in private repos |
| **Copilot secret scanning** | AI-powered detection of unstructured secrets (passwords) |
| **Push protection (private repos)** | Block secret commits in private repos |
| **Delegated bypass** | Approval workflow for push protection bypass |
| **Custom patterns** | Define organization-specific secret patterns |
| **Security overview** | Organization-wide risk dashboard |

#### Paid: GitHub Code Security 🔒

> ⚠️ **Paid** — Requires purchasing GitHub Code Security (GitHub Team or Enterprise plan).

| Feature | Description |
|---------|-------------|
| **Code scanning (private repos)** | Static analysis with CodeQL for private repos |
| **Copilot Autofix** | AI-generated fixes for code scanning alerts |
| **Custom Dependabot auto-triage** | Auto-dismiss, snooze, or trigger fixes at scale |
| **Dependency review (private repos)** | Review dependency changes in PRs |
| **Security campaigns** | Fix security alerts at scale |
| **Security overview** | Organization-wide risk dashboard |

### 1.3 Security Policy

Create a `SECURITY.md` file in your repository to tell users how to responsibly report security issues.

```markdown
# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| 1.x     | ✅        |
| < 1.0   | ❌        |

## Reporting a Vulnerability

If you find a security vulnerability, please report it responsibly:

1. **Do NOT** create a public Issue
2. Email: security@example.com
3. We will respond within 48 hours
4. We will work with you to fix the issue before public disclosure
```

### ✅ Hands-on: Create a Security Policy

1. In your repository, click **Security** tab
2. Click **Set up a security policy** (or create `.github/SECURITY.md`)
3. Write a policy following the template above

---

## Part 2: Dependabot & Supply Chain Security (15 min)

### 2.1 What is Dependabot?

**Dependabot** monitors your project's dependencies and alerts you when vulnerabilities are found. It can also **automatically create PRs** to update vulnerable packages.

```
┌──────────┐     ┌──────────────┐     ┌──────────────┐
│  GitHub   │────▶│  Dependabot  │────▶│  Result      │
│  Advisory │     │              │     │              │
│  Database │     │ - Check deps │     │ - ⚠️ Alert   │
│           │     │ - Find vulns │     │ - 🔄 PR      │
│  (CVEs)   │     │ - Auto-fix   │     │ - ✅ Updated  │
└──────────┘     └──────────────┘     └──────────────┘
```

### 2.2 Dependency Graph

The dependency graph automatically analyzes your project and shows all direct and transitive dependencies.

**How to view:**
1. Go to your repository
2. Click **Insights** tab → **Dependency graph**

Supported ecosystems include: npm, pip, Maven, Gradle, Composer, Cargo, Go modules, and more.

### 2.3 Dependabot Alerts

When a known vulnerability is found in one of your dependencies, GitHub creates a Dependabot alert.

**How to enable:**
1. **Settings** → **Code security** (or **Code security and analysis**)
2. Enable **Dependabot alerts**

Each alert includes:
- Severity level (Critical, High, Medium, Low)
- Affected package and version range
- Recommended fix version
- CVE details and advisory link

### 2.4 Dependabot Security Updates

Dependabot can automatically create PRs to fix vulnerable dependencies.

**How to enable:**
1. **Settings** → **Code security**
2. Enable **Dependabot security updates**

```
Dependabot Alert: axios 0.21.1 has a known vulnerability (CVE-2023-XXXX)
    ↓
Dependabot automatically creates a PR:
    "Bump axios from 0.21.1 to 0.21.4"
    ↓
You review and merge the PR ✅
```

### 2.5 Dependabot Version Updates

Beyond security fixes, Dependabot can keep **all** dependencies up-to-date by regularly checking for new versions.

Create `.github/dependabot.yml`:

```yaml
version: 2
updates:
  # npm dependencies
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 10
    labels:
      - "dependencies"

  # GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
    labels:
      - "ci"
```

| Setting | Description |
|---------|-------------|
| `package-ecosystem` | Which ecosystem to monitor (npm, pip, etc.) |
| `directory` | Where the manifest file is located |
| `schedule.interval` | How often to check (`daily`, `weekly`, `monthly`) |
| `open-pull-requests-limit` | Max number of open PRs at once |
| `labels` | Labels to add to generated PRs |

### 2.6 Software Bill of Materials (SBOM)

You can export a complete list of your project's dependencies as an SBOM:

1. Go to **Insights** → **Dependency graph**
2. Click **Export SBOM**
3. Downloads in SPDX format

> 💡 SBOMs are increasingly required for regulatory compliance and supply chain transparency.

### ✅ Hands-on: Set Up Dependabot

1. **Enable Dependabot alerts** in Settings → Code security
2. **Enable Dependabot security updates**
3. **Create `.github/dependabot.yml`** for version updates

#### 🪟 Windows (PowerShell)

```powershell
# Create directory
New-Item -ItemType Directory -Path ".github" -Force

# Create dependabot.yml
@"
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
    labels:
      - "dependencies"
      - "ci"
"@ | Out-File -FilePath ".github\dependabot.yml" -Encoding utf8
```

#### 🍎 Mac (Terminal)

```bash
# Create directory
mkdir -p .github

# Create dependabot.yml
cat << 'EOF' > .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
    labels:
      - "dependencies"
      - "ci"
EOF
```

#### Common Steps

```bash
git add .github/dependabot.yml
git commit -m "Add Dependabot configuration"
git push origin main
```

---

## Part 3: Secret Scanning & Code Scanning (15 min)

### 3.1 Secret Scanning

**Secret scanning** detects secrets (API keys, tokens, passwords) that have been accidentally committed to your repository.

```
Developer accidentally commits:
    API_KEY=sk-1234567890abcdef
        ↓
Secret Scanning detects it:
    ⚠️ "GitHub Personal Access Token found in config.js"
        ↓
Alert created → Developer rotates the key 🔄
```

#### Availability

| Scope | Free | Paid (GitHub Secret Protection) |
|-------|:----:|:------:|
| Partner alerts (public repos) | ✅ | ✅ |
| User alerts (public repos) | ✅ | ✅ |
| User alerts (private repos) | ❌ | 🔒 Paid |
| Copilot secret scanning (AI) | ❌ | 🔒 Paid |
| Custom patterns | ❌ | 🔒 Paid |

#### How to Enable (Public Repos)

1. **Settings** → **Code security**
2. Enable **Secret scanning**

### 3.2 Push Protection

**Push protection** prevents secrets from being pushed to the repository in the first place — it blocks the push **before** the secret reaches GitHub.

```
$ git push origin main
remote: error: GH009: Secrets detected!
remote:
remote: — GitHub Personal Access Token found in config.js:3
remote:
remote: This push was blocked.
```

#### Availability

| Scope | Free | Paid |
|-------|:----:|:----:|
| Push protection for users (public repos) | ✅ | ✅ |
| Push protection (private repos) | ❌ | 🔒 Paid (Secret Protection) |
| Delegated bypass | ❌ | 🔒 Paid (Secret Protection) |

> 💡 Push protection for users is **on by default** for your personal account. You can manage it in **Settings** → **Code security and analysis**.

### 3.3 Code Scanning

**Code scanning** uses static analysis to find security vulnerabilities and coding errors in your code. GitHub's built-in tool is **CodeQL**.

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│  Code    │────▶│  CodeQL  │────▶│  Alerts  │
│  Push/PR │     │ Analysis │     │          │
│          │     │          │     │ - XSS    │
│          │     │ - Build  │     │ - SQLi   │
│          │     │ - Query  │     │ - CSRF   │
│          │     │ - Analyze│     │ - etc.   │
└──────────┘     └──────────┘     └──────────┘
```

#### Availability

| Scope | Free | Paid (GitHub Code Security) |
|-------|:----:|:------:|
| CodeQL (public repos) | ✅ | ✅ |
| CodeQL (private repos) | ❌ | 🔒 Paid |
| Copilot Autofix (public repos) | ✅ | ✅ |
| Copilot Autofix (private repos) | ❌ | 🔒 Paid |
| Security campaigns | ❌ | 🔒 Paid |

#### Setting Up Code Scanning (Public Repos)

1. **Settings** → **Code security**
2. Enable **Code scanning** → **Set up** → **Default**
3. Or create a workflow manually:

```yaml
# .github/workflows/codeql.yml
name: "CodeQL"

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 6 * * 1'  # Every Monday at 6:00 UTC

jobs:
  analyze:
    name: Analyze
    runs-on: ubuntu-latest
    permissions:
      security-events: write
    strategy:
      matrix:
        language: [ 'javascript' ]
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: ${{ matrix.language }}

      - name: Autobuild
        uses: github/codeql-action/autobuild@v3

      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v3
```

### 3.4 Dependency Review

**Dependency review** shows the security impact of dependency changes in a PR — before you merge.

#### Availability

| Scope | Free | Paid (GitHub Code Security) |
|-------|:----:|:------:|
| Public repos | ✅ | ✅ |
| Private repos | ❌ | 🔒 Paid |

#### Dependency Review Action

```yaml
# .github/workflows/dependency-review.yml
name: Dependency Review

on: pull_request

permissions:
  contents: read

jobs:
  dependency-review:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Dependency Review
        uses: actions/dependency-review-action@v4
        with:
          fail-on-severity: moderate
```

### 3.5 GitHub Advanced Security (Paid) Summary

> ⚠️ The following products are **paid** and require a GitHub Team or Enterprise plan. For the latest pricing, see the [GitHub security plans page](https://github.com/security/plans).

| Product | What It Does | Key Features |
|---------|-------------|--------------|
| **GitHub Secret Protection** 🔒 | Prevent secret leaks | Secret scanning (private), Push protection (private), Custom patterns, AI detection |
| **GitHub Code Security** 🔒 | Find and fix vulnerabilities | Code scanning (private), Copilot Autofix (private), Dependency review (private), Security campaigns |

### ✅ Hands-on: Explore Security Features

**Exercise 1: Check Your Repository's Security Tab**

1. Go to your repository
2. Click the **Security** tab
3. Explore: Security overview, Dependabot alerts, Code scanning alerts, Secret scanning alerts

**Exercise 2: Enable Default Security Settings**

1. **Settings** → **Code security**
2. Enable all free features available for your repository:
   - Dependabot alerts ✅
   - Dependabot security updates ✅
   - Secret scanning (if public repo) ✅

**Exercise 3: Review Dependabot Alerts (if any)**

1. **Security** tab → **Dependabot alerts**
2. Review any open alerts
3. Check the recommended fix versions
4. If a Dependabot PR exists, review and merge it

---

## Wrap Up (10 min)

### What We Learned Today

- ✅ GitHub's free security features vs paid (Advanced Security)
- ✅ How to create a Security Policy (`SECURITY.md`)
- ✅ Dependabot alerts, security updates, and version updates
- ✅ Secret scanning and push protection
- ✅ Code scanning with CodeQL
- ✅ Dependency review for PRs

### Security Best Practices Checklist

| Practice | Status |
|----------|--------|
| Enable Dependabot alerts | ☐ |
| Enable Dependabot security updates | ☐ |
| Add `.github/dependabot.yml` for version updates | ☐ |
| Create `SECURITY.md` | ☐ |
| Enable secret scanning (if public repo) | ☐ |
| Enable push protection | ☐ |
| Set up code scanning (if public repo) | ☐ |
| Add dependency review action | ☐ |
| Never commit secrets to code | ☐ |
| Use environment variables / Secrets for sensitive data | ☐ |

### Full Workshop Series Recap

| WS | Theme | Key Features |
|----|-------|-------------|
| Preparation | Getting ready | Account, Git setup |
| WS 1 | Development cycle overview | Repository, Issue, Branch, PR |
| WS 2 | Repository & Issue | Repo settings, Issue management |
| WS 3 | Branch & Pull Request | Branch strategy, code review |
| WS 4 | Project management | Boards, automation, sprints |
| WS 5 | GitHub Actions | CI/CD, deployment, automation |
| WS 6 | GitHub Copilot | Chat, Agent, Skills |
| WS 7 | Release & Deployment | Tags, Releases, Pages, Packages |
| WS 8 | Security | Dependabot, scanning, protection |
| WS 9 | Administration | Repo, Org, Enterprise management |

### Next Steps

- 🔒 **Enable all free security features** on your repositories
- 📋 **Create Dependabot configuration** for your projects
- 🔑 **Rotate any leaked secrets** found by scanning
- 🏢 **Evaluate GitHub Advanced Security** (paid) for private repos and organizations
- 📖 **GitHub Security Lab** — [securitylab.github.com](https://securitylab.github.com/)
- 🎓 **GitHub Certifications** — Consider the [GitHub Advanced Security certification](https://resources.github.com/learn/certifications/)

---

## 📚 Reference Links

- [GitHub Security Features](https://docs.github.com/en/code-security/getting-started/github-security-features)
- [About GitHub Advanced Security](https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security)
- [Dependabot Quickstart](https://docs.github.com/en/code-security/getting-started/dependabot-quickstart-guide)
- [About Secret Scanning](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning)
- [About Code Scanning](https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning)
- [GitHub Advisory Database](https://github.com/advisories)
- [GitHub Security Plans](https://github.com/security/plans)
