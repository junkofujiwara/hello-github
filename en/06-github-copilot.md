# Workshop 6: GitHub Copilot DeepDive (60 min)

> 📖 [日本語版](../ja/06-github-copilot.md)

## 🎯 Workshop Goals

- Understand the overview and capabilities of GitHub Copilot
- Enable and set up GitHub Copilot (Free plan)
- Use Copilot Chat for interactive coding
- Understand Copilot Agent (Coding Agent) concepts and usage
- Learn self-paced learning with GitHub Skills

---

## 📋 Agenda

| Time | Content |
|------|---------|
| 0:00 - 0:05 | Review of Workshop 5 |
| 0:05 - 0:15 | GitHub Copilot overview |
| 0:15 - 0:25 | Enabling Copilot and setup |
| 0:25 - 0:40 | Copilot Chat hands-on |
| 0:40 - 0:55 | Copilot Agent & Skills |
| 0:55 - 1:00 | Wrap Up |

---

## Review of Workshop 5 (5 min)

In Workshop 5, we learned CI/CD and automation with GitHub Actions.  
In this final workshop, we'll explore **GitHub Copilot** — an AI-powered development assistant.

---

## Part 1: GitHub Copilot Overview (10 min)

### 1.1 What is GitHub Copilot?

**GitHub Copilot** is an AI-powered development tool. It provides code auto-completion, interactive coding through chat, code explanations, fix suggestions, and much more.

```
┌─────────────────────────────────────────────────┐
│              GitHub Copilot                      │
│                                                  │
│  💬 Chat          ✍️ Code Completion              │
│  Ask questions     Auto-complete code            │
│                                                  │
│  🤖 Agent         🛠️ Skills                      │
│  Autonomous tasks  Extensions & tool integration │
│                                                  │
│  📝 Code Review   🔍 Explain                     │
│  Review assistance Code explanation              │
└─────────────────────────────────────────────────┘
```

### 1.2 Feature Overview

| Feature | Description | Where to Use |
|---------|-------------|-------------|
| **Code Completion** | Auto-complete and suggest code | In the editor |
| **Copilot Chat** | Ask questions in natural language | VS Code, GitHub.com |
| **Copilot Agent** | Autonomously execute tasks | VS Code, GitHub.com |
| **Code Review** | AI-assisted PR reviews | GitHub.com |
| **Copilot in CLI** | Command-line assistance | Terminal |
| **Skills / Extensions** | External tools & MCP integration | VS Code |

### 1.3 GitHub Copilot Free

GitHub Copilot offers several plans. With a **free GitHub account**, you can use **Copilot Free**.

| Plan | Price | Key Features |
|------|-------|-------------|
| **Copilot Free** | Free | Code completion (monthly limit), Chat, Agent (limited) |
| **Copilot Pro** | Paid | Unlimited completions, advanced model selection |
| **Copilot Business** | Paid | Organization-level, policy management |
| **Copilot Enterprise** | Paid | Customization, Knowledge Base |

