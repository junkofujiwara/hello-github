# 🛠️ Tool Guide: Branch Management with GitHub Desktop & VS Code

> 📖 [日本語版](../ja/tool-guide.md)

This guide is a **supplementary resource** for the workshop series.  
It covers how to perform Git operations using **GitHub Desktop** and **VS Code**, in addition to the command line (CLI).

---

## 📋 Table of Contents

1. [Tool Comparison](#1-tool-comparison)
2. [GitHub Desktop](#2-github-desktop)
3. [Git in VS Code](#3-git-in-vs-code)
4. [Git CLI Quick Reference](#4-git-cli-quick-reference)
5. [Tips for Combining Tools](#5-tips-for-combining-tools)

---

## 1. Tool Comparison

| Aspect | Git CLI | GitHub Desktop | VS Code |
|--------|---------|---------------|---------|
| **Audience** | All developers | Beginners to intermediate | Developers writing code |
| **Interaction** | Command line | GUI (buttons and menus) | GUI + integrated terminal |
| **Strengths** | Full feature set, scriptable | Visual and beginner-friendly | Code editing and Git in one window |
| **Install** | [git-scm.com](https://git-scm.com) | [desktop.github.com](https://desktop.github.com) | [code.visualstudio.com](https://code.visualstudio.com) |

> 💡 All these tools perform the same underlying Git operations. Choose whichever works best for the situation.

---

## 2. GitHub Desktop

### 2.1 What is GitHub Desktop?

**GitHub Desktop** is a **free GUI client** provided by GitHub.  
It allows you to perform basic Git operations with buttons and menus — no need to memorize commands.

### 2.2 Installation and Setup

1. Download from [https://desktop.github.com](https://desktop.github.com)
2. After installation, click **Sign in to GitHub.com**
3. Authorize your GitHub account in the browser
4. Confirm your Git settings (name and email) → **Finish**

### 2.3 Cloning a Repository

1. **File** → **Clone repository**
2. Select the repository from the **GitHub.com** tab (or enter a URL)
3. Choose the local path
4. Click **Clone**

```
┌─────────────────────────────────┐
│  Clone a repository             │
│                                 │
│  GitHub.com │ URL               │
│  ┌───────────────────────────┐  │
│  │ user/my-first-project     │  │
│  │ user/hello-github         │  │
│  └───────────────────────────┘  │
│                                 │
│  Local path: C:\GitHub\...      │
│                    [Clone]      │
└─────────────────────────────────┘
```

### 2.4 Creating and Switching Branches

#### Create a New Branch

1. Click the **Current branch** button at the top
2. Click **New branch**
3. Enter a branch name (e.g., `feature/add-profile`)
4. Click **Create branch**

```
┌──────────────────────────────┐
│  Current branch ▼            │
│  ┌────────────────────────┐  │
│  │ 🔍 Filter              │  │
│  ├────────────────────────┤  │
│  │ ● main                 │  │
│  │   feature/add-profile  │  │
│  ├────────────────────────┤  │
│  │ [New branch]           │  │
│  └────────────────────────┘  │
└──────────────────────────────┘
```

#### Switch Branches

1. Click the **Current branch** button
2. Select the branch you want to switch to

### 2.5 Committing and Pushing Changes

1. Edit files — changes appear in the **Changes** tab on the left
2. Check the files you want to include in the commit
3. Enter a **commit message** at the bottom left
4. Click **Commit to branch-name**
5. Click **Push origin** at the top to send changes to the remote

```
┌──────────────────────────────────────┐
│  Changes (2)         History         │
│  ┌────────────────────────────────┐  │
│  │ ☑ README.md                   │  │
│  │ ☑ index.html                  │  │
│  └────────────────────────────────┘  │
│                                      │
│  Summary: Add profile section #1     │
│  Description: (optional details)     │
│                                      │
│  [Commit to feature/add-profile]     │
└──────────────────────────────────────┘
```

### 2.6 Creating a Pull Request

1. After committing and pushing to your branch
2. Go to **Branch** → **Create pull request**  
   (or click the **Create Pull Request** button shown at the top)
3. Your browser opens to the GitHub PR creation page

### 2.7 Fetching and Pulling Changes

| Action | Description | Button |
|--------|-------------|--------|
| **Fetch origin** | Check for remote updates (does not download) | **Fetch origin** at the top |
| **Pull origin** | Download remote changes to local | **Pull origin** (shown after Fetch) |

---

## 3. Git in VS Code

### 3.1 Built-in Git Support

VS Code has **built-in Git support** — no additional installation required.  
Click the **Source Control** icon (branch symbol) in the left sidebar to open the Git panel.

### 3.2 Cloning a Repository

1. Open the Command Palette: **Ctrl+Shift+P** (Mac: **Cmd+Shift+P**)
2. Type `Git: Clone` and select it
3. Enter the repository URL (or select **Clone from GitHub**)
4. Choose the local destination folder

### 3.3 Creating and Switching Branches

#### From the Status Bar (Easiest Way)

The current branch name is shown at the bottom-left of VS Code:

```
┌────────────────────────────────────────────┐
│  ... Editor ...                             │
├────────────────────────────────────────────┤
│ ⎇ main  ← Click here                       │
└────────────────────────────────────────────┘
```

1. Click the **branch name** (e.g., `main`) at the bottom left
2. From the dropdown:
   - Select an existing branch to **switch**
   - Select **Create new branch** to **create one**

#### From the Command Palette

| Action | Command |
|--------|---------|
| Create branch | `Git: Create Branch` |
| Switch branch | `Git: Checkout to...` |
| List branches | `Git: Branch` |
| Delete branch | `Git: Delete Branch` |

### 3.4 Committing and Pushing Changes

1. Edit and save your files
2. Click the **Source Control** icon in the left sidebar
3. Changed files are listed
4. Click **+** next to a file name to stage it
5. Enter a **commit message** in the text box at the top
6. Click the **✓ (Commit)** button
7. Click **… (menu)** → **Push** to send to remote

```
┌─────────────────────────────────┐
│  SOURCE CONTROL                 │
│  ┌───────────────────────────┐  │
│  │ Enter message...    [✓]   │  │
│  └───────────────────────────┘  │
│                                 │
│  Changes                        │
│    M  README.md           [+]   │
│    U  index.html          [+]   │
│                                 │
│  Staged Changes                 │
│    M  README.md           [-]   │
└─────────────────────────────────┘
```

> 💡 **M** = Modified, **U** = Untracked (new file), **D** = Deleted

### 3.5 Resolving Merge Conflicts

VS Code is especially helpful for resolving merge conflicts:

1. When a conflict occurs, markers appear in the file
2. Each conflict section shows buttons:
   - **Accept Current Change** — keep your changes
   - **Accept Incoming Change** — keep the other branch's changes
   - **Accept Both Changes** — keep both
   - **Compare Changes** — view a side-by-side diff

```
<<<<<<< HEAD (Current Change)
Your changes here
=======
Their changes here
>>>>>>> feature/other-branch (Incoming Change)
```

### 3.6 Recommended VS Code Extensions

| Extension | Description |
|-----------|-------------|
| **GitHub Pull Requests and Issues** | Manage PRs and Issues from within VS Code |
| **GitLens** | Enhanced Git features: line-by-line history, branch visualization, and more |
| **GitHub Copilot** | AI-powered code completion and chat |
| **Git Graph** | Visual branch / merge graph |

To install extensions:

1. Click the **Extensions** icon (square symbol) in the left sidebar
2. Search for the extension name
3. Click **Install**

---

## 4. Git CLI Quick Reference

A summary of frequently used commands in the workshops.  
These work on both **Windows (PowerShell / Git Bash)** and **Mac (Terminal)**.

### Branch Operations

```bash
# List branches
git branch

# List all branches (including remote)
git branch -a

# Create and switch to a new branch
git checkout -b feature/my-feature

# Switch branches
git checkout main

# Delete a branch
git branch -d feature/my-feature
```

### Managing Changes

```bash
# Check the status of changes
git status

# View the diff
git diff

# Stage all files
git add .

# Stage a specific file
git add README.md

# Commit
git commit -m "Describe your changes"
```

### Remote Operations

```bash
# Push to remote
git push origin feature/my-feature

# Pull latest from remote
git pull origin main

# Fetch remote info (without merging)
git fetch
```

### Useful Commands

```bash
# View commit history (compact)
git log --oneline

# Amend the last commit message
git commit --amend -m "Updated message"

# Stash changes temporarily
git stash

# Restore stashed changes
git stash pop
```

---

## 5. Tips for Combining Tools

### When to Use Which Tool

```
┌────────────────────────────────────────────────┐
│  Scenario                → Recommended Tool     │
├────────────────────────────────────────────────┤
│  First time using Git    → GitHub Desktop       │
│  Writing code            → VS Code              │
│  Automation / scripting  → Git CLI              │
│  Resolving conflicts     → VS Code              │
│  Visualizing repo history→ GitHub Desktop/GitLens│
└────────────────────────────────────────────────┘
```

### Mixing Tools Freely

All tools operate on the same Git repository, so you can **combine them however you like**:

- Create a branch in GitHub Desktop → write code in VS Code → push with CLI
- Commit in VS Code → review history in GitHub Desktop
- Clone with CLI → manage changes in GitHub Desktop

> 💡 The most important thing is to **use whatever tool feels most comfortable**. As you gain experience, switching between tools for different tasks will boost your productivity.

---

## 📚 Reference Links

- [GitHub Desktop Documentation](https://docs.github.com/en/desktop)
- [Using Git in VS Code](https://code.visualstudio.com/docs/sourcecontrol/overview)
- [VS Code Extension: GitHub Pull Requests and Issues](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github)
- [VS Code Extension: GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [Git Official Documentation](https://git-scm.com/doc)
