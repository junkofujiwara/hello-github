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

### 1.1 Create a Repository

A **Repository (repo)** is like a "project folder." It stores code, documents, and their entire change history.

1. Go to GitHub → **+** (top right) → **New repository**
2. Enter the following:

| Field | Value |
|-------|-------|
| Repository name | `hello-github` |
| Description | `My first GitHub repository` |
| Visibility | **Public** |
| Initialize | ✅ Check **Add a README file** |

3. Click **Create repository**

### 1.2 Clone the Repository

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

### 3.1 What is a Branch?

A **Branch** is a mechanism for working separately without affecting the main code.  
Like a "branch" on a tree, you split off from the main trunk (main), do your work, and then merge back.

```
main ──────●──────●──────────●──────●─── (stable version)
                   \        /
feature/xxx ────────●──●───  (working branch)
```

### 3.2 Create a Branch

#### ⌨️ Using the Command Line

```bash
# Create and switch to a new branch
git checkout -b feature/add-index-html
```

#### 🖥️ Using VS Code

1. Open the cloned folder in VS Code (**File** → **Open Folder**)
2. Click the branch name (`main`) at the bottom left of the window
3. Select **Create new branch**
4. Type `feature/add-index-html` and press **Enter**

> 💡 The naming convention `feature/description` is a common branch naming pattern.  
> For more details on VS Code Git operations, see the [Tool Guide](tool-guide.md).

### 3.3 Create a File

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

#### 🖥️ Using VS Code

1. In the Explorer sidebar, click the **New File** icon (📄+) next to the project name
2. Name the file `index.html`
3. Paste the following content:

```html
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
```

4. Save with **Ctrl+S** (Mac: **Cmd+S**)

### 3.4 Commit and Push

#### ⌨️ Using the Command Line

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

#### 🖥️ Using VS Code

1. Click the **Source Control** icon (branch symbol) in the left sidebar
2. Click the **+** next to `index.html` to stage it
3. Type `Add index.html #1` in the message box at the top
4. Click the **✓ (Commit)** button
5. Click **… (menu)** → **Push** (or click **Sync Changes**)

> 💡 Including `#1` in the commit message links it to Issue #1.

---

## Part 4: Pull Request and Merge (10 min)

### 4.1 What is a Pull Request (PR)?

A **Pull Request (PR)** is a **request** to merge your changes into the `main` branch.  
In team development, PRs are used to have other members review your code (code review).

### 4.2 Create a Pull Request

1. Go to the GitHub repository page
2. Click the **Compare & pull request** banner (or go to **Pull requests** → **New pull request**)
3. Enter the following:

| Field | Value |
|-------|-------|
| Title | `Add index.html` |
| Description | `Closes #1` |

4. Click **Create pull request**

> 💡 Writing `Closes #1` in the description will **automatically close** Issue #1 when the PR is merged.

### 4.3 Review the Changes

The PR screen shows several useful views:
- **Conversation**: Comments and review discussions
- **Commits**: A list of commits (change records) included in this PR
- **Files changed**: The diff (what changed and how)

1. Click the **Files changed** tab
2. Review the diff (differences)
3. Verify the changes look correct

### 4.4 [Optional] Pair Up for Code Review 👥

> If you're attending the workshop with a friend or colleague, **try reviewing each other's Pull Requests!**  
> If you're working alone, skip this section and go to [4.5 Merge](#45-merge).

In real team development, code is typically reviewed by another team member before merging. Let's experience that workflow here.

#### Step A: Add a Collaborator

First, grant your partner access to your repository:

1. Go to your repository page → **Settings** tab
2. Click **Collaborators** in the left menu (you may need to re-enter your password)
3. Click **Add people**
4. Enter your partner's **GitHub username** or **email address**
5. Select them from the results → click **Add (username) to this repository**
6. Your partner will receive an invitation email — they need to click **Accept invitation**

```
┌──────────────────────────────────────┐
│  Settings → Collaborators            │
│                                      │
│  Manage access                       │
│  ┌────────────────────────────────┐  │
│  │ 👤 You              Owner      │  │
│  │ 👤 Partner          Pending... │  │
│  └────────────────────────────────┘  │
│                                      │
│  [Add people]                        │
└──────────────────────────────────────┘
```

