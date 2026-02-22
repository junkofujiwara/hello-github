# Workshop 7: Release & Deployment DeepDive (60 min)

> 📖 [日本語版](../ja/07-release-and-deployment.md)

## 🎯 Workshop Goals

- Understand Git tags and GitHub Releases for version management
- Learn how to auto-generate release notes
- Set up GitHub Pages to host static sites
- Use GitHub Packages to publish and manage packages
- Know the differences between free and paid features

---

## 📋 Agenda

| Time | Content |
|------|---------|
| 0:00 - 0:05 | Review of Workshop 6 |
| 0:05 - 0:20 | Tags & Releases |
| 0:20 - 0:35 | GitHub Pages |
| 0:35 - 0:50 | GitHub Packages |
| 0:50 - 1:00 | Wrap Up |

---

## Review of Workshop 6 (5 min)

In Workshop 6, we learned about AI-powered development with GitHub Copilot.  
In this workshop, we'll focus on **Release & Deployment** — how to version, publish, and distribute your projects using GitHub's built-in features.

---

## Part 1: Tags & Releases (15 min)

### 1.1 What Are Git Tags?

A **tag** is a pointer to a specific commit. Tags are used to mark important points in a project's history — typically release versions.

```
main: A ── B ── C ── D ── E ── F
                ↑              ↑
             v1.0.0         v2.0.0
             (tag)           (tag)
```

#### Two Types of Tags

| Type | Description | Use Case |
|------|-------------|----------|
| **Lightweight** | Just a pointer to a commit | Temporary or private markers |
| **Annotated** | Full object with tagger, date, message | Release versions (recommended) |

#### Creating Tags

##### 🪟 Windows (PowerShell) / 🍎 Mac (Terminal)

```bash
# Annotated tag (recommended for releases)
git tag -a v1.0.0 -m "Release version 1.0.0"

# Lightweight tag
git tag v1.0.0-beta

# Tag a specific commit
git tag -a v1.0.0 -m "Release version 1.0.0" abc1234

# Push tags to remote
git push origin v1.0.0        # Push one tag
git push origin --tags         # Push all tags

# List tags
git tag -l
git tag -l "v1.*"              # Filter by pattern
```

### 1.2 Semantic Versioning

The convention for version numbers is **Semantic Versioning (SemVer)**:

```
v MAJOR . MINOR . PATCH
  ↓        ↓       ↓
  Breaking  New    Bug
  changes  features fixes

Examples:
  v1.0.0 → v1.0.1  (patch: bug fix)
  v1.0.0 → v1.1.0  (minor: new feature, backward compatible)
  v1.0.0 → v2.0.0  (major: breaking change)
```

Pre-release versions: `v1.0.0-alpha`, `v1.0.0-beta.1`, `v1.0.0-rc.1`

### 1.3 GitHub Releases

A **Release** is a GitHub feature built on top of Git tags. Releases add:

- Release title and description (release notes)
- Binary assets (downloadable files)
- Mark as pre-release or latest
- Discussion links

```
Git Tag (v1.0.0)
    ↓
GitHub Release
├── Title: "Version 1.0.0 - Initial Release"
├── Release notes (changelog)
├── Assets: app-v1.0.0.zip, app-v1.0.0.tar.gz
├── Pre-release: No
└── Latest: Yes
```

#### Creating a Release

1. Go to your repository → **Releases** (right sidebar)
2. Click **Draft a new release**
3. Choose or create a tag (e.g., `v1.0.0`)
4. Set release title
5. Write release notes (or auto-generate)
6. Upload binary assets if needed
7. Click **Publish release**

### 1.4 Auto-Generated Release Notes

GitHub can automatically generate release notes from merged PRs and contributors.

**How it works:**
1. When creating a release, click **Generate release notes**
2. GitHub lists all merged PRs since the last release
3. Categorized by labels (features, bug fixes, etc.)

#### Customizing Auto-Generation

Create `.github/release.yml`:

```yaml
changelog:
  exclude:
    labels:
      - ignore-for-release
    authors:
      - dependabot
  categories:
    - title: 🚀 New Features
      labels:
        - enhancement
        - feature
    - title: 🐛 Bug Fixes
      labels:
        - bug
        - fix
    - title: 📖 Documentation
      labels:
        - documentation
    - title: 🔧 Maintenance
      labels:
        - chore
        - dependencies
    - title: Other Changes
      labels:
        - "*"
```

### 1.5 Automating Releases with GitHub Actions

