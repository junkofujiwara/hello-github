# Workshop 2: Repository & Issue DeepDive (60 min)

> 📖 [日本語版](../ja/02-repository-and-issue.md)

## 🎯 Workshop Goals

- Gain a detailed understanding of Repository contents and settings
- Master techniques for working effectively with Issues
- Learn how to use templates, labels, and milestones

---

## 📋 Agenda

| Time | Content |
|------|---------|
| 0:00 - 0:05 | Review of Workshop 1 |
| 0:05 - 0:25 | Repository deep dive |
| 0:25 - 0:50 | Issue deep dive |
| 0:50 - 1:00 | Wrap Up |

---

## Review of Workshop 1 (5 min)

In Workshop 1, we experienced the full cycle: Repository → Issue → Branch → PR → Merge.  
Now let's focus on **Repository** and **Issue** and take a deeper look.

---

## Part 1: Repository Deep Dive (20 min)

### 1.1 Repository Structure

A repository contains much more than just source code. Here are the important files and folders you should know as part of project best practices:

```
my-project/
├── .github/                    # GitHub-specific settings
│   ├── workflows/              # GitHub Actions workflows
│   ├── ISSUE_TEMPLATE/         # Issue templates
│   ├── PULL_REQUEST_TEMPLATE.md # PR template
│   └── CODEOWNERS              # Code owner settings
├── src/                        # Source code
├── docs/                       # Documentation
├── tests/                      # Tests
├── .gitignore                  # Files excluded from Git tracking
├── README.md                   # Project description
├── LICENSE                     # License
├── CONTRIBUTING.md             # Contribution guide
└── CHANGELOG.md                # Change log
```

### 1.2 README.md

The README is the **"face" of your project** — the first thing people see when they visit your repository.  
Write it so visitors can quickly understand what the project is and how to use it.

```markdown
# Project Name

Short description of the project.

## Features
- Feature 1
- Feature 2

## Setup
How to get started.

## Usage
How to use the project.

## Contributing
Guidelines for contributing.

## License
License information.
```

### 1.3 .gitignore

A `.gitignore` file tells Git: **"Don't track these files."**  
Use it for passwords, secret keys, build artifacts, and other files you don't want in the repository.

#### 🪟 Windows (PowerShell)

```powershell
# Create .gitignore
@"
# OS-generated files
Thumbs.db
Desktop.ini

# Mac OS-generated files
.DS_Store

# Editor files
.vscode/
*.swp
*.swo

# Dependencies
node_modules/

# Build output
dist/
build/

# Environment files
.env
.env.local
"@ | Out-File -FilePath ".gitignore" -Encoding utf8
```

#### 🍎 Mac (Terminal)

```bash
# Create .gitignore
cat << 'EOF' > .gitignore
# OS-generated files
Thumbs.db
Desktop.ini

# Mac OS-generated files
.DS_Store

# Editor files
.vscode/
*.swp
*.swo

# Dependencies
node_modules/

# Build output
dist/
build/

# Environment files
.env
.env.local
EOF
```

