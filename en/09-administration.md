# Workshop 9: Administration DeepDive (60 min)

> 📖 [日本語版](../ja/09-administration.md)

## 🎯 Workshop Goals

- Understand the three levels of GitHub administration: Repository, Organization, Enterprise
- Master repository settings and access control
- Learn Organization management: roles, teams, and policies
- Know which features require an Enterprise contract (paid)
- Plan governance and compliance strategies for your organization

---

## 📋 Agenda

| Time | Content |
|------|---------|
| 0:00 - 0:05 | Review of Workshop 8 |
| 0:05 - 0:20 | Repository administration |
| 0:20 - 0:40 | Organization administration |
| 0:40 - 0:55 | Enterprise administration |
| 0:55 - 1:00 | Wrap Up |

---

## Review of Workshop 8 (5 min)

In Workshop 8, we learned about GitHub's security features including Dependabot, secret scanning, and code scanning.  
In this workshop, we'll focus on **Administration** — how to manage repositories, organizations, and enterprises as an administrator.

---

## Part 1: Repository Administration (15 min)

### 1.1 The Three Levels of GitHub Administration

GitHub administration is organized in three hierarchical levels. Each level has its own settings and controls.

```
┌─────────────────────────────────────────────────────┐
│  Enterprise (top-level governance)                   │
│  🏢 Enterprise contract required                     │
│                                                      │
│  ┌───────────────────────────────────────────────┐  │
│  │  Organization (team-level management)          │  │
│  │  👥 Free for all plans                         │  │
│  │                                                │  │
│  │  ┌─────────────────────────────────────────┐  │  │
│  │  │  Repository (project-level settings)     │  │  │
│  │  │  📁 Free for all plans                   │  │  │
│  │  └─────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

| Level | Managed By | Available On |
|-------|-----------|-------------|
| Repository | Repo admin / Org owner | All plans (Free, Pro, Team, Enterprise) |
| Organization | Org owner | All plans (Free, Pro, Team, Enterprise) |
| Enterprise | Enterprise owner | 🔒 Enterprise contract required |

### 1.2 Repository Roles

Repository access is controlled by roles. Each role provides a different level of permissions.

| Role | Permissions | Use Case |
|------|------------|----------|
| **Read** | View code, issues, PRs | Stakeholders, reviewers |
| **Triage** | Manage issues, PRs (no code write) | Project managers |
| **Write** | Push code, manage issues/PRs | Developers |
| **Maintain** | Manage settings (no destructive actions) | Tech leads |
| **Admin** | Full control including danger zone | Repository owners |

> 💡 For personal repositories, you can invite **collaborators** directly. For organization repositories, access is managed through **teams**.

### 1.3 Repository Settings

Key repository settings that administrators should know:

#### General Settings

| Setting | Description |
|---------|-------------|
| **Visibility** | Public, Private, or Internal (🔒 Enterprise) |
| **Default branch** | The base branch for PRs (typically `main`) |
| **Features** | Enable/disable Issues, Projects, Wiki, Discussions |
| **Merge settings** | Allow merge commits, squash, rebase |
| **Auto-delete head branches** | Automatically delete branches after merge |

> 💡 **Internal repositories** are visible to all members of the Enterprise. This visibility option is only available with an Enterprise contract (🔒 Enterprise).

#### Branch Protection Rules

Protect important branches by requiring checks before merging.

| Rule | Description |
|------|-------------|
| Require pull request reviews | PRs must be reviewed before merge |
| Require status checks | CI must pass before merge |
| Require signed commits | Commits must be cryptographically signed |
| Require linear history | No merge commits (rebase/squash only) |
| Include administrators | Rules apply even to admins |
| Restrict who can push | Limit who can push to the branch |

#### Repository Rulesets

Rulesets are a more modern and flexible alternative to branch protection rules. They can target branches, tags, or pushes.

```
Ruleset: "production-protection"
├── Target: branches matching "main", "release/*"
├── Rules:
│   ├── Require pull request (2 approvals)
│   ├── Require status checks (CI/CD)
│   ├── Block force pushes
│   └── Require signed commits
└── Bypass: Organization admins only
```

> 💡 Rulesets can be created at both the repository and organization level. Organization-level rulesets (🔒 Enterprise) can enforce rules across all repositories.

#### Danger Zone

| Action | Description | Reversible? |
|--------|-------------|:-----------:|
| Change visibility | Public ↔ Private | ✅ |
| Transfer ownership | Move to another owner/org | ✅ |
| Archive | Make read-only | ✅ |
| Delete | Permanently delete repository | ❌ |

### 1.4 CODEOWNERS

The `CODEOWNERS` file automatically assigns reviewers when specific files are changed.

```
# .github/CODEOWNERS