```yaml
# .github/workflows/release.yml
name: Create Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Create Release
        uses: softprops/action-gh-release@v2
        with:
          generate_release_notes: true
          draft: false
          prerelease: ${{ contains(github.ref, '-') }}
```

### ✅ Hands-on: Create a Release

1. Create a tag:
   ```bash
   git tag -a v0.1.0 -m "First release"
   git push origin v0.1.0
   ```
2. Go to **Releases** → **Draft a new release**
3. Select the tag `v0.1.0`
4. Click **Generate release notes**
5. Publish the release

---

## Part 2: GitHub Pages (15 min)

### 2.1 What is GitHub Pages?

**GitHub Pages** is a free static site hosting service built into GitHub. It can serve HTML, CSS, and JavaScript directly from a repository.

```
Repository (source files)
    ↓
GitHub Pages (builds & serves)
    ↓
https://username.github.io/repo-name/
```

#### Availability

| Feature | Free | GitHub Team | Enterprise |
|---------|:----:|:-----------:|:----------:|
| Public repos → public Pages | ✅ | ✅ | ✅ |
| Private repos → public Pages | ✅ | ✅ | ✅ |
| Private repos → private Pages | ❌ | ❌ | 🔒 Enterprise |

> 💡 **Private GitHub Pages** (access control) requires GitHub Enterprise Cloud (🔒 Enterprise).

### 2.2 Setting Up GitHub Pages

#### Method 1: From Settings (Quick)

1. **Settings** → **Pages**
2. **Source**: Choose branch and folder
   - Branch: `main` (or `gh-pages`)
   - Folder: `/ (root)` or `/docs`
3. Click **Save**
4. Wait for deployment → your site is live!

#### Method 2: Using GitHub Actions (Recommended)

```yaml
# .github/workflows/pages.yml
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
        uses: actions/configure-pages@v5

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: './public'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 2.3 Types of GitHub Pages Sites

| Type | URL | Repository Name |
|------|-----|-----------------|
| **User site** | `username.github.io` | `username.github.io` |
| **Organization site** | `orgname.github.io` | `orgname.github.io` |
| **Project site** | `username.github.io/repo` | Any repository |

### 2.4 Static Site Generators

GitHub Pages works with any static site generator:

| Generator | Language | Use Case |
|-----------|----------|----------|
| **Jekyll** | Ruby | Blog, docs (built-in support) |
| **Hugo** | Go | Fast builds, large sites |
| **Next.js** | JavaScript | React-based static export |
| **VitePress** | JavaScript | Vue-based documentation |
| **MkDocs** | Python | Technical documentation |

> 💡 Jekyll has built-in support on GitHub Pages — no Actions workflow needed. Other generators require a GitHub Actions workflow to build and deploy.

### 2.5 Custom Domains

You can use your own domain with GitHub Pages:

1. **Settings** → **Pages** → **Custom domain**
2. Enter your domain (e.g., `docs.example.com`)
3. Add DNS records:

| Type | Name | Value |
|------|------|-------|
| CNAME | `docs` | `username.github.io` |
| A | `@` | `185.199.108.153` (+ 109, 110, 111) |

4. Enable **Enforce HTTPS** ✅

### ✅ Hands-on: Deploy a GitHub Pages Site

1. Create a file `index.html` in the root of your repository:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My GitHub Pages Site</title>
    <style>
        body { font-family: sans-serif; max-width: 800px; margin: 2rem auto; padding: 0 1rem; }
        h1 { color: #0366d6; }
    </style>
</head>
<body>
    <h1>🎉 Hello, GitHub Pages!</h1>
    <p>This site is hosted on GitHub Pages.</p>
    <p>Repository: <a href="https://github.com/USERNAME/REPO">GitHub</a></p>
</body>
</html>
```

2. Push to `main`
3. **Settings** → **Pages** → Source: `main` branch, `/ (root)` folder
4. Wait 1-2 minutes and visit `https://username.github.io/repo-name/`

---

## Part 3: GitHub Packages (15 min)

### 3.1 What is GitHub Packages?

**GitHub Packages** is a package registry integrated with GitHub. You can publish, store, and consume packages alongside your source code.

```
Source Code (Repository)
    ↓ build & publish
GitHub Packages (Registry)
    ↓ install / pull
Consumers (other projects, CI/CD, users)
```

### 3.2 Supported Registries

