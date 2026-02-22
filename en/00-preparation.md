# Preparation: Getting Ready for the Workshop (Pre-work)

> 📖 [日本語版](../ja/00-preparation.md)

## 🎯 Goals

- Create a GitHub account and complete basic setup
- Understand the overall software development cycle
- Understand the basics of how Git works
- Set up the development environment

---

## 📋 Agenda

| Time | Content |
|------|---------|
| 0:00 - 0:10 | Introduction: Why learn GitHub |
| 0:10 - 0:25 | GitHub account creation & setup |
| 0:25 - 0:40 | The software development cycle overview |
| 0:40 - 0:55 | Git basics |
| 0:55 - 1:00 | Wrap Up |

---

## Part 1: Introduction — Why Learn GitHub (10 min)

### 1.1 What is GitHub?

**GitHub** is a **Git-based version control platform**.  
In simple terms, it's a web service that helps teams manage everyone's code changes when working together. It's used daily by developers around the world.

### Why GitHub Matters

- 🌍 The world's largest platform with **over 100 million developers**
- 🏢 The **de facto standard tool** in professional software development
- 🤝 Essential for **team collaboration**
- 📝 Useful for **documentation management**, not just code
- 🤖 The foundation for **CI/CD (automated testing and deployment)**

### This Workshop Series

```
Preparation → Workshop 1 (full cycle walkthrough) → Workshops 2-9 (deep dives)
```

| Workshop | Theme |
|----------|-------|
| Preparation | Environment setup & basics |
| Workshop 1 | GitHub development cycle walkthrough |
| Workshop 2 | Repository & Issue deep dive |
| Workshop 3 | Branch & Pull Request deep dive |
| Workshop 4 | Project management deep dive |
| Workshop 5 | GitHub Actions deep dive |
| Workshop 6 | GitHub Copilot deep dive |
| Workshop 7 | Release & Deployment deep dive |
| Workshop 8 | Security deep dive |
| Workshop 9 | Administration deep dive |

---

## Part 2: GitHub Account Creation & Setup (15 min)

### 2.1 Creating an Account