# Default owners for everything
* @org/tech-leads

# Frontend team owns UI files
/src/components/ @org/frontend-team
*.css @org/frontend-team
*.tsx @org/frontend-team

# Backend team owns API files
/src/api/ @org/backend-team

# DevOps team owns infrastructure
/.github/ @org/devops-team
Dockerfile @org/devops-team
```

### ✅ Hands-on: Review Repository Settings

1. Go to one of your repositories
2. Click **Settings** tab
3. Explore: General, Branches, Collaborators & Teams
4. Review the **Danger Zone** section

---

## Part 2: Organization Administration (20 min)

### 2.1 What is an Organization?

An **Organization** is a shared account where teams can collaborate across many projects. Organizations provide:

- Centralized management of repositories
- Team-based access control
- Shared settings and policies
- Billing management

```
Organization: "my-company"
├── Teams
│   ├── @my-company/engineering (Write to all repos)
│   ├── @my-company/frontend (Write to frontend repos)
│   ├── @my-company/backend (Write to backend repos)
│   └── @my-company/security (Security manager role)
├── Repositories
│   ├── web-app (Private)
│   ├── api-server (Private)
│   ├── docs (Public)
│   └── infrastructure (Private)
└── Settings
    ├── Member privileges
    ├── Security policies
    └── Billing
