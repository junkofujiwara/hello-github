# Workshop 1: GitHub Development Cycle Walk-Through (60 min)

> 📖 [日本語版](../ja/01-github-development-cycle.md)

## 🎯 Workshop Goals

- Experience the full GitHub development cycle **from start to finish**
- Understand how Repository, Issue, Branch, Pull Request, and GitHub Actions connect together
- Get hands-on practice and complete the entire development flow

---

## 📋 Agenda

| Time | Content |
|------|---------|
| 0:00 - 0:05 | Review of preparation & today's goals |
| 0:05 - 0:15 | Step 1: Create a Repository |
| 0:15 - 0:22 | Step 2: Create an Issue |
| 0:22 - 0:32 | Step 3: Branch, Code, and Push |
| 0:32 - 0:42 | Step 4: Pull Request and Merge |
| 0:42 - 0:52 | Step 5: Set up GitHub Actions |
| 0:52 - 1:00 | Wrap Up |

---

## Review of Preparation (5 min)

Let's recall what we learned in the preparation workshop:
- GitHub account creation
- The software development cycle
- How Git works

Today, we'll experience the full GitHub development cycle **end to end**.

```
 ① Create Repository
      ↓
 ② Create Issue (define the task)
      ↓
 ③ Create Branch & Code (develop)
      ↓
 ④ Pull Request & Merge (review & integrate)
      ↓
 ⑤ GitHub Actions (automation)
```

---

## Part 1: Create a Repository (10 min)

### 1.1 What is a Repository?

A **Repository (repo)** is like a "project folder." It stores code, documents, and their entire change history.

### 1.2 Create a Repository

1. Go to GitHub → **+** (top right) → **New repository**
2. Enter the following:

| Field | Value |
|-------|-------|
| Repository name | `hello-github` |
| Description | `My first GitHub repository` |
| Visibility | **Public** |
| Initialize | ✅ Check **Add a README file** |

3. Click **Create repository**

### 1.3 Clone the Repository

#### 🪟 Windows (PowerShell)

```powershell
# Navigate to your working directory
cd C:\GitHub

# Clone
git clone https://github.com/YOUR-USERNAME/hello-github.git

# Move into the directory
cd hello-github

# Verify files
dir
```

#### 🍎 Mac (Terminal)

```bash
# Navigate to your working directory
cd ~/GitHub

# Clone
git clone https://github.com/YOUR-USERNAME/hello-github.git

# Move into the directory
cd hello-github

# Verify files
ls -la
```

> 💡 Replace `YOUR-USERNAME` with your actual GitHub username.

---

## Part 2: Create an Issue (10 min)

### 2.1 What is an Issue?

An **Issue** is a tool for managing tasks, bug reports, and feature requests.  
Think of it as a **To-Do list** that your team can share — a place to track what needs to be done.

### 2.2 Create Your First Issue

1. Go to the repository page → **Issues** tab
2. Click **New issue**
3. Enter the following:

| Field | Value |
|-------|-------|
| Title | `Create index.html` |
| Description | See below |

**Description:**
```markdown
## Overview
Create the initial HTML file for the project.

## Tasks
- [ ] Create index.html
- [ ] Add basic HTML structure
- [ ] Add a heading and greeting text

## Notes
First file to be created for this project.
```

4. Click **Submit new issue**

> 💡 Note the Issue number (e.g., `#1`). You'll use it later.

---

## Part 3: Branch, Code, and Push (15 min)

### 3.1 Create a Branch

A **Branch** is a mechanism for working separately without affecting the main code.  
Like a "branch" on a tree, you split off from the main trunk (main), do your work, and then merge back.

```
main ──────●──────●──────────●──────●─── (stable version)
                   \        /
feature/xxx ────────●──●───  (working branch)
```

```bash
# Create and switch to a new branch
git checkout -b feature/add-index-html
```

> 💡 The naming convention `feature/description` is a common branch naming pattern.

### 3.2 Create a File

#### 🪟 Windows (PowerShell)

```powershell
# Create index.html
@"
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello GitHub</title>
</head>
<body>
    <h1>Hello, GitHub!</h1>
    <p>This is my first page managed with GitHub.</p>
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
    <title>Hello GitHub</title>
</head>
<body>
    <h1>Hello, GitHub!</h1>
    <p>This is my first page managed with GitHub.</p>
</body>
</html>
EOF
```

