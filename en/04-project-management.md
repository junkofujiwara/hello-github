# Workshop 4: Project Management DeepDive (60 min)

> 📖 [日本語版](../ja/04-project-management.md)

## 🎯 Workshop Goals

- Understand project management with GitHub Projects
- Master Board view, Table view, and Roadmap view
- Practice efficient task management by linking Issues and Projects
- Set up automation rules to streamline workflows

---

## 📋 Agenda

| Time | Content |
|------|---------|
| 0:00 - 0:05 | Review of Workshop 3 |
| 0:05 - 0:20 | GitHub Projects basics |
| 0:20 - 0:35 | Views and customization |
| 0:35 - 0:50 | Automation & hands-on practice |
| 0:50 - 1:00 | Wrap Up |

---

## Review of Workshop 3 (5 min)

In Workshop 3, we dived deep into Branches and Pull Requests.  
Now let's learn how to manage all of this with **GitHub Projects**.

Creating Issues, creating branches, submitting PRs — Projects lets you **see the big picture** across your entire development workflow.  
Think of it like putting sticky notes on a board to track progress.

---

## Part 1: GitHub Projects Basics (15 min)

### 1.1 What is GitHub Projects?

**GitHub Projects** is a project management tool that lets you organize Issues and Pull Requests in **Board (kanban)**, **Table (spreadsheet)**, or **Roadmap** views.  
It makes it easy to see at a glance: "Who is working on what right now?"

```
┌─────────────────────────────────────────────────────────┐
│                   GitHub Projects                        │
│                                                          │
│  📋 Todo        🔄 In Progress     ✅ Done              │
│  ┌──────────┐  ┌──────────┐      ┌──────────┐          │
│  │ Issue #5 │  │ Issue #3 │      │ Issue #1 │          │
│  │ Search   │  │ API      │      │ Setup    │          │
│  └──────────┘  └──────────┘      └──────────┘          │
│  ┌──────────┐  ┌──────────┐      ┌──────────┐          │
│  │ Issue #6 │  │ Issue #4 │      │ Issue #2 │          │
│  │ Testing  │  │ UI       │      │ DB Design│          │
│  └──────────┘  └──────────┘      └──────────┘          │
└─────────────────────────────────────────────────────────┘
```

### 1.2 Types of Projects

| Type | Description | Use Case |
|------|-------------|----------|
| **User Project** | Personal project | Personal task management, managing across repos |
| **Organization Project** | Organization project | Team/department project management |

> 💡 A single Project can include Issues from **multiple repositories** — a key advantage. Great for managing tasks across repos!

### 1.3 Creating a Project

1. Go to your profile page or the repository's **Projects** tab
2. Click **New project**
3. Choose a template:

| Template | Description |
|----------|-------------|
| **Board** | Kanban board layout |
| **Table** | Spreadsheet layout |
| **Roadmap** | Timeline layout |
| **Start from scratch** | Blank project |

4. Enter a project name
5. Click **Create project**

### 1.4 Adding Items to a Project

| Item | Description |
|------|-------------|
| **Issue** | Repository Issue |
| **Pull Request** | Repository PR |
| **Draft issue** | A note not yet converted to an Issue |

#### How to Add Items

**Method 1: From the Project view**
1. Click `+ Add item`
2. Search by repository name and select Issues/PRs

**Method 2: From an Issue page**
1. Issue sidebar → **Projects**
2. Select the Project to add to

**Method 3: Draft Issues**
1. Click `+ Add item`
2. Type text and press Enter
3. Can be converted to a real Issue later

### ✅ Hands-on: Create a Project

1. Create a new Project using the **Board template**
   - Name: `GitHub Workshop Project`
2. Add Issues created in Workshop 2 to the Project
3. Add 2 or more Draft Issues

---

## Part 2: Views and Customization (15 min)

### 2.1 View Types

| View | Description | Best For |
|------|-------------|----------|
| **Board** | Kanban board | Daily task management, sprint tracking |
| **Table** | Table (spreadsheet-like) | List view, filtering, sorting |
| **Roadmap** | Timeline | Long-term planning, milestone tracking |

### 2.2 Board View

Manage items kanban-style with drag-and-drop.

```
┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
│  📋 Todo  │  │ 🔄 In    │  │ 👀 In    │  │  ✅ Done │
│           │  │ Progress │  │  Review  │  │          │
│  #5 Search│  │  #3 API  │  │  #4 UI   │  │  #1 Setup│
│  #6 Test  │  │          │  │          │  │  #2 DB   │
└──────────┘  └──────────┘  └──────────┘  └──────────┘
```

#### Customizing Columns

**Example 1: Development team**
```
Backlog → Todo → In Progress → In Review → Done
```

**Example 2: Customer support**
```
New → Triaged → In Progress → Waiting → Resolved
```

### 2.3 Table View

```
┌────────┬──────────┬──────────┬──────────┬──────────┐
│ Title  │ Status   │ Priority │ Assignee │ Sprint   │
├────────┼──────────┼──────────┼──────────┼──────────┤
│ #1 Setup│ Done    │ High     │ Alice    │ Sprint 1 │
│ #2 DB  │ Done     │ High     │ Bob      │ Sprint 1 │
│ #3 API │ Progress │ Medium   │ Alice    │ Sprint 2 │
│ #4 UI  │ Review   │ Medium   │ Charlie  │ Sprint 2 │
│ #5 Search│ Todo   │ Low      │ -        │ Sprint 3 │
└────────┴──────────┴──────────┴──────────┴──────────┘
```