> 💡 A **Collaborator** is a member who has read and write access to a repository. While anyone can *view* a public repository, only collaborators can *make changes* to it.

#### Step B: Request a Review

After creating your PR (or on an existing PR):

1. In the PR page, click the gear icon next to **Reviewers** in the right sidebar
2. Search for and select your partner's username
3. Your partner will receive a review request notification

#### Step C: Perform the Review (Partner's Task)

The reviewer follows these steps:

1. Open the Pull Request from the notification or PR page
2. Click the **Files changed** tab
3. Hover over a line and click **+** to add a comment on that line
4. After reviewing everything, click **Review changes** (top right)
5. Choose one of:
   - **Comment** — Leave comments only (neither approve nor reject)
   - **Approve** ✅ — Approve the changes
   - **Request changes** — Ask for modifications
6. Click **Submit review**

```
┌──────────────────────────────────────┐
│  Review changes                      │
│  ┌────────────────────────────────┐  │
│  │ Looks great! Clear and well    │  │
│  │ structured.                    │  │
│  └────────────────────────────────┘  │
│                                      │
│  ○ Comment                           │
│  ● Approve                           │
│  ○ Request changes                   │
│                                      │
│              [Submit review]         │
└──────────────────────────────────────┘
```

> 💡 **Review tips**: For your first review, keep it simple — try comments like "This looks great!" or "Maybe this could be even clearer if…". You don't need to write a perfect review!

#### Step D: Review Each Other's Repos

Your partner should also have a PR ready, so **visit each other's repositories** and review:

1. You → Review the PR on your partner's repository
2. Your partner → Review the PR on your repository

### 4.5 Merge

1. Confirm the changes in the PR (if you received a review, verify that it's been Approved)
2. Click the **Merge pull request** button
3. Click **Confirm merge**
4. (Optional) Click **Delete branch** to clean up

### 4.6 Verify

1. Go to the **Code** tab → verify `index.html` exists on the `main` branch
2. Go to the **Issues** tab → verify Issue #1 is automatically closed
3. (Optional) Verify your partner's review comments appear in the PR conversation
4. Update your local repository:

```bash
# Switch to main branch
git checkout main

# Pull the latest changes
git pull origin main
```

### ✅ Checkpoint

- [ ] Pull Request was created
- [ ] PR includes a reference to the Issue (`Closes #1`)
- [ ] (Optional) Added your partner as a collaborator and reviewed each other's PRs
- [ ] Merge completed
- [ ] Issue #1 was automatically closed

---

## Part 5: Set Up GitHub Actions (10 min)

### 5.1 What is GitHub Actions?

**GitHub Actions** is a mechanism that **automatically runs predefined tasks** when something happens in your repository (e.g., a push or PR creation).  
For example, you can set it up to "automatically run tests whenever code is pushed."

### 5.2 Create a Simple Workflow

1. Go to the repository page → click the **Actions** tab
2. Click **set up a workflow yourself**
3. Change the filename to `hello.yml`
4. Enter the following content:

```yaml
name: Hello GitHub Actions

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  greet:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Hello
        run: echo "Hello, GitHub Actions! 🚀"

      - name: Show date
        run: date

      - name: List files
        run: ls -la
```

5. Click **Commit changes** (commit directly to the `main` branch)

### 5.3 Verify the Workflow Run

1. Click the **Actions** tab
2. Click the workflow run that was triggered
3. Click the **greet** job
4. Review the logs for each step

> 💡 The workflow was automatically triggered by the push to the `main` branch!

### 5.4 Workflow Structure

```
name:         Workflow name
on:           Trigger (when to run)
jobs:         Job definitions
  job-name:
    runs-on:  Execution environment
    steps:    Processing steps
      - uses: Use an existing Action
      - run:  Run a command
```

### ✅ Checkpoint

- [ ] `.github/workflows/hello.yml` was created
- [ ] The workflow ran automatically
- [ ] Verified the results in the Actions tab

---

## Wrap Up (10 min)

### What We Learned Today

```
✅ Step 1: Repository      → Created a project
✅ Step 2: Issue            → Defined a task
✅ Step 3: Branch & Commit  → Developed safely
✅ Step 4: Pull Request     → Reviewed & integrated
✅ Step 5: GitHub Actions   → Automated tasks
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
- [GitHub Actions Quickstart](https://docs.github.com/en/actions/quickstart)