```

### 2.2 Organization Roles

| Role | Description | Available On |
|------|-------------|-------------|
| **Owner** | Full administrative access | All plans |
| **Member** | Default role, can create repos | All plans |
| **Billing manager** | Manage billing only | All plans |
| **Moderator** | Block users, manage comments | All plans |
| **Security manager** | View/manage security alerts across all repos | GitHub Team / Enterprise |
| **Outside collaborator** | Access to specific repos, not a member | All plans |
| **Custom roles** | Define custom permission sets | 🔒 Enterprise |

> ⚠️ **Custom organization roles** are only available with an Enterprise contract (🔒 Enterprise).

### 2.3 Teams

Teams are groups of organization members that provide an efficient way to manage access.

#### Team Features

| Feature | Description |
|---------|-------------|
| **Nested teams** | Create sub-teams within parent teams |
| **Team mentions** | `@org/team-name` in issues and PRs |
| **Team discussions** | Internal team communication |
| **Code review assignment** | Auto-assign team members for reviews |
| **IdP sync** | Sync teams with identity provider groups (🔒 Enterprise) |

#### Creating a Team

1. Go to Organization → **Teams** tab
2. Click **New team**
3. Set team name, description, and visibility
4. Add members
5. Grant repository access with appropriate role

### 2.4 Organization Settings

#### Member Privileges

| Setting | Description |
|---------|-------------|
| **Base permissions** | Default role for all members on all repos (None, Read, Write, Admin) |
| **Repository creation** | Allow members to create repos (Public, Private, or both) |
| **Repository forking** | Allow/disallow forking of private repos |
| **Pages creation** | Control who can publish GitHub Pages |
| **Outside collaborators** | Require owner approval for adding outsiders |

> 💡 Best practice: Set base permissions to **Read** or **None** and manage access through teams.

#### Actions & Packages Settings

| Setting | Description |
|---------|-------------|
| **Actions permissions** | Allow all, select, or disable GitHub Actions |
| **Required workflows** | Force specific workflows on all repos (🔒 Enterprise) |
| **Self-hosted runners** | Manage shared runners across repos |
| **Packages access** | Control package visibility and access |

#### Security Settings

| Setting | Description |
|---------|-------------|
| **2FA requirement** | Require two-factor authentication for all members |
| **SAML SSO** | Single sign-on with identity provider (🔒 Enterprise) |
| **SCIM provisioning** | Automatic user provisioning from IdP (🔒 Enterprise) |
| **IP allow list** | Restrict access by IP address (🔒 Enterprise) |
| **Security configurations** | Apply security settings across repos at once |

### 2.5 Audit Log

The audit log records actions taken in your organization. It helps with security investigations and compliance.

**Accessible via:** Organization → **Settings** → **Audit log**

| Feature | Free/Team | Enterprise |
|---------|:---------:|:----------:|
| View audit log (web) | ✅ | ✅ |
| Search and filter | ✅ | ✅ |
| Export (JSON/CSV) | ✅ | ✅ |
| Audit log streaming | ❌ | 🔒 Enterprise |
| API access | ✅ | ✅ |

> 🔒 **Audit log streaming** to external SIEM tools (Splunk, Azure, S3, etc.) requires an Enterprise contract.

### 2.6 Organization-level Rulesets

Rulesets created at the organization level apply across multiple repositories.

| Feature | Free/Team | Enterprise |
|---------|:---------:|:----------:|
| Repository-level rulesets | ✅ | ✅ |
| Organization-level rulesets | ❌ | 🔒 Enterprise |
| Rule insights (evaluation mode) | ❌ | 🔒 Enterprise |

### ✅ Hands-on: Explore Organization Settings

1. Go to your organization (or create one at github.com/organizations/new)
2. Click **Settings**
3. Explore: **Member privileges**, **Teams**, **Security**
4. Review the **Audit log** (if available)

**Create a Team:**

1. Organization → **Teams** → **New team**
2. Name: `developers`
3. Add yourself as a member
4. Grant the team access to a repository

---

## Part 3: Enterprise Administration (15 min)

### 3.1 What is GitHub Enterprise?

> 🔒 **Enterprise contract required** — Everything in this section requires purchasing GitHub Enterprise Cloud or GitHub Enterprise Server.

GitHub Enterprise provides top-level governance for organizations that need advanced security, compliance, and management features.

```
┌─────────────────────────────────────────────────────────┐
│  Enterprise Account (🔒 Enterprise contract)             │
│                                                          │
│  ├── Policies (enforced across all orgs)                 │
│  ├── Billing (centralized)                               │
│  ├── Identity (SAML SSO, SCIM, EMU)                     │
│  ├── Audit log streaming                                 │
│  ├── IP allow list                                       │
│  └── Cost centers                                        │
│                                                          │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐        │
│  │ Org: CoA    │  │ Org: CoB    │  │ Org: CoC   │        │
│  │ 50 repos   │  │ 20 repos   │  │ 30 repos   │        │
│  └────────────┘  └────────────┘  └────────────┘        │
└─────────────────────────────────────────────────────────┘
```

### 3.2 GitHub Plans Comparison

| Feature | Free | Team | Enterprise 🔒 |
|---------|:----:|:----:|:----------:|
| Public repositories | ∞ | ∞ | ∞ |
| Private repositories | ∞ | ∞ | ∞ |
| Collaborators | ∞ | ∞ | ∞ |
| Actions minutes/month | 2,000 | 3,000 | 50,000 |
| Packages storage | 500 MB | 2 GB | 50 GB |
| Required PR reviewers | ❌ | ✅ | ✅ |
| Code owners | ❌ | ✅ | ✅ |
| Protected branches | ❌ | ✅ | ✅ |
| Internal repositories | ❌ | ❌ | 🔒 Enterprise |
| SAML SSO | ❌ | ❌ | 🔒 Enterprise |
| SCIM provisioning | ❌ | ❌ | 🔒 Enterprise |
| Enterprise Managed Users | ❌ | ❌ | 🔒 Enterprise |
| Organization-level rulesets | ❌ | ❌ | 🔒 Enterprise |
| Audit log streaming | ❌ | ❌ | 🔒 Enterprise |
| IP allow list | ❌ | ❌ | 🔒 Enterprise |
| GitHub Connect | ❌ | ❌ | 🔒 Enterprise |
| 99.9% SLA | ❌ | ❌ | 🔒 Enterprise |
| Data residency | ❌ | ❌ | 🔒 Enterprise |

> 💰 For the latest pricing, see the [GitHub Pricing page](https://github.com/pricing).

### 3.3 Enterprise Policies (🔒 Enterprise)

Enterprise administrators can set policies that are enforced across **all organizations** in the enterprise.

| Policy Area | Examples |
|-------------|---------|
| **Repository** | Control creation, deletion, visibility, transfer, forking |
| **Branch/Tag** | Enforce rulesets across all organizations |
| **Actions** | Restrict which actions can be used, manage runner groups |
| **Copilot** | Enable/disable features, control model access |
| **Code security** | Enforce security configurations organization-wide |
| **IP allow list** | Restrict access to enterprise resources by IP |
| **Authentication** | SAML SSO, SCIM, session policies |

```
Enterprise Policy: "Require 2FA"
    ↓
