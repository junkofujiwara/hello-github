# Workshop 3: Branch & Pull Request DeepDive (60 min)

> 📖 [日本語版](../ja/03-branch-and-pullrequest.md)

## 🎯 Workshop Goals

- Understand branch strategies and learn to choose the right branching approach for your project
- Experience the full Pull Request and code review workflow hands-on
- Learn the differences between the 3 merge methods and when to use each

---

## 📋 Agenda

| Time | Content |
|------|---------|
| 0:00 - 0:05 | Review of Workshop 2 |
| 0:05 - 0:20 | Branch deep dive |
| 0:20 - 0:40 | Pull Request deep dive |
| 0:40 - 0:55 | Hands-on: Code review practice |
| 0:55 - 1:00 | Wrap Up |

---

## Review of Workshop 2 (5 min)

In Workshop 2, we dived deep into Repository and Issue management.  
Now let's focus on **Branch** and **Pull Request** — the core of the development workflow.

---

## Part 1: Branch Strategies (15 min)

### 1.1 How Branches Work

A branch is like a **pointer (bookmark)** that points to a commit in history.  
When you create a new branch, a new pointer is created pointing to the same commit.

```
       ← Commit A ← Commit B ← Commit C
                                    ↑
                                  main
                                    ↑
                            feature/login
```

When you make a new commit on a branch, only that branch's pointer moves forward (main stays put):

```
       ← Commit A ← Commit B ← Commit C ← Commit D
                                    ↑           ↑
                                  main    feature/login
```

### 1.2 Branch Strategies

#### GitHub Flow (Recommended for beginners)

The simplest and most widely-used branch strategy. Start with this one.

```
main ──●──────●──────────●──────●──────●──── (always deployable)
        \    /    \      /       \    /
         ●──●      ●──●──●       ●──●
       feature/A  feature/B    feature/C
```

**Rules:**
1. `main` branch is always kept in a deployable state
2. All work is done on branches created from `main`
3. Use Pull Requests for review before merging
4. Deploy immediately after merge

#### Git Flow

A more structured strategy for larger projects that need strict branch management.  
It's fine to learn this after you've mastered GitHub Flow.

```
main    ──●───────────────────●───────────── (released version)
           \                 /
develop ────●──●──●──●──●──●──●──●──── (development version)
              \  / \      /
feature/A ─────●    \    /
                 feature/B ──●──●
```

**Branch types:**

| Branch | Purpose | Example |
|--------|---------|---------|
| `main` | Released code | - |
| `develop` | Integration branch for development | - |
| `feature/*` | New features | `feature/user-auth` |
| `release/*` | Release preparation | `release/v1.0` |
| `hotfix/*` | Emergency bug fixes | `hotfix/login-fix` |

### 1.3 Branch Naming Conventions

Branch names should make it clear to anyone on the team **what work is being done**.

| Pattern | Example | Purpose |
|---------|---------|---------|
| `feature/description` | `feature/add-search` | New feature |
| `fix/description` | `fix/login-error` | Bug fix |
| `docs/description` | `docs/update-readme` | Documentation |
| `refactor/description` | `refactor/user-model` | Refactoring |
| `test/description` | `test/add-unit-tests` | Adding tests |

> 💡 It's also common to **include the Issue number**: `feature/42-add-search`

### 1.4 Branch Protection Rules

These settings protect the `main` branch from being accidentally broken. Always set these up for team development.

**Settings** → **Branches** → **Add branch protection rule**

| Rule | Plain-English Explanation |
|------|--------------------------|
| **Require a pull request before merging** | No direct pushes to main — PRs are required |
| **Require approvals** | Can't merge without at least 1 "OK" (approval) |
| **Require status checks to pass** | Automated tests (CI) must succeed before merge |
| **Require conversation resolution** | All review comments must be resolved before merge |
| **Require signed commits** | Require signed commits (identity verification) |
| **Include administrators** | Apply these rules even to admins |

### 1.5 Branch Operations

These commands work on both Windows (PowerShell / Git Bash) and Mac (Terminal):

```bash
# List branches
git branch              # Local branches
git branch -r           # Remote branches
git branch -a           # All branches

# Create and switch to a new branch
git checkout -b feature/new-feature    # Create and switch
git switch -c feature/new-feature      # Same (newer command)

# Switch between branches
git checkout main
git switch main         # Newer command

# Delete a branch (after merge)
git branch -d feature/old-feature      # Delete a merged branch
git branch -D feature/old-feature      # Force delete

# Delete a remote branch
git push origin --delete feature/old-feature

# Fetch latest info from remote
git fetch origin
git fetch --prune       # Clean up references to deleted remote branches
```