1. Go to [https://github.com](https://github.com)
2. Click **Sign up**
3. Enter the following:
   - **Email address**: Your email
   - **Password**: A strong password
   - **Username**: Your desired username (this will become your profile URL)
4. Complete email verification
5. Set up your profile

> 💡 **Username tip**: Your username becomes part of your profile URL (`github.com/username`). You can change it later, but choose something memorable and recognizable.

### 2.2 Profile Setup

1. Click your icon (top right) → **Settings**
2. **Profile** section:
   - **Name**: Your display name
   - **Bio**: A brief self-introduction
   - **Avatar**: Upload a profile picture

### 2.3 Two-Factor Authentication (2FA)

Set up two-factor authentication to keep your account secure. 2FA adds an extra verification step (like a code from your phone) when you sign in:

1. **Settings** → **Password and authentication**
2. Click **Enable** under **Two-factor authentication**
3. Set up using an authenticator app (e.g., Google Authenticator)

> ⚠️ **Important**: Save your 2FA recovery codes in a safe place. You'll need them if you lose access to your phone.

---

## Part 3: The Software Development Cycle (15 min)

### 3.1 What is the Development Cycle?

Software development isn't a one-time effort — it's a continuous cycle of repeating these steps:

```
    ┌──────────┐
    │  Plan    │ ← Issue
    └────┬─────┘
         │
    ┌────▼─────┐
    │ Develop  │ ← Branch / Commit
    └────┬─────┘
         │
    ┌────▼─────┐
    │  Review  │ ← Pull Request
    └────┬─────┘
         │
    ┌────▼─────┐
    │ Release  │ ← Merge / Deploy
    └────┬─────┘
         │
         └──────→ Back to Plan (Feedback loop)
```

### 3.2 Each Phase at a Glance

| Phase | GitHub Feature | Description |
|-------|---------------|-------------|
| **Plan** | Decide what to build, manage tasks | Issue, Project |
| **Develop** | Write code, manage changes | Repository, Branch, Commit |
| **Review** | Verify code quality | Pull Request, Code Review |
| **Test** | Automated tests, quality checks | GitHub Actions |
| **Release** | Deploy to production | Merge, GitHub Actions, Releases |

### 3.3 How GitHub Supports the Development Cycle

Here's how the GitHub features map to each step of the development flow:

```
 Issue (define the task)
   ↓
 Branch (create a working branch)
   ↓
 Commit (save changes)
   ↓
 Pull Request (request a review)
   ↓
 Code Review (review the code)
   ↓
 GitHub Actions (run automated tests)
   ↓
 Merge (integrate into main)
   ↓
 Deploy / Release
```

### 3.4 Traditional Development vs. GitHub-Powered Development

Using GitHub makes many previously manual tasks much more efficient:

| Aspect | Traditional Approach | With GitHub |
|--------|---------------------|-------------|
| File management | Folder names (v1, v2...) | Git tracks change history automatically |
| Code sharing | Email, USB drives | Share via Repository |
| Task management | Excel, whiteboards | Issue & Project |
| Code review | Print and review on paper | Online review with Pull Requests |
| Testing & builds | Run manually | Automate with GitHub Actions |

### 3.5 DevOps and DevSecOps

Building on the development cycle, **DevOps** and **DevSecOps** take collaboration and automation even further.

#### What is DevOps?

DevOps combines **Development (Dev)** and **Operations (Ops)**. It's a culture and set of practices where development and operations teams work closely together to **continuously and rapidly** build, test, release, and operate software.

```
   Dev                   Ops
 ┌──────────┐      ┌──────────┐
 │ Plan      │      │ Deploy    │
 │ Code      │ ───▶ │ Monitor   │
 │ Build     │ ◀─── │ Feedback  │
 │ Test      │      │ Operate   │
 └──────────┘      └──────────┘
       ↑                 │
       └── Continuous ───┘
           Improvement
```

Key pillars of DevOps:

| Pillar | Description |
|--------|-------------|
| **CI (Continuous Integration)** | Frequently merge code changes and automatically build & test |
| **CD (Continuous Delivery / Deployment)** | Automatically prepare (or deploy) tested code to production |
| **IaC (Infrastructure as Code)** | Manage infrastructure configuration as code for reproducibility and automation |
| **Monitoring** | Monitor systems after release to detect issues early |

#### What is DevSecOps?

DevSecOps integrates **Security (Sec)** into the DevOps approach. Security is considered at **every stage** of the development cycle — not just at the end.

```
        DevSecOps
┌───────────────────────────┐
│     Dev + Sec + Ops       │
│                           │
│  Plan    ← Security reqs  │
│  Code    ← Secure coding  │
│  Build   ← Dependency scan│
│  Test    ← Security tests │
│  Release ← Vuln checks    │
│  Operate ← Monitoring &   │
│             Incident resp. │
└───────────────────────────┘
```

> 💡 The core idea of DevSecOps is **"Security is not an afterthought — it's built in from the start."** This approach is known as **"Shift Left."**

#### GitHub and DevOps / DevSecOps

GitHub provides a rich set of features to support DevOps and DevSecOps practices:

| DevOps / DevSecOps Practice | GitHub Feature |
|-----------------------------|----------------|
| CI/CD | GitHub Actions |
| Code review | Pull Requests |
| Security scanning | Dependabot, Code Scanning, Secret Scanning |
| IaC management | Version-control infrastructure code in Repositories |
| Monitoring integration | GitHub Actions + external monitoring tools |
| Security policies | Branch Protection Rules, CODEOWNERS |

> 📝 In this workshop series, Workshop 5 (GitHub Actions) covers CI/CD, and Workshop 8 (Security) dives deeper into DevSecOps practices.

---

## Part 4: Git Basics (15 min)

### 4.1 What is Git?

**Git** is a **distributed version control system**.  
"Version control" means recording the history of file changes so you can track who changed what and when.

> 💡 **Git ≠ GitHub** (these are often confused, but they're different things!)  
> **Git** = A tool that manages file change history (runs on your local machine)  
> **GitHub** = A cloud service that uses Git to enable team collaboration

### 4.2 Version Control Basics

With version control, you create a "save point" (Commit) each time you make changes. Like save data in a game, you can return to any previous state — a huge advantage.

```
┌──────────────────────────────────────────────┐
│           Timeline of Changes                │
│                                              │
│  v1 ──── v2 ──── v3 ──── v4 ──── v5        │
│  Init    Add     Update  Fix     Feature    │
│          header  style   bug     add        │
│                                              │
│  ← Any version can be restored! →            │
└──────────────────────────────────────────────┘
```

### 4.3 Key Terminology

| Term | Simple Explanation |
|------|-------------|
| **Repository** | A storage location for a project's files and change history — like a "project folder" |
| **Commit** | A snapshot of changes — like a "save point" in a game |
| **Branch** | A way to work separately without affecting the main code — like a "side branch" |
| **Merge** | Bringing a branch's work back into the main line |
| **Clone** | Copying an entire remote repository to your local machine |
| **Push** | Sending your local changes to GitHub |
| **Pull** | Getting the latest changes from GitHub to your local machine |

### 4.4 The Relationship Between Remote and Local

In GitHub development, your repository exists in two places: **GitHub (remote)** and **your computer (local)**. You sync between them as you work.

```
┌────────────────┐         ┌────────────────┐
│  Your PC       │  push → │    GitHub      │
│  (Local)       │ ← pull  │  (Remote)      │
│                │         │                │
│  Git           │         │  Repository    │
│  Repository    │         │  Issue         │
│                │         │  Pull Request  │
│                │         │  Actions       │
└────────────────┘         └────────────────┘
```

---

## Part 5: Set Up Your Development Environment

### 5.1 Install Git

#### 🪟 Windows

1. Download from [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Run the installer (default settings are OK)
3. Verify the installation:

```powershell
git --version
```

> 💡 **Alternative**: If you have [winget](https://learn.microsoft.com/windows/package-manager/winget/), you can also install with:
> ```powershell
> winget install --id Git.Git -e --source winget
> ```

#### 🍎 Mac

1. Open **Terminal** (Applications → Utilities → Terminal)
2. Run the following command:

```bash
git --version
```

3. If Git is not installed, you'll be prompted to install Xcode Command Line Tools. Follow the prompts.

> 💡 **Alternative**: If you have [Homebrew](https://brew.sh/), you can install with:
> ```bash
> brew install git
> ```

### 5.2 Configure Git (Common for Both OS)

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

> ⚠️ Use the **same email** as your GitHub account.

#### Verify the configuration

```bash
git config --global --list
```

### 5.3 Install a Text Editor

We recommend **Visual Studio Code (VS Code)**:

1. Download from [https://code.visualstudio.com/](https://code.visualstudio.com/)
2. Choose the installer for your OS (Windows / Mac)
3. Run the installer
4. Recommended extensions:
   - **GitHub Pull Requests and Issues** - Manage GitHub from VS Code
   - **GitLens** - Enhanced Git features

### 5.4 Verify Your Setup

#### 🪟 Windows (PowerShell)

```powershell
# Check Git version
git --version

# Check configuration
git config --global user.name
git config --global user.email

# Check VS Code (if installed)
code --version
```

#### 🍎 Mac (Terminal)

```bash
# Check Git version
git --version

# Check configuration
git config --global user.name
git config --global user.email

# Check VS Code (if installed)
code --version
```

---

## ✅ Pre-work Checklist

- [ ] GitHub account created
- [ ] Profile set up (name, icon)
- [ ] Two-factor authentication enabled
- [ ] Understand the development cycle overview
- [ ] Understand the basic Git concepts
- [ ] Git installed and configured
- [ ] Text editor installed

---

## Wrap Up (5 min)

### What We Learned Today

- ✅ GitHub account creation and initial setup
- ✅ The software development cycle and GitHub's role in it
- ✅ Git basics and key terminology
- ✅ Basic Git commands

### Next Workshop: "GitHub Development Cycle Walkthrough"

In the next workshop, we'll experience the full GitHub development workflow hands-on:

- Create a Repository
- Organize tasks with Issues
- Create a Branch and make code changes
- Review & merge with Pull Requests
- Experience automation with GitHub Actions

> 📝 **Before next time**: Make sure your GitHub account is created and Git is installed.

---

## 📚 Reference Links

- [GitHub Official Documentation](https://docs.github.com/en)
- [Git Official Documentation](https://git-scm.com/doc)
- [GitHub Skills](https://skills.github.com/) - Interactive learning courses
- [Visual Studio Code](https://code.visualstudio.com/)