| Registry | Ecosystem | URL |
|----------|-----------|-----|
| **npm** | JavaScript / Node.js | `npm.pkg.github.com` |
| **Maven** | Java | `maven.pkg.github.com` |
| **Gradle** | Java | `maven.pkg.github.com` |
| **NuGet** | .NET | `nuget.pkg.github.com` |
| **RubyGems** | Ruby | `rubygems.pkg.github.com` |
| **Container (Docker)** | Docker / OCI images | `ghcr.io` |

### 3.3 Storage & Pricing

| Plan | Storage | Data transfer |
|------|---------|--------------|
| **Free** | 500 MB | 1 GB / month |
| **Pro** | 2 GB | 10 GB / month |
| **Team** | 2 GB | 10 GB / month |
| **Enterprise** | 50 GB | 100 GB / month |

> 💡 Public packages have **unlimited** storage and data transfer on all plans.

> 💰 For the latest pricing, see the [GitHub Pricing page](https://github.com/pricing).

### 3.4 Container Registry (ghcr.io)

The **GitHub Container Registry** (`ghcr.io`) is the most commonly used GitHub Packages registry. It hosts Docker/OCI container images.

#### Publishing a Container Image

```bash
# Login to GitHub Container Registry
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin

# Build the image
docker build -t ghcr.io/USERNAME/my-app:v1.0.0 .

# Push the image
docker push ghcr.io/USERNAME/my-app:v1.0.0

# Pull the image (from another machine)
docker pull ghcr.io/USERNAME/my-app:v1.0.0
```

#### Automating with GitHub Actions

```yaml
# .github/workflows/publish-container.yml
name: Publish Container Image

on:
  push:
    tags:
      - 'v*'

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Login to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:${{ github.ref_name }}
            ghcr.io/${{ github.repository }}:latest
```

### 3.5 Publishing an npm Package

```yaml
# .github/workflows/publish-npm.yml
name: Publish npm Package

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          registry-url: 'https://npm.pkg.github.com'
          scope: '@your-org'

      - name: Install dependencies
        run: npm ci

      - name: Publish
        run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### 3.6 Package Visibility & Access

| Setting | Public Repository | Private Repository |
|---------|:-----------------:|:------------------:|
| Public packages | ✅ | ✅ |
| Private packages | ✅ | ✅ |
| Organization access control | ✅ | ✅ |
| Inherit repo access | ✅ | ✅ |

**Managing package visibility:**
1. Go to your profile or organization → **Packages**
2. Select the package → **Package settings**
3. Change visibility (public/private)
4. Manage access (teams, collaborators)

### ✅ Hands-on: Explore GitHub Packages

**Exercise 1: Browse Packages**

1. Visit [github.com/features/packages](https://github.com/features/packages)
2. Go to your profile → **Packages** tab
3. Explore public container images at `ghcr.io`

**Exercise 2: Create a Release with Assets**

1. Create a simple project (e.g., a README + script)
2. Tag it: `git tag -a v1.0.0 -m "Initial release" && git push origin v1.0.0`
3. Go to **Releases** → Edit the release
4. Upload a ZIP file as a binary asset
5. Publish the release

**Exercise 3: Configure a Publish Workflow (Optional)**

1. Add a `Dockerfile` or `package.json` to your repo
2. Create one of the Actions workflows above
3. Push a tag to trigger the publish

---

## Wrap Up (10 min)

### What We Learned Today

- ✅ Git tags and semantic versioning
- ✅ GitHub Releases and auto-generated release notes
- ✅ Automating releases with GitHub Actions
- ✅ GitHub Pages for static site hosting
- ✅ GitHub Packages and Container Registry (ghcr.io)
- ✅ Publishing packages with CI/CD workflows

### Release & Deployment Checklist

| Task | Status |
|------|--------|
| Use semantic versioning for tags | ☐ |
| Create `.github/release.yml` for release note categories | ☐ |
| Set up automated releases with GitHub Actions | ☐ |
| Deploy a GitHub Pages site | ☐ |
| Configure custom domain for Pages (if needed) | ☐ |
| Publish packages to GitHub Packages | ☐ |
| Set up container image builds with `ghcr.io` | ☐ |

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

- 🏷️ **Start tagging your releases** with semantic versioning
- 🌐 **Deploy a documentation site** with GitHub Pages
- 📦 **Publish your first package** to GitHub Packages
- 🔄 **Automate your release workflow** with GitHub Actions
- 📖 **GitHub Skills** — [skills.github.com](https://skills.github.com/)

---

## 📚 Reference Links

- [About Releases](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)
- [Automatically Generated Release Notes](https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub Packages Documentation](https://docs.github.com/en/packages)
- [Working with the Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)
- [Semantic Versioning](https://semver.org/)