### ✅ Hands-on: Practice Branch Operations

```bash
# Create a branch
git checkout -b feature/add-style

# Create a CSS file
# (Use the OS-specific command below)
```

#### 🪟 Windows (PowerShell)

```powershell
@"
body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f6f8fa;
    color: #24292f;
}

h1 {
    color: #0969da;
    border-bottom: 1px solid #d0d7de;
    padding-bottom: 8px;
}
"@ | Out-File -FilePath "style.css" -Encoding utf8
```

#### 🍎 Mac (Terminal)

```bash
cat << 'EOF' > style.css
body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f6f8fa;
    color: #24292f;
}

h1 {
    color: #0969da;
    border-bottom: 1px solid #d0d7de;
    padding-bottom: 8px;
}
EOF
```

#### Common Steps

```bash
git add style.css
git commit -m "Add stylesheet"
git push origin feature/add-style
```

---

## Part 2: Pull Request Deep Dive (20 min)

### 2.1 The Role of Pull Requests

A Pull Request (PR) is more than just a merge request. It's the **most important place** in team development.

| Role | Description |
|------|-------------|
| **Code review** | Have other members check your changes |
| **Discussion** | Discuss implementation reasoning and alternatives |
| **Quality gate** | Verify that automated tests are passing |
| **Knowledge sharing** | Help the whole team deepen their understanding of the code |
| **Change record** | Preserve history of why changes were made |

### 2.2 Writing Good Pull Requests

#### Creating a PR Template

Create `.github/PULL_REQUEST_TEMPLATE.md`:

```markdown
## Summary
<!-- Brief description of this change -->

## Related Issue
<!-- Link to the related Issue -->
Closes #

## Changes Made
<!-- List the main changes -->
- 
- 
- 

## Type of Change
- [ ] 🐛 Bug fix
- [ ] ✨ New feature
- [ ] 📝 Documentation update
- [ ] ♻️ Refactoring
- [ ] 🧪 Add/update tests

## Testing
<!-- Describe how you tested -->
- [ ] Existing tests pass
- [ ] Added new tests

## Screenshots
<!-- Attach if UI was changed -->

## Checklist
- [ ] Code follows project style guidelines
- [ ] Added necessary tests
- [ ] Updated documentation
```

### 2.3 PR Size and Granularity

The smaller a PR, the easier it is to review and the faster bugs are found.

| PR Size | Line Changes (approx.) | Verdict |
|---------|----------------------|---------|
| 🟢 Small | ~200 lines | ✅ Easy to review! |
| 🟡 Medium | 200–500 lines | ⚠️ Within acceptable range |
| 🔴 Large | 500+ lines | ❌ Consider splitting |

> 💡 Smaller PRs lead to higher-quality reviews and faster time to merge.

#### Tips to Avoid Large PRs

1. **Split features into smaller pieces**: Break one feature into multiple PRs
2. **Separate refactoring**: Don't mix feature additions with refactoring
3. **Build incrementally**: Submit PRs in order: base → feature → tests

### 2.4 Code Review Best Practices

#### Reviewer Checklist

| Check | What to Look For |
|-------|-----------------|
| **Functionality** | Does it meet the requirements? |
| **Readability** | Is the code easy to read? |
| **Design** | Does it follow the architecture? |
| **Testing** | Are there sufficient tests? |
| **Security** | Any security concerns? |
| **Performance** | Any performance impact? |

#### How to Write Review Comments

The key is to always include **why** you think so and **what** you suggest as an alternative.

**Good comment examples:**

```
✅ "This function is getting long — extracting the validation part
    into a separate function would improve readability."

✅ "We might need a null check here. If user is undefined,
    this will throw an error."

✅ "Nit: Using 'users' instead of 'userList' would be more
    in line with common naming conventions."
```

**Comments to avoid:**

```
❌ "This is wrong." (no reason given)
❌ "Why did you write it this way?" (sounds confrontational)
❌ "Rewrite everything." (not specific)
```

#### Review Comment Prefixes

Agreeing on prefixes as a team makes it clear whether a comment is a required fix or just a suggestion.