> 💰 For the latest pricing, see the [GitHub Copilot pricing page](https://github.com/features/copilot#pricing).

> 💡 This workshop uses **Copilot Free**. If you have a free GitHub account, you can get started at no additional cost.

### 1.4 Supported Editors and Environments

| Environment | Support |
|-------------|---------|
| **Visual Studio Code** | ✅ Full support (recommended) |
| **Visual Studio** | ✅ Supported |
| **JetBrains IDEs** | ✅ Supported |
| **Neovim** | ✅ Supported |
| **GitHub.com** | ✅ Chat, Agent |
| **GitHub Mobile** | ✅ Chat |
| **CLI (Terminal)** | ✅ Copilot in CLI |

---

## Part 2: Enabling Copilot and Setup (10 min)

### 2.1 Enable Copilot Free

1. Sign in to [github.com](https://github.com)
2. Click your profile icon (top right) → **Settings**
3. Left sidebar → **Copilot**
4. Select **GitHub Copilot Free** and enable it
5. Accept the terms of service

> 💡 If you already have Copilot Pro/Business/Enterprise, you can skip this step.

### 2.2 Install the VS Code Extension

#### 🪟 Windows / 🍎 Mac (Common)

1. Open VS Code
2. Open the Extensions panel (`Ctrl+Shift+X` / `Cmd+Shift+X`)
3. Search for and install:

| Extension | Description |
|-----------|-------------|
| **GitHub Copilot** | Code completion, Chat, Agent |

4. After installation, verify the Copilot icon appears in the bottom-right of VS Code

### 2.3 Authenticate with GitHub

1. Click the **Copilot icon** at the bottom of VS Code
2. Select **Sign in to GitHub**
3. A browser window opens — sign in to GitHub
4. Authorize the connection
5. Return to VS Code and verify Copilot is active

### 2.4 Verify It Works

Create a new file in VS Code to confirm code completion works:

1. Create a new file (e.g., `test.js`)
2. Start typing:

```javascript
// A function that adds two numbers
function add(
```

3. Verify that Copilot shows a gray suggestion
4. Press `Tab` to accept the completion

### ✅ Hands-on: Setup Verification

- [ ] Copilot Free is enabled
- [ ] GitHub Copilot extension is installed in VS Code
- [ ] Authenticated with your GitHub account
- [ ] Code completion is working

---

## Part 3: Copilot Chat Hands-on (15 min)

### 3.1 What is Copilot Chat?

**Copilot Chat** lets you interact with AI through natural language while coding. It supports a wide range of tasks including asking questions, generating code, debugging, and refactoring.

### 3.2 Opening Chat

| Method | Shortcut |
|--------|----------|
| **Chat panel** | `Ctrl+Alt+I` (Windows) / `Cmd+Alt+I` (Mac) |
| **Inline chat** | `Ctrl+I` (Windows) / `Cmd+I` (Mac) |
| **Quick chat** | `Ctrl+Shift+I` (Windows) / `Cmd+Shift+I` (Mac) |

### 3.3 Basic Usage

#### Ask a Question

```
Explain Git branching strategies
```

#### Generate Code

```
Create a responsive header with a navigation bar in HTML
```

#### Explain Code

With a file open:
```
Explain what this code does
```

#### Fix an Error

```
What causes this error and how do I fix it: TypeError: Cannot read property 'map' of undefined
```

### 3.4 Chat Participants

Use `@` in chat to target specific participants:

| Participant | Description | Example |
|-------------|-------------|---------|
| `@workspace` | Use the entire workspace as context | `@workspace Explain this project structure` |
| `@vscode` | VS Code settings and operations | `@vscode How do I switch to a dark theme?` |
| `@terminal` | Use terminal output as context | `@terminal What caused this error?` |

### 3.5 Slash Commands

| Command | Description | Example |
|---------|-------------|---------|
| `/explain` | Explain code | `/explain What does this function do?` |
| `/fix` | Fix code | `/fix Fix this bug` |
| `/tests` | Generate tests | `/tests Create unit tests for this function` |
| `/new` | Create new project/file | `/new Create an Express server` |
| `/doc` | Generate documentation | `/doc Add documentation to this function` |

### 3.6 Using Context

#### File References

Use `#` in chat to reference files:

```
Suggest improvements for the styling in #index.html
```

#### Selection Context

1. Select code in the editor
2. Open Copilot Chat
3. The selected code is automatically used as context

### ✅ Hands-on: Experience Copilot Chat

**Exercise 1: Generate an HTML Page**

Ask Copilot Chat:

```
Generate HTML code to add a GitHub Copilot introduction section to the index.html file in the hello-github repository. Use a dark theme with a card-style layout.
```

**Exercise 2: Explain Code**

Open the GitHub Actions workflow file created in Workshop 5:

```
/explain Explain each step of this workflow
```

**Exercise 3: Fix an Error**

Intentionally type buggy code and ask Copilot to fix it:

```javascript
// This code has a bug
function greet(name) {
    console.log("Hello, " + nane);
}
greet();
```

```
/fix Fix the bugs in this code
```

---

## Part 4: Copilot Agent & Skills (15 min)

### 4.1 What is Copilot Agent?

**Copilot Agent (Coding Agent)** is a feature where Copilot autonomously plans and executes tasks. Unlike simple completions or chat, it can make changes across multiple files and break down tasks automatically.

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  User        │────▶│  Copilot     │────▶│  Result      │
│  Request     │     │  Agent       │     │              │
│              │     │              │     │ - File edits  │
│ "Add auth    │     │ - Planning   │     │ - Tests added │
│  feature"    │     │ - Code gen   │     │ - PR created  │
│              │     │ - Run tests  │     │              │
└──────────────┘     └──────────────┘     └──────────────┘
```

### 4.2 How to Use Agent

#### In VS Code

1. Open Copilot Chat
2. Switch chat mode to **Agent** (select from dropdown)
3. Describe your task in natural language

```
Add a CSS file to this project and link it to index.html.
Make it responsive and support dark mode.
```

#### On GitHub.com (Copilot Coding Agent)

1. Go to your repository on GitHub.com and create an Issue
2. Assign Copilot to the Issue (set Assignee to `copilot`)
3. Copilot automatically creates a branch, makes code changes, and opens a PR

```
Issue Title: Add a contact form to the website
Issue Body:
- Create a contact form with name, email, and message fields
- Add form validation
- Style with CSS to match existing design
```

### 4.3 Agent vs Chat

| Aspect | Chat | Agent |
|--------|------|-------|
| **Scope** | Single question/answer | Multi-step tasks |
| **File operations** | Suggestions only | Direct edits |
| **Autonomy** | User directs each step | Plans and executes autonomously |
| **Terminal** | No | Can run commands |
| **Multi-file** | Context reference | Simultaneous multi-file edits |

### 4.4 What are Skills?

**Skills (Extensions / MCP)** extend Copilot's capabilities. They integrate with external tools, APIs, and data sources to perform more advanced tasks.

#### MCP (Model Context Protocol)

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Copilot     │────▶│  MCP Server  │────▶│  External    │
│  Agent       │     │              │     │  Service     │
│              │     │  - DB access  │     │              │
│              │◀────│  - API calls  │◀────│  - Database  │
│              │     │  - File I/O   │     │  - APIs      │
└──────────────┘     └──────────────┘     └──────────────┘
```

MCP is an open protocol that connects AI models to external tools.

| Use Case | Example |
|----------|---------|
| **Database access** | Generate and execute SQL |
| **API integration** | Call REST APIs |
| **File system** | Read and write files |
| **Browser** | Fetch web pages |
| **Custom tools** | Your own tool integrations |

#### VS Code Configuration Example

Create `.vscode/mcp.json`:

```json
{
  "servers": {
    "my-mcp-server": {
      "type": "stdio",
      "command": "node",
      "args": ["path/to/mcp-server.js"]
    }
  }
}
```

### 4.5 Learn Copilot with GitHub Skills

[GitHub Skills](https://skills.github.com/) offers interactive courses for Copilot.

| Course | Content |
|--------|---------|
| **Code with GitHub Copilot** | Basic Copilot usage |
| **Copilot Autofix** | Auto-fix security vulnerabilities |
| **Build and deploy** | Build apps with Copilot |

> 💡 Each GitHub Skills course is provided as a repository — you learn by actually writing code.

### ✅ Hands-on: Experience Agent

**Exercise 1: Generate Files with Agent Mode**

Switch Copilot Chat to **Agent** mode in VS Code and ask:

```
Create an about.html file in the hello-github repository.
The page should be "About the GitHub Workshop."
Use the same styling as index.html and include an overview of the workshop series.
```

Review the plan Agent proposes, then approve and execute.

**Exercise 2: Assign an Issue to Copilot on GitHub.com (Demo)**

> ⚠️ Some features may be limited on Copilot Free. Follow the instructor's demo.

1. Open the repository on GitHub.com
2. Create a new Issue
3. Observe Copilot's workflow

**Exercise 3: Prompt Engineering Practice**

Practice writing better prompts for better results:

| ❌ Vague Prompt | ✅ Good Prompt |
|----------------|---------------|
| Make a page | Add a navigation bar to index.html with links to Home, About, and Contact. Make it responsive. |
| Fix the bug | Fix the TypeError on line 23. Use a default value when name is undefined. |
| Write tests | Create unit tests for the add function. Include normal cases (positive, negative, zero) and edge cases (string input). |

---

## Wrap Up (5 min)

### What We Learned Today

- ✅ GitHub Copilot overview and capabilities
- ✅ Enabling Copilot Free and VS Code setup
- ✅ Interactive coding with Copilot Chat
- ✅ Slash commands and context usage
- ✅ Copilot Agent concepts and usage
- ✅ Skills / MCP for extending capabilities
- ✅ Self-paced learning with GitHub Skills

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

- 🧪 **GitHub Skills** — Take interactive courses
- 🤖 **Copilot** — Integrate into your daily dev workflow
- 🔌 **MCP** — Build custom MCP servers for tool integration
- 🏢 **Team adoption** — Consider Copilot Business / Enterprise
- 📖 **GitHub Universe** and **GitHub Blog** — Stay up to date

---

## 📚 Reference Links

- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [About GitHub Copilot Free](https://docs.github.com/en/copilot/about-github-copilot/github-copilot-free)
- [Copilot in VS Code](https://code.visualstudio.com/docs/copilot/overview)
- [GitHub Skills](https://skills.github.com/)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
- [GitHub Copilot Trust Center](https://resources.github.com/copilot-trust-center/)