Enforced on ALL organizations:
    ├── Org: Development ✅ 2FA required
    ├── Org: QA          ✅ 2FA required
    └── Org: Operations  ✅ 2FA required
```

### 3.4 Identity and Access Management (🔒 Enterprise)

#### SAML Single Sign-On (SSO)

SAML SSO integrates GitHub with your company's identity provider (Entra ID, Okta, etc.).

```
Developer → GitHub login → Redirected to IdP → Authenticate → Access granted
```

| Feature | Description |
|---------|-------------|
| **SAML SSO** | Single sign-on through corporate IdP |
| **SCIM** | Automatic user provisioning/deprovisioning |
| **Team sync** | Sync GitHub teams with IdP groups |

#### Enterprise Managed Users (EMU)

EMU provides the highest level of identity control. User accounts are **fully managed** by the enterprise — provisioned, controlled, and deleted by the IdP.

| Standard Enterprise | Enterprise with EMU |
|:-------------------:|:-------------------:|
| Users have personal GitHub accounts | Users have enterprise-managed accounts |
| Users can contribute to public repos | Users can only contribute within the enterprise |
| Users manage their own profiles | Profiles managed by IdP |
| SSO adds a linked identity | IdP is the sole identity source |

### 3.5 Enterprise Roles (🔒 Enterprise)

| Role | Description |
|------|-------------|
| **Enterprise owner** | Full administrative access to all settings |
| **Enterprise member** | Default role for all users in the enterprise |
| **Billing manager** | Manage billing and cost centers |
| **Guest collaborator** | Limited access to specific repositories |

### 3.6 Deployment Options (🔒 Enterprise)

| Option | Description |
|--------|-------------|
| **GitHub Enterprise Cloud** | Hosted by GitHub on github.com |
| **GitHub Enterprise Cloud with data residency** | Hosted on a dedicated subdomain (e.g., your-company.ghe.com) |
| **GitHub Enterprise Server** | Self-hosted on your own infrastructure |
| **GitHub Connect** | Connect Enterprise Server to Enterprise Cloud |

### 3.7 Billing and Cost Centers (🔒 Enterprise)

Enterprise accounts centralize billing across all organizations.

| Feature | Description |
|---------|-------------|
| **Centralized billing** | One bill for all organizations |
| **Cost centers** | Allocate spending to business units |
| **Azure billing** | Bill usage to Azure subscriptions |
| **Visual Studio subscriptions** | Include GitHub Enterprise as part of VS Enterprise |
| **Spending limits** | Set limits for Actions, Packages, Codespaces |

### ✅ Hands-on: Understand Your Admin Scope

Based on your current role, complete the relevant exercises:

**For Repository Admins:**
1. Go to a repository → **Settings**
2. Review **Branches** → Add a branch protection rule for `main`
3. Create a `CODEOWNERS` file

**For Organization Owners:**
1. Go to Organization → **Settings**
2. Review **Member privileges** and set base permissions
3. Create a team and assign repository access
4. Review the **Audit log**

**For those evaluating Enterprise:**
1. Visit [GitHub Enterprise](https://github.com/enterprise) to understand the offering
2. Review the [Enterprise Cloud trial](https://docs.github.com/en/enterprise-cloud@latest/admin/overview/setting-up-a-trial-of-github-enterprise-cloud) option
3. List which Enterprise features your organization would benefit from

---

## Wrap Up (5 min)

### What We Learned Today

- ✅ Three levels of GitHub administration (Repository → Organization → Enterprise)
- ✅ Repository roles, settings, branch protection, and rulesets
- ✅ Organization roles, teams, and member management
- ✅ Enterprise-only features (SAML SSO, SCIM, EMU, policies, audit log streaming)
- ✅ GitHub plans comparison and deployment options

### Administration Checklist

| Level | Task | Status |
|-------|------|--------|
| **Repository** | Set branch protection rules | ☐ |
| **Repository** | Create CODEOWNERS file | ☐ |
| **Repository** | Review danger zone settings | ☐ |
| **Organization** | Set base permissions appropriately | ☐ |
| **Organization** | Create teams for access management | ☐ |
| **Organization** | Require 2FA for all members | ☐ |
| **Organization** | Review audit log regularly | ☐ |
| **Enterprise** 🔒 | Configure SAML SSO | ☐ |
| **Enterprise** 🔒 | Set up SCIM provisioning | ☐ |
| **Enterprise** 🔒 | Define enterprise-wide policies | ☐ |
| **Enterprise** 🔒 | Configure audit log streaming | ☐ |

### Features by Plan Quick Reference

| Feature | Free | Team | Enterprise 🔒 |
|---------|:----:|:----:|:----------:|
| Repository settings | ✅ | ✅ | ✅ |
| Branch protection | Basic | ✅ | ✅ |
| Organization management | ✅ | ✅ | ✅ |
| Teams | ✅ | ✅ | ✅ |
| Required PR reviews | ❌ | ✅ | ✅ |
| Code owners | ❌ | ✅ | ✅ |
| Security manager role | ❌ | ✅ | ✅ |
| Custom org roles | ❌ | ❌ | 🔒 Enterprise |
| Internal repos | ❌ | ❌ | 🔒 Enterprise |
| SAML SSO | ❌ | ❌ | 🔒 Enterprise |
| SCIM / EMU | ❌ | ❌ | 🔒 Enterprise |
| Enterprise policies | ❌ | ❌ | 🔒 Enterprise |
| Audit log streaming | ❌ | ❌ | 🔒 Enterprise |
| IP allow list | ❌ | ❌ | 🔒 Enterprise |

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

- 📋 **Audit your current settings** — Review repository and organization configurations
- 👥 **Organize teams** — Set up team-based access control
- 🔒 **Evaluate Enterprise** — Consider if your organization needs Enterprise features
- 📖 **GitHub Enterprise docs** — [docs.github.com/enterprise-cloud](https://docs.github.com/en/enterprise-cloud@latest)
- 🎓 **GitHub Certifications** — Consider the [GitHub Administration certification](https://resources.github.com/learn/certifications/)

---

## 📚 Reference Links

- [Managing Repository Settings](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features)
- [About Organizations](https://docs.github.com/en/organizations)
- [Roles in an Organization](https://docs.github.com/en/organizations/managing-peoples-access-to-your-organization-with-roles/roles-in-an-organization)
- [About Enterprise Accounts](https://docs.github.com/en/enterprise-cloud@latest/admin/managing-your-enterprise-account/about-enterprise-accounts)
- [About Enterprise Managed Users](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/understanding-iam-for-enterprises/about-enterprise-managed-users)
- [GitHub Plans](https://docs.github.com/en/get-started/learning-about-github/githubs-plans)
- [GitHub Pricing](https://github.com/pricing)