| Prefix | Meaning | Action |
|--------|---------|--------|
| `[must]` | Must be fixed | Required fix |
| `[should]` | Recommended fix | Fix if possible |
| `[nit]` | Minor style issue | Optional |
| `[question]` | Question | Needs an answer |
| `[suggestion]` | Suggestion | Consider it |
| `[praise]` | Something good | No action needed 👏 |

#### Review Actions

| Action | Meaning |
|--------|---------|
| **Comment** | Comment only (no approval/rejection) |
| **Approve** | Approved (OK to merge) |
| **Request changes** | Request changes (re-review after fixes) |

### 2.5 Merge Strategies

GitHub offers 3 merge methods. Understand the differences so you can choose the right one.

#### ① Merge Commit (default)

```
main:    A ── B ──────── M
                \       /
feature:         C ── D
```

- All commit history is preserved
- A merge commit is created
- Full, accurate history remains

#### ② Squash and Merge

```
main:    A ── B ── CD'
                \
feature:         C ── D  (these are combined into one)
```

- Feature branch commits are combined into a single commit
- Main history stays clean
- Detailed commit history is lost

#### ③ Rebase and Merge

```
main:    A ── B ── C' ── D'
                \
feature:         C ── D  (rebased into a linear history)
```

- Commits are arranged linearly
- No merge commit is created
- History is clean and linear

#### Choosing a Merge Strategy

| Strategy | Best For |
|----------|----------|
| **Merge commit** | When you want to keep full history, large features |
| **Squash and merge** | When you want to combine small commits, minor fixes |
| **Rebase and merge** | When you want a linear history |

### 2.6 Conflict Resolution

When the **same part of the same file** is changed in two different branches, Git doesn't know which change to use. This is called a **conflict (collision)**.

#### What a Conflict Looks Like

Both changes are shown between `<<<<<<<` and `>>>>>>>` markers. You need to manually resolve the content.

```
<<<<<<< feature/add-profile
Name: Alice
=======
Name: Bob
>>>>>>> main
```

#### Resolution Steps (Windows / Mac)

```bash
# 1. Fetch main branch changes
git checkout feature/add-profile
git merge main

# 2. Check which files have conflicts
git status

# 3. Edit the file to resolve the conflict
# Fix the content between <<<<<<< and >>>>>>>

# 4. Stage the resolved file
git add <resolved-file>

# 5. Commit
git commit -m "Resolve conflict"

# 6. Push
git push origin feature/add-profile
```

> 💡 Simple conflicts can also be resolved on GitHub. Use the **Resolve conflicts** button on the PR page.

---

## Part 3: Hands-on — Code Review Practice (15 min)

### Scenario

Work in pairs (or individually) on the following:

#### Step 1: Create an Issue

```
Title: Add a footer to the website
Labels: enhancement
```

#### Step 2: Create a branch and make changes

These commands work on both Windows and Mac:

```bash
git checkout -b feature/add-footer
```

Create the following `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Project</title>
</head>
<body>
    <header>
        <h1>My Project</h1>
    </header>
    <main>
        <p>This is a GitHub workshop project.</p>
    </main>
    <footer>
        <p>&copy; 2026 My Project. All rights reserved.</p>
    </footer>
</body>
</html>
```

```bash
git add index.html
git commit -m "Add index.html with footer #IssueNumber"
git push origin feature/add-footer
```

#### Step 3: Create a Pull Request

- Fill in the PR template
- Include `Closes #IssueNumber`

#### Step 4: Code Review

As a reviewer, check:
- [ ] Is the HTML structure correct?
- [ ] Are proper meta tags included?
- [ ] Is indentation consistent?
- [ ] Is the commit message appropriate?

Leave **at least 2** review comments.

#### Step 5: Fix & Merge

- Address the review comments
- Get approval and merge

---

## Wrap Up (5 min)

### What We Learned Today

- ✅ Branch strategies (GitHub Flow and Git Flow)
- ✅ Branch protection rules
- ✅ How to write good PRs and use templates
- ✅ Code review best practices and comment tips
- ✅ Merge strategies (Merge, Squash, Rebase)
- ✅ How to resolve conflicts (collisions)

### Next Workshop: "Project Management DeepDive"

- How to use GitHub Projects
- Board view and Table view
- Automation rules
- Sprint management and iterations

---

## 📚 Reference Links

- [GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow)
- [About Pull Requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [About Code Reviews](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews)
- [About Merge Methods](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges)
- [About Branch Protection Rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-a-branch-protection-rule/about-protected-branches)