### 3.3 Commit and Push

```bash
# Check status
git status

# Stage the file
git add index.html

# Commit (reference the Issue number)
git commit -m "Add index.html #1"

# Push
git push origin feature/add-index-html
```

> 💡 Including `#1` in the commit message links it to Issue #1.

---

## Part 4: Pull Request and Merge (10 min)

### 4.1 Create a Pull Request

A **Pull Request (PR)** is a **request** to merge your changes into the `main` branch.  
In team development, PRs are used to have other members review your code (code review).

1. Go to the GitHub repository page
2. Click the **Compare & pull request** banner (or go to **Pull requests** → **New pull request**)
3. Enter the following:

| Field | Value |
|-------|-------|
| Title | `Add index.html` |
| Description | `Closes #1` |
| Reviewers | (skip for this workshop) |

4. Click **Create pull request**

> 💡 Writing `Closes #1` in the description will **automatically close** Issue #1 when the PR is merged.

### 4.2 Review the Changes

The PR screen shows several useful views:
- **Conversation**: Comments and review discussions
- **Commits**: A list of commits (change records) included in this PR
- **Files changed**: The diff (what changed and how)

1. Click the **Files changed** tab
2. Review the diff (differences)
3. Verify the changes look correct

### 4.3 Merge

1. Click the **Merge pull request** button
2. Click **Confirm merge**
3. (Optional) Click **Delete branch** to clean up

### 4.4 Verify

1. Go to the **Code** tab → verify `index.html` exists on the `main` branch
2. Go to the **Issues** tab → verify Issue #1 is automatically closed
3. Update your local repository:

```bash
# Switch to main branch
git checkout main

# Pull the latest changes
git pull origin main
```

---

## Wrap Up (10 min)

### What We Learned Today

```
1. Repository  → Created a project
2. Issue       → Defined a task
3. Branch      → Created a safe workspace
4. Commit/Push → Recorded changes
5. Pull Request → Requested a review
6. Merge       → Integrated into main
7. Issue Close → Task automatically completed
```

### Next Workshop: "Repository & Issue DeepDive"

- Repository settings and management
- Issue templates and labels
- Milestone management

### 💡 GitHub Copilot: AI-Powered Development

Although we didn't cover it in today's workshop, GitHub offers an AI assistant called **GitHub Copilot** that can help at every stage of the development cycle.

#### What is GitHub Copilot?

GitHub Copilot is an **AI-powered coding assistant**. It provides code auto-completion, chat-based Q&A, code explanations, and fix suggestions — significantly boosting developer productivity.

#### How Copilot Fits into the Development Cycle

| Development Phase | Copilot Use Case |
|-------------------|-----------------|
| **Plan (Issue)** | Help draft Issue descriptions and templates |
| **Develop (Coding)** | Code completion, function generation, refactoring suggestions |
| **Review (PR)** | Auto-generate Pull Request summaries, assist code review |
| **Test (Actions)** | Auto-generate test code |
| **Documentation** | Auto-generate comments and documentation |

#### Key Copilot Features

- **Copilot Code Completion** — Real-time code suggestions in your editor
- **Copilot Chat** — Ask questions about code in natural language
- **Copilot Agent** — An AI agent that autonomously handles complex tasks (e.g., resolving Issues, multi-file edits)
- **Copilot for Pull Requests** — Auto-generate PR summaries

> 📝 We'll dive deeper into GitHub Copilot in Workshop 6. For now, just remember that **AI can support you throughout the entire development cycle**.

### Upcoming DeepDive Workshops

| Workshop | Theme | Topics |
|----------|-------|--------|
| Workshop 2 | Repository & Issue | Templates, labels, milestones |
| Workshop 3 | Branch & Pull Request | Branch strategies, code review, merge strategies |
| Workshop 4 | Project Management | Board views, automation, sprint management |
| Workshop 5 | GitHub Actions | CI/CD, automated testing, deployment |
| Workshop 6 | GitHub Copilot | Chat, Agent, AI-powered development |
| Workshop 7 | Release & Deployment | Tags, Releases, Pages, Packages |
| Workshop 8 | Security | Dependabot, scanning, protection |
| Workshop 9 | Administration | Repo, Org, Enterprise management |

---

## 📚 Reference Links

- [Creating a Repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
- [About Issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues)
- [About Pull Requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [About Branches](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches)