### 2.4 Custom Fields

| Field Type | Description | Example |
|-----------|-------------|---------|
| **Text** | Text input | Notes, reference URLs |
| **Number** | Numeric value | Story points, estimated hours |
| **Date** | Date | Deadline, start date |
| **Single select** | Single selection | Priority, category |
| **Iteration** | Iteration period | Sprint |

#### Recommended Custom Fields

```
Priority:    🔴 High / 🟡 Medium / 🟢 Low
Size:        🐘 Large / 🐕 Medium / 🐁 Small
Sprint:      Sprint 1 / Sprint 2 / Sprint 3 ...
Category:    Frontend / Backend / Infrastructure / Documentation
```

### 2.5 Filters and Grouping

#### Filters

```
assignee:@me                              # My tasks
label:bug                                 # Bugs only
status:Todo,In Progress                   # Specific statuses
assignee:@me label:bug status:"In Progress"  # Combined
```

#### Grouping

- Group by **Status** → Kanban-like display
- Group by **Assignee** → Per-person view
- Group by **Priority** → Priority-based view

### 2.6 Using Multiple Views

| View Name | Type | Purpose |
|-----------|------|---------|
| Sprint Board | Board | Current sprint kanban |
| All Items | Table | Full item list |
| My Tasks | Table | Personal task list |
| Roadmap | Roadmap | Long-term plan |
| Bug Tracker | Table | Bugs only |

### ✅ Hands-on: Customize Views

1. **Add custom fields**
   - Priority (Single select): High / Medium / Low
   - Size (Single select): Large / Medium / Small
2. **Add a Table view** (name: `All Items`, sorted by Priority)
3. **Add a filtered view** (name: `My Tasks`, filter: `assignee:@me`)

---

## Part 3: Automation & Hands-on Practice (15 min)

### 3.1 Built-in Automation

Go to **Settings** → **Workflows** to configure:

| Workflow | Description |
|----------|-------------|
| **Item added to project** | Set Status to Todo when item is added |
| **Item reopened** | Set Status back to Todo when reopened |
| **Item closed** | Set Status to Done when closed |
| **Pull request merged** | Set Status to Done when PR is merged |
| **Auto-add to project** | Automatically add Issues matching criteria |

#### How to Configure

1. Click **⋯** (top right of Project) → **Workflows**
2. Select the workflow you want
3. Click **Edit**
4. Configure conditions and actions
5. Toggle ON to enable

### 3.2 Integration with GitHub Actions

```yaml
# .github/workflows/project-automation.yml
name: Project Automation

on:
  issues:
    types: [opened, labeled]

jobs:
  add-to-project:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/add-to-project@v1
        with:
          project-url: https://github.com/users/YOUR-USERNAME/projects/1
          github-token: ${{ secrets.PROJECT_TOKEN }}
```

### 3.3 Sprint Management

#### Setting Up Iterations (Sprints)

1. In Table view, click **+** → Add **Iteration** field
2. Set the iteration duration (e.g., 2 weeks)
3. Assign items to sprints

```
Sprint 1 (2/1 - 2/14)
├── Issue #1: Login feature    → Done ✅
├── Issue #2: DB design        → Done ✅
└── Issue #3: API development  → Done ✅

Sprint 2 (2/15 - 2/28)
├── Issue #4: UI development   → In Progress 🔄
├── Issue #5: Search feature   → Todo 📋
└── Issue #6: Testing          → Todo 📋
```

### 3.4 Hands-on: Run a Project

#### Scenario: "Team Website" Project

**Step 1: Create Issues**

| Issue | Title | Label | Priority |
|-------|-------|-------|----------|
| #1 | Create top page | enhancement | High |
| #2 | Add navigation menu | enhancement | High |
| #3 | Fix broken footer links | bug | Medium |
| #4 | Add team member profiles page | enhancement | Medium |
| #5 | Make responsive design | enhancement | Low |
| #6 | Run performance tests | task | Low |

**Step 2: Manage in Project**

1. Add all Issues to the Project
2. Set Priority and Size fields
3. Assign #1, #2, #3 to Sprint 1
4. Assign #4, #5, #6 to Sprint 2

**Step 3: Set Up Workflows**

1. Enable `Item closed → Done`
2. Enable `Item added → Todo`

**Step 4: Simulate Work**

1. Change #1's Status to `In Progress`
2. Add a work comment to Issue #1
3. Close #1 and verify it automatically moves to `Done`

---

## Wrap Up (10 min)

### What We Learned Today

- ✅ Creating and managing GitHub Projects
- ✅ Board view, Table view, and Roadmap view
- ✅ Custom fields
- ✅ Filters and grouping
- ✅ Built-in automation workflows
- ✅ Sprint management basics

### Next Workshop: "GitHub Actions DeepDive"

- Workflow syntax deep dive
- Building CI/CD pipelines
- Automated testing, building, and deployment
- Using custom Actions

---

## 📚 Reference Links

- [GitHub Projects Documentation](https://docs.github.com/en/issues/planning-and-tracking-with-projects)
- [Best Practices for Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/best-practices-for-projects)
- [Automating Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project)
- [Customizing Views](https://docs.github.com/en/issues/planning-and-tracking-with-projects/customizing-views-in-your-project)