> 💡 GitHub provides [template .gitignore files](https://github.com/github/gitignore) for many languages and frameworks. You can also auto-generate one at [gitignore.io](https://www.toptal.com/developers/gitignore).

### 1.4 Repository Settings

Go to the **Settings** tab to configure key options:

| Setting | Description |
|---------|-------------|
| **General** | Repository name, description, visibility |
| **Collaborators** | Add team members and manage permissions |
| **Branches** | Branch protection rules (covered in detail next workshop) |
| **Pages** | GitHub Pages (static site hosting) |
| **Secrets and variables** | Secret management for Actions |

### 1.5 Repository Visibility

| Type | Description | Use Case |
|------|-------------|----------|
| **Public** | Anyone can view | Open source, learning projects |
| **Private** | Invited members only | Company projects, personal projects |

### 1.6 LICENSE

A license defines how others can use your code.

| License | Feature | Use Case |
|---------|---------|----------|
| **MIT** | Very permissive | Most open-source projects |
| **Apache 2.0** | Patent protection | Enterprise projects |
| **GPL v3** | Copyleft (derivatives must be open) | Strong open-source projects |
| **No License** | All rights reserved | Private/proprietary code |

#### Adding a LICENSE

1. On the repository page → **Add file** → **Create new file**
2. Enter `LICENSE` as the filename
3. Click **Choose a license template** (appears on the right)
4. Select a template → **Review and submit**

### ✅ Hands-on: Improve Your Repository

Add the following to the repository you created in Workshop 1:

1. Create a `.gitignore` file
2. Improve your `README.md` following the template above

**🪟 Windows (PowerShell):**

```powershell
# Create a branch
git checkout -b improve/repository-setup

# Create .gitignore
@("node_modules/", ".env", ".DS_Store", "Thumbs.db") | Out-File -Encoding utf8 .gitignore

# Commit & push
git add .
git commit -m "Add basic repository configuration"
git push origin improve/repository-setup
```

**🍎 Mac (Terminal):**

```bash
# Create a branch
git checkout -b improve/repository-setup

# Create .gitignore
echo "node_modules/" > .gitignore
echo ".env" >> .gitignore
echo ".DS_Store" >> .gitignore
echo "Thumbs.db" >> .gitignore

# Commit & push
git add .
git commit -m "Add basic repository configuration"
git push origin improve/repository-setup
```

---

## Part 2: Issue Deep Dive (25 min)

### 2.1 The Role of Issues

Issues aren't just for bug reports — they're a versatile tool for many purposes:

| Purpose | Example |
|---------|---------|
| 🐛 **Bug report** | "Login button doesn't respond when clicked" |
| ✨ **Feature request** | "Add dark mode support" |
| 📋 **Task management** | "Design the database schema" |
| 💬 **Discussion** | "Which framework should we use?" |
| 📝 **Documentation** | "Rewrite the API documentation" |

### 2.2 Writing Good Issues

#### Bug Report Example

#### Bug Report Example

```markdown
## Bug Description
Cannot log in even after entering the correct password on the login page.

## Steps to Reproduce
1. Go to https://example.com/login
2. Enter username and password
3. Click "Login" button
4. Error message "Authentication failed" appears

## Expected Behavior
Should be able to log in with correct credentials.

## Actual Behavior
"Authentication failed" message is always displayed.

## Environment
- OS: Windows 11 / macOS Sequoia
- Browser: Chrome 120
- Version: v2.1.0

## Screenshots
(Attach if applicable)
```

#### Feature Request Example

```markdown
## Summary
Allow users to switch to dark mode.

## Background / Motivation
- Many users report eye strain when using the app at night
- Competing services have dark mode as a standard feature

## Proposed Implementation
- Add a theme toggle option in the settings page
- Offer three options: Light / Dark / System default
- Manage themes using CSS variables

## Acceptance Criteria
- [ ] Theme toggle UI implemented
- [ ] Dark mode CSS created
- [ ] Theme preference saved persistently
```

### 2.3 Labels

Labels are **classification tags** you attach to Issues and PRs. They are color-coded so you can tell the type at a glance.

#### Default Labels

| Label | Color | Purpose |
|-------|-------|---------|
| `bug` | 🔴 Red | Bug reports |
| `enhancement` | 🔵 Blue | New features |
| `documentation` | � Purple | Documentation improvements |
| `good first issue` | 🟢 Green | Good for newcomers |
| `help wanted` | 🟡 Yellow | Community help needed |
| `duplicate` | ⚪ Gray | Duplicate issue |
| `invalid` | ⚪ Gray | Invalid issue |
| `wontfix` | ⚪ Gray | Won't be fixed |

#### Creating Custom Labels

Go to **Issues** → **Labels** → **New label** to create your own labels.

Recommended custom labels:

| Label | Purpose |
|-------|---------|
| `priority: high` | High priority |
| `priority: medium` | Medium priority |
| `priority: low` | Low priority |
| `status: in-progress` | Currently being worked on |
| `status: blocked` | Blocked by another issue |
| `type: task` | General task |
| `type: question` | Question / discussion |

### 2.4 Milestones

Milestones let you group multiple Issues by "version" or "release" for organized tracking.  
You can see at a glance: "How much progress have we made toward v1.0?"

#### Creating a Milestone

1. **Issues** → **Milestones** → **New milestone**
2. Set the following:
   - **Title**: `v1.0 Release`
   - **Due date**: Target date
   - **Description**: `Features included in the first release`
3. Click **Create milestone**

#### Using Milestones

```
Milestone: v1.0 Release
├── Issue #1: Implement login feature      ✅ Closed
├── Issue #2: User registration feature    ✅ Closed
├── Issue #3: Display user profile         🔵 Open
└── Issue #4: Password reset               🔵 Open

Progress: ████████░░ 50% (2/4 completed)
```

### 2.5 Issue Templates

Issue templates let you create Issues with a predefined format.  
For bug reports, they auto-fill "Steps to Reproduce"; for feature requests, "Background / Motivation" — so you never forget to include important details.

#### How to Create Templates

1. Go to **Settings** → **General** → **Features** section
2. Click **Set up templates** under **Issues**
3. Add your templates

#### Bug Report Template

Create `.github/ISSUE_TEMPLATE/bug_report.md`:

```markdown
---
name: Bug Report
about: Report a bug to help us improve
title: '[BUG] '
labels: bug
assignees: ''
---

## Bug Description
<!-- A clear, concise description of the bug -->

## Steps to Reproduce
1. 
2. 
3. 

## Expected Behavior
<!-- What should happen -->

## Actual Behavior
<!-- What actually happened -->

## Environment
- OS: Windows / macOS
- Browser: 
- Version: 

## Screenshots
<!-- Attach if applicable -->
```

#### Feature Request Template

Create `.github/ISSUE_TEMPLATE/feature_request.md`:

```markdown
---
name: Feature Request
about: Suggest a new feature
title: '[FEATURE] '
labels: enhancement
assignees: ''
---

## Summary
<!-- A brief description of the feature -->

## Background / Motivation
<!-- Why is this feature needed? -->

## Proposed Implementation
<!-- How should it be implemented? -->

## Acceptance Criteria
- [ ] 
- [ ] 
- [ ] 
```

### 2.6 Useful Issue Features

#### Task Lists

You can use checklists inside Issues:

```markdown
## Tasks
- [x] Finalize the design
- [x] Create database tables
- [ ] Implement API
- [ ] Implement frontend
- [ ] Write tests
```

Progress is shown as a progress bar in the Issues list.

#### Cross-referencing Issues

```markdown
Related Issue: #5
Blocked by: Need to wait for #3 to complete
```

#### Auto-closing Issues with Keywords

When you include certain keywords in a PR or commit message, the linked Issue will be **automatically closed** when the PR is merged. This is a very convenient feature!

| Keyword | Example |
|---------|---------|
| `close` / `closes` / `closed` | `Closes #1` |
| `fix` / `fixes` / `fixed` | `Fixes #1` |
| `resolve` / `resolves` / `resolved` | `Resolves #1` |

### ✅ Hands-on: Practice with Issues

Perform the following tasks:

1. **Create labels** (3 or more custom labels)
2. **Create a milestone**
3. **Create Issue templates** (Bug Report & Feature Request)
4. **Create 3+ Issues using the templates**
   - At least one using the Bug Report template
   - At least one using the Feature Request template
   - Assign labels and a milestone to each Issue

---

## Wrap Up (10 min)

### What We Learned Today

- ✅ Repository structure (README, .gitignore, LICENSE, etc.)
- ✅ Repository settings and Visibility (Public / Private)
- ✅ How to write clear, helpful Issues
- ✅ Using labels and milestones effectively
- ✅ Creating Issue templates

### Next Workshop: "Branch & Pull Request DeepDive"

- Branch strategies (GitHub Flow, Git Flow)
- Pull Request best practices
- How to do code reviews
- Merge strategies (Merge, Squash, Rebase)

---

## 📚 Reference Links

- [About Repositories](https://docs.github.com/en/repositories)
- [About Issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues)
- [About Labels](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/managing-labels)
- [About Issue Templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository)
- [gitignore Templates](https://github.com/github/gitignore)
