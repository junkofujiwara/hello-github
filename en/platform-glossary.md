# 📖 Platform Glossary: GitHub vs GitLab vs Bitbucket vs Azure DevOps

> 📖 [日本語版](../ja/platform-glossary.md)

Coming to GitHub from another platform? This glossary maps **equivalent concepts** across **GitHub**, **GitLab**, **Bitbucket**, and **Azure DevOps** so you can find your way quickly.

---

## 📋 Table of Contents

1. [Quick Reference Table](#1-quick-reference-table)
2. [Repository & Project Structure](#2-repository--project-structure)
3. [Branching & Code Review](#3-branching--code-review)
4. [CI/CD & Automation](#4-cicd--automation)
5. [Project Management & Issues](#5-project-management--issues)
6. [Security & Access Control](#6-security--access-control)
7. [Packages & Releases](#7-packages--releases)
8. [Organization & Administration](#8-organization--administration)
9. [Key Differences to Watch Out For](#9-key-differences-to-watch-out-for)

---

## 1. Quick Reference Table

| Concept | GitHub | GitLab | Bitbucket | Azure DevOps |
|---------|--------|--------|-----------|--------------|
| **Code hosting unit** | Repository | Project | Repository | Repository (in a Project) |
| **Grouping repos** | Organization | Group | Workspace | Project |
| **Code review request** | Pull Request (PR) | Merge Request (MR) | Pull Request (PR) | Pull Request (PR) |
| **CI/CD config file** | `.github/workflows/*.yml` | `.gitlab-ci.yml` | `bitbucket-pipelines.yml` | `azure-pipelines.yml` |
| **CI/CD system** | GitHub Actions | GitLab CI/CD | Bitbucket Pipelines | Azure Pipelines |
| **CI/CD runner** | Runner (GitHub-hosted / Self-hosted) | Runner (Shared / Specific) | Runner (Cloud / Self-hosted) | Agent (Microsoft-hosted / Self-hosted) |
| **CI/CD unit of work** | Job (in a Workflow) | Job (in a Pipeline) | Step (in a Pipeline) | Task (in a Pipeline) |
| **Issue tracking** | Issues | Issues | Jira (integrated) / Issues (limited) | Work Items |
| **Project board** | Projects (Project Board) | Boards | Jira Board / Trello | Boards |
| **Wiki / Docs** | Wiki / GitHub Pages | Wiki / GitLab Pages | Wiki / Confluence (integrated) | Wiki |
| **Package registry** | GitHub Packages | GitLab Package Registry | — (via Jira/Artifactory) | Azure Artifacts |
| **Static site hosting** | GitHub Pages | GitLab Pages | — | Azure Static Web Apps (separate) |
| **Secret management** | Actions Secrets | CI/CD Variables (masked) | Repository Variables / Secured | Pipeline Variables (secret) |
| **Code scanning** | Code Scanning (CodeQL) | SAST / DAST | — (third-party) | Azure DevOps Extensions |
| **Dependency alerts** | Dependabot | Dependency Scanning | — (third-party) | — (third-party) |
| **Container registry** | GitHub Container Registry (ghcr.io) | GitLab Container Registry | — (third-party) | Azure Container Registry (separate) |
| **Code snippet sharing** | Gist | Snippets | Snippets | — |
| **Inline code suggestions** | Suggested Changes (in PR) | Suggestions (in MR) | — | — |

---

## 2. Repository & Project Structure

### Repository

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Repository** | The fundamental unit. Belongs to a User or Organization. |
| **GitLab** | **Project** | A project contains a single Git repository plus CI/CD, issues, etc. |
| **Bitbucket** | **Repository** | Lives inside a Workspace (Cloud) or Project (Server/Data Center). |
| **Azure DevOps** | **Repository** | Lives inside a Project, which can hold multiple repos. |

### Grouping / Organization

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Organization** | A shared account that owns repos. Has teams for access control. |
| **GitLab** | **Group / Subgroup** | Hierarchical. A group contains projects and can nest subgroups. |
| **Bitbucket** | **Workspace** | Top-level container (Cloud). Projects group repos within a workspace. |
| **Azure DevOps** | **Organization → Project** | Org is the top level. A Project groups repos, pipelines, boards, etc. |

### Fork

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Fork** | Server-side copy of a repo under your account. Common for OSS contributions. |
| **GitLab** | **Fork** | Same concept. Fork a project to your namespace. |
| **Bitbucket** | **Fork** | Same concept. Fork within the same workspace or to another. |
| **Azure DevOps** | **Fork** | Supported, but less commonly used. |

---

## 3. Branching & Code Review

### Pull Request / Merge Request

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Pull Request (PR)** | Request to merge a branch. Supports reviews, checks, and discussions. |
| **GitLab** | **Merge Request (MR)** | Functionally identical to a PR. The term emphasizes *merging*. |
| **Bitbucket** | **Pull Request (PR)** | Same as GitHub's PR. |
| **Azure DevOps** | **Pull Request (PR)** | Same concept with policies for required reviewers, build validation, etc. |

> 💡 "Pull Request" and "Merge Request" mean the same thing — a request to review and integrate code. If someone says "MR", they likely come from GitLab.

### Branch Protection

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Branch Protection Rules / Rulesets** | Require reviews, status checks, restrict who can push. |
| **GitLab** | **Protected Branches** | Restrict push/merge by role. Can require approval rules. |
| **Bitbucket** | **Branch Permissions** | Restrict write access per branch pattern. |
| **Azure DevOps** | **Branch Policies** | Require reviewers, linked work items, successful builds, etc. |

### Code Review

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Review (Approve / Request Changes / Comment)** | Reviewers leave a formal review status. |
| **GitLab** | **Approval Rules** | Configurable required approvals. Reviewers and approvers are distinct. |
| **Bitbucket** | **Approve / Needs Work** | Simpler review states. |
| **Azure DevOps** | **Approve / Reject / Wait for Author** | Multiple vote options on a PR. |

### Draft

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Draft Pull Request** | Cannot be merged until marked "Ready for review." |
| **GitLab** | **Draft Merge Request** | Prefixed with `Draft:` in the title. Same behavior. |
| **Bitbucket** | — | No native draft PR. Use naming conventions (e.g., `[WIP]`). |
| **Azure DevOps** | **Draft Pull Request** | Supported. Cannot complete until published. |

---

## 4. CI/CD & Automation

### Pipeline / Workflow

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Workflow** (contains **Jobs** → **Steps**) | Defined in `.github/workflows/*.yml`. Triggered by events. |
| **GitLab** | **Pipeline** (contains **Stages** → **Jobs**) | Defined in `.gitlab-ci.yml`. Stages run sequentially, jobs in a stage run in parallel. |
| **Bitbucket** | **Pipeline** (contains **Steps**) | Defined in `bitbucket-pipelines.yml`. |
| **Azure DevOps** | **Pipeline** (contains **Stages** → **Jobs** → **Steps**) | YAML (`azure-pipelines.yml`) or Classic (GUI) editor. |

### Runner / Agent

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Runner** | GitHub-hosted (Ubuntu, Windows, macOS) or self-hosted. |
| **GitLab** | **Runner** | Shared, group, or project runners. Configured with tags. |
| **Bitbucket** | **Runner** | Atlassian-hosted or self-hosted (Bitbucket Runner). |
| **Azure DevOps** | **Agent** | Microsoft-hosted or self-hosted agents in Agent Pools. |

### Reusable CI Components

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Action** (from Marketplace) / **Reusable Workflow** | Actions are the plugin ecosystem. Reusable workflows call other workflows. |
| **GitLab** | **CI/CD Component** / **include** | Use `include:` to share pipeline templates. Components Catalog (newer). |
| **Bitbucket** | **Pipe** | Pre-built integration steps from the Marketplace. |
| **Azure DevOps** | **Task** / **Template** | Tasks from the Marketplace. Templates for reuse. |

### Environment & Deployment

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Environment** | Named target (e.g., `production`) with protection rules and secrets. |
| **GitLab** | **Environment** | Track deployments per environment. Supports review apps. |
| **Bitbucket** | **Deployment Environment** | Define environments for tracking deployments. |
| **Azure DevOps** | **Environment** | Target for deployment jobs with approvals and checks. |

---

## 5. Project Management & Issues

### Issue / Work Item

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Issue** | Lightweight. Supports labels, milestones, assignees. Markdown-based. |
| **GitLab** | **Issue** | Richer built-in features: weights, due dates, health status, epics. |
| **Bitbucket** | **Jira Issue** (typically) | Native issues are minimal. Most teams use Jira integration. |
| **Azure DevOps** | **Work Item** (Bug, Task, User Story, Epic, etc.) | Full-featured with customizable work item types and processes. |

### Labels / Tags

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Labels** | Color-coded tags on issues and PRs. |
| **GitLab** | **Labels** | Scoped labels (e.g., `priority::high`) for exclusive categories. |
| **Bitbucket** | — (in Jira: **Labels**) | Limited native labeling. |
| **Azure DevOps** | **Tags** | Free-form tags on work items. |

### Milestone / Iteration

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Milestone** | Groups issues/PRs with a due date and progress bar. |
| **GitLab** | **Milestone / Iteration** | Milestones for releases, Iterations for sprints. |
| **Bitbucket** | — (in Jira: **Fix Version / Sprint**) | Use Jira for sprint planning. |
| **Azure DevOps** | **Iteration (Sprint)** | Hierarchical iteration paths. Full sprint planning support. |

### Project Board

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Projects** (new) / Project Board (classic) | Table, board, and roadmap views. Custom fields. Powered by GraphQL. |
| **GitLab** | **Boards** | Kanban boards based on labels or assignees. |
| **Bitbucket** | **Jira Board / Trello** | Scrum or Kanban boards via Jira. |
| **Azure DevOps** | **Boards** | Kanban boards with swimlanes, WIP limits, cumulative flow. |

---

## 6. Security & Access Control

### Access Tokens

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Personal Access Token (PAT)** / **Fine-grained PAT** | Used for API and Git operations. Fine-grained PATs scope to specific repos. |
| **GitLab** | **Personal Access Token / Project Access Token / Group Access Token** | Multiple token scopes at different levels. |
| **Bitbucket** | **App Password** / **Repository Access Token** | App Passwords are user-level. |
| **Azure DevOps** | **Personal Access Token (PAT)** | Scoped to organization with granular permissions. |

### Roles & Permissions

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Read / Triage / Write / Maintain / Admin** | Repository-level roles. Organization roles for broader access. |
| **GitLab** | **Guest / Reporter / Developer / Maintainer / Owner** | Hierarchical roles at project and group levels. |
| **Bitbucket** | **Read / Write / Admin** | Simpler role model. |
| **Azure DevOps** | **Reader / Contributor / Project Admin / etc.** | Fine-grained, security-group–based permissions. |

### Code Scanning / Security

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Code Scanning** / **Secret Scanning** / **Dependabot** | Part of GitHub Advanced Security (GHAS). |
| **GitLab** | **SAST / DAST / Dependency Scanning / Secret Detection** | Built into GitLab Ultimate. |
| **Bitbucket** | — | Relies on third-party integrations (e.g., Snyk, SonarCloud). |
| **Azure DevOps** | **Microsoft Defender for DevOps** / Extensions | Security integrations via marketplace or Defender. |

---

## 7. Packages & Releases

### Package Registry

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **GitHub Packages** | npm, Maven, NuGet, Docker (ghcr.io), RubyGems. |
| **GitLab** | **Package Registry** | npm, Maven, NuGet, PyPI, Conan, Go, and more. |
| **Bitbucket** | — | No built-in registry. Use Artifactory or other external tools. |
| **Azure DevOps** | **Azure Artifacts** | npm, Maven, NuGet, Python, Universal Packages. |

### Release

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Release** | Based on Git tags. Attach binaries. Auto-generate release notes. |
| **GitLab** | **Release** | Similar to GitHub. Linked to milestones and tags. |
| **Bitbucket** | **Downloads** | File downloads section. No formal release feature. Use Jira Releases. |
| **Azure DevOps** | **Release** (Classic) / Pipeline artifacts | Classic Releases (GUI) or YAML pipeline deployments. |

---

## 8. Organization & Administration

### Team Management

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Teams** (inside an Organization) | Nested teams. Assign repo access per team. Mention with `@org/team`. |
| **GitLab** | **Groups / Subgroups** | Members inherit access from parent groups. |
| **Bitbucket** | **User Groups** (inside a Workspace) | Groups for permission management. |
| **Azure DevOps** | **Teams** (inside a Project) | Each team gets its own board, backlog, and iteration. |

### Single Sign-On (SSO)

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **SAML SSO** / **SCIM** (Enterprise) | Org-level or Enterprise-level SSO with identity provider. |
| **GitLab** | **SAML SSO / SCIM / LDAP** | Group-level SAML. Instance-level LDAP. |
| **Bitbucket** | **Atlassian Access** (SAML / SCIM) | Managed through Atlassian organization. |
| **Azure DevOps** | **Azure AD (Entra ID)** | Integrated natively with Microsoft identity platform. |

### Audit & Compliance

| Platform | Term | Notes |
|----------|------|-------|
| **GitHub** | **Audit Log** | Organization and Enterprise audit logs. API available. |
| **GitLab** | **Audit Events** | Instance, group, and project-level events. |
| **Bitbucket** | **Audit Log** (via Atlassian Admin) | Workspace-level audit logs. |
| **Azure DevOps** | **Auditing** | Organization-level audit stream. |

---

## 9. Key Differences to Watch Out For

### For GitLab users coming to GitHub

| GitLab | GitHub | Tip |
|--------|--------|-----|
| Merge Request (MR) | **Pull Request (PR)** | Same concept, different name. |
| `.gitlab-ci.yml` | **`.github/workflows/*.yml`** | GitHub uses multiple workflow files, not one. |
| Stages → Jobs | **Jobs → Steps** | Terminology hierarchy is different. |
| `include:` templates | **Reusable Workflows / Composite Actions** | Different mechanism for sharing CI logic. |
| Scoped Labels (`priority::high`) | **Labels** (manual convention) | GitHub labels are flat. Use naming conventions like `priority: high`. |
| Epics | **Projects** (using roadmap view) | GitHub doesn't have "Epics" — use Projects for high-level tracking. |

### For Bitbucket users coming to GitHub

| Bitbucket | GitHub | Tip |
|-----------|--------|-----|
| Workspace | **Organization** | Top-level container for repos and teams. |
| Pipelines / Pipes | **Actions / Workflows** | "Pipes" ≈ "Actions" from the Marketplace. |
| App Password | **Personal Access Token (PAT)** | GitHub PATs have fine-grained scope options. |
| Jira integration | **GitHub Issues + Projects** | Or continue using Jira with the GitHub for Jira app. |

### For Azure DevOps users coming to GitHub

| Azure DevOps | GitHub | Tip |
|--------------|--------|-----|
| Work Items | **Issues** | Simpler but extensible via Projects custom fields. |
| Boards | **Projects** (board view) | GitHub Projects supports Kanban-style boards. |
| Azure Pipelines | **GitHub Actions** | YAML-based but different syntax and ecosystem. |
| Agent | **Runner** | Same concept — the machine that executes CI/CD jobs. |
| Artifacts | **GitHub Packages** | Different supported formats. |
| Azure Repos | **GitHub Repositories** | Both are Git-based; migration is straightforward. |

---

## 💡 Tips for a Smooth Transition

1. **Don't translate 1:1** — Each platform has its own philosophy. Adapt to the platform's strengths rather than trying to replicate your old workflow exactly.
2. **Explore the Marketplace** — GitHub's [Actions Marketplace](https://github.com/marketplace?type=actions) is the equivalent of GitLab's CI/CD components, Bitbucket's Pipes, and Azure DevOps's Task extensions.
3. **Use the GitHub CLI (`gh`)** — It accelerates common tasks like creating PRs, checking workflow runs, and managing issues from the terminal.
4. **Read the migration guides**:
   - [Migrating from GitLab to GitHub](https://docs.github.com/en/migrations/importing-source-code/using-github-importer/importing-a-repository-with-github-importer)
   - [Migrating from Bitbucket to GitHub](https://docs.github.com/en/migrations/importing-source-code/using-github-importer/importing-a-repository-with-github-importer)
   - [Migrating from Azure DevOps to GitHub](https://docs.github.com/en/migrations/importing-source-code/using-github-importer/importing-a-repository-with-github-importer)

---

[Back to README](../README.md)
