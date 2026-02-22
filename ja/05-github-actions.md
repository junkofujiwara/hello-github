# ワークショップ5：GitHub Actions DeepDive（60分）

> 📖 [English version](../en/05-github-actions.md)

## 🎯 このワークショップのゴール

- GitHub Actionsの基本概念を理解する
- ワークフローの構文を読み書きできるようになる
- CI/CDパイプラインを構築する
- テスト自動化・ビルド・デプロイを設定する

---

## 📋 アジェンダ

| 時間 | 内容 |
|------|------|
| 0:00 - 0:05 | 前回の振り返り |
| 0:05 - 0:25 | GitHub Actionsの基本 |
| 0:25 - 0:40 | ワークフローの実践 |
| 0:40 - 0:55 | 応用編：CI/CDとカスタムアクション |
| 0:55 - 1:00 | まとめ |

---

## 前回の振り返り（5分）

ワークショップ4では、GitHub Projectsを使ったプロジェクト管理を学びました。  
このワークショップでは、**GitHub Actions** を使って開発作業を自動化する方法を学びます。

---

## Part 1：GitHub Actionsの基本（20分）

### 1.1 GitHub Actionsとは

**GitHub Actions** は、リポジトリで何かが起きたとき（Push、PR作成など）をきっかけに、あらかじめ決めた処理を**自動で実行してくれる仕組み**です。  
たとえば「コードをPushしたらテストを自動実行」「PRをマージしたらサイトを自動デプロイ」といったことができます。

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│  Event   │────▶│ Workflow │────▶│  Result  │
│ (トリガー) │     │ (自動処理) │     │ (結果)   │
│          │     │          │     │          │
│ - Push   │     │ - テスト  │     │ - ✅ 成功 │
│ - PR     │     │ - ビルド  │     │ - ❌ 失敗 │
│ - Issue  │     │ - デプロイ │     │ - 📊 レポ │
│ - 定時   │     │ - 通知    │     │          │
└──────────┘     └──────────┘     └──────────┘
```

### 1.2 覚えておきたい用語

| 用語 | 説明 | 例 |
|------|------|-----|
| **Workflow** | 自動化プロセス全体 | `.github/workflows/ci.yml` |
| **Event** | ワークフローを起動するトリガー | `push`, `pull_request` |
| **Job** | ワークフロー内の実行単位 | `build`, `test`, `deploy` |
| **Step** | ジョブ内の各処理 | `actions/checkout@v4` |
| **Action** | 再利用可能なステップの単位 | `actions/setup-node@v4` |
| **Runner** | ジョブが実行される環境 | `ubuntu-latest`, `windows-latest` |

### 1.3 ワークフローファイルの配置

ワークフローは、リポジトリの `.github/workflows/` ディレクトリにYAMLファイルとして配置します。

```
hello-github/
├── .github/
│   └── workflows/
│       ├── ci.yml          # テスト & ビルド
│       ├── deploy.yml      # デプロイ
│       └── greeting.yml    # あいさつbot
├── src/
│   └── index.html
└── README.md
```

#### 🪟 Windows（PowerShell）でのディレクトリ作成

```powershell
# .github/workflows ディレクトリを作成
New-Item -ItemType Directory -Path ".github\workflows" -Force
```

#### 🍎 Mac（Terminal）でのディレクトリ作成

```bash
# .github/workflows ディレクトリを作成
mkdir -p .github/workflows
```

### 1.4 最初のワークフロー

以下は一番シンプルなワークフローの例です。コメント付きで読んでみましょう。

```yaml
# .github/workflows/hello.yml
name: Hello World                    # ワークフローの名前

on:                                  # トリガーの定義
  push:                              # Pushされたら実行
    branches: [ main ]               # mainブランチのみ

jobs:                                # ジョブの定義
  greeting:                          # ジョブ名
    runs-on: ubuntu-latest           # 実行環境
    steps:                           # ステップの定義
      - name: Say Hello              # ステップ名
        run: echo "Hello, GitHub Actions!"  # 実行コマンド
```

### 1.5 トリガー（on）の種類

| トリガー | 説明 | 例 |
|---------|------|-----|
| `push` | コードがPushされた時 | `on: push` |
| `pull_request` | PRが作成・更新された時 | `on: pull_request` |
| `issues` | Issueが作成・更新された時 | `on: issues` |
| `schedule` | 定期的に実行（cron） | `on: schedule` |
| `workflow_dispatch` | 手動で実行 | ボタンで起動 |
| `release` | リリースが作成された時 | `on: release` |

#### スケジュール実行の例

```yaml
on:
  schedule:
    - cron: '0 9 * * 1'   # 毎週月曜日の9:00 UTC
#          ┌─ 分 (0-59)
#          │ ┌─ 時 (0-23)
#          │ │ ┌─ 日 (1-31)
#          │ │ │ ┌─ 月 (1-12)
#          │ │ │ │ ┌─ 曜日 (0-6, 0=日曜)
#          * * * * *
```

#### 手動実行の例

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'デプロイ先の環境'
        required: true
        default: 'staging'
        type: choice
        options:
          - staging
          - production
```

### 1.6 ジョブの実行環境（Runner）

| Runner | OS | 用途 |
|--------|-----|------|
| `ubuntu-latest` | Ubuntu Linux | 一般的なCI/CD |
| `windows-latest` | Windows Server | Windows向けビルド |
| `macos-latest` | macOS | iOS/macOSアプリ |

> 💡 **パブリックリポジトリ**では無料で利用できます。プライベートリポジトリでは月間の無料枠があります。

### ✅ ハンズオン：最初のワークフローを作成

#### 🪟 Windows（PowerShell）

```powershell
# ディレクトリを作成
New-Item -ItemType Directory -Path ".github\workflows" -Force

# ワークフローファイルを作成
@"
name: Hello World

on:
  push:
    branches: [ main ]

jobs:
  greeting:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello, GitHub Actions!"
      - name: Show Date
        run: date
      - name: Show Runner Info
        run: |
          echo "Runner OS: `$RUNNER_OS"
          echo "Runner Arch: `$RUNNER_ARCH"
"@ | Out-File -FilePath ".github\workflows\hello.yml" -Encoding utf8
```

#### 🍎 Mac（Terminal）

```bash
# ディレクトリを作成
mkdir -p .github/workflows

# ワークフローファイルを作成
cat << 'EOF' > .github/workflows/hello.yml
name: Hello World

on:
  push:
    branches: [ main ]

jobs:
  greeting:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello, GitHub Actions!"
      - name: Show Date
        run: date
      - name: Show Runner Info
        run: |
          echo "Runner OS: $RUNNER_OS"
          echo "Runner Arch: $RUNNER_ARCH"
EOF
```

#### 共通手順

```bash
git add .github/workflows/hello.yml
git commit -m "Add hello world workflow"
git push origin main
```

Push後、GitHubリポジトリの **Actions** タブでワークフローが実行されていることを確認しましょう。

---

## Part 2：ワークフローの実践（15分）

### 2.1 実用的なCI ワークフロー

#### HTML/CSS の静的サイトチェック

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Check file existence
        run: |
          echo "📂 Checking project files..."
          ls -la
          if [ -f "index.html" ]; then
            echo "✅ index.html found"
          else
            echo "❌ index.html not found"
            exit 1
          fi

      - name: Validate HTML structure
        run: |
          echo "🔍 Checking HTML structure..."
          if grep -q "<html" index.html && grep -q "</html>" index.html; then
            echo "✅ HTML structure is valid"
          else
            echo "❌ HTML structure is invalid"
            exit 1
          fi

      - name: Check for broken links
        run: |
          echo "🔗 Checking for potential issues..."
          if grep -q "href=\"\"" index.html; then
            echo "⚠️ Warning: Empty href found"
          else
            echo "✅ No empty hrefs"
          fi
```

### 2.2 複数ジョブ

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Lint check
        run: echo "Running lint..."

  test:
    runs-on: ubuntu-latest
    needs: lint                    # lintが成功してから実行
    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        run: echo "Running tests..."

  deploy:
    runs-on: ubuntu-latest
    needs: [lint, test]            # 両方成功してから実行
    if: github.ref == 'refs/heads/main'  # mainブランチのみ
    steps:
      - uses: actions/checkout@v4
      - name: Deploy
        run: echo "Deploying..."
```

```
┌────────┐     ┌────────┐     ┌────────┐
│  Lint  │────▶│  Test  │────▶│ Deploy │
└────────┘     └────────┘     └────────┘
    ✅              ✅             ✅
```

### 2.3 環境変数とシークレット

#### 環境変数

ワークフロー内で使う値を環境変数として定義できます。ジョブ全体で使うものと、ステップごとに使うものがあります。

```yaml
jobs:
  example:
    runs-on: ubuntu-latest
    env:
      APP_NAME: hello-github       # ジョブレベルの環境変数
    steps:
      - name: Use environment variable
        env:
          GREETING: "Hello"        # ステップレベルの環境変数
        run: |
          echo "$GREETING from $APP_NAME"
          echo "Branch: ${{ github.ref_name }}"
          echo "Actor: ${{ github.actor }}"
```

#### シークレット（秘密情報）

APIキーやパスワードなど、コードに直接書きたくない情報は「シークレット」として登録します。

```yaml
steps:
  - name: Deploy with secret
    env:
      API_KEY: ${{ secrets.API_KEY }}
    run: |
      echo "Deploying with API key..."
      # シークレットはログにマスクされて表示されます
```

> ⚠️ シークレットは **Settings** → **Secrets and variables** → **Actions** で設定します。

### 2.4 アーティファクト（ビルド成果物の保存）

ワークフローで作成したファイル（テスト結果レポートなど）を保存して、あとからダウンロードできます。

```yaml
steps:
  - name: Create report
    run: |
      echo "Test Report" > report.txt
      echo "All tests passed!" >> report.txt

  - name: Upload artifact
    uses: actions/upload-artifact@v4
    with:
      name: test-report
      path: report.txt
      retention-days: 30
```

### ✅ ハンズオン：CIワークフローを作成

#### 🪟 Windows（PowerShell）

```powershell
@"
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Check project files
        run: |
          echo "Checking project structure..."
          ls -la
          echo "✅ Project structure check complete"

      - name: Validate HTML
        run: |
          if [ -f "index.html" ]; then
            echo "✅ index.html exists"
          else
            echo "ℹ️ No index.html found (not required)"
          fi
"@ | Out-File -FilePath ".github\workflows\ci.yml" -Encoding utf8
```

#### 🍎 Mac（Terminal）

```bash
cat << 'EOF' > .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Check project files
        run: |
          echo "Checking project structure..."
          ls -la
          echo "✅ Project structure check complete"

      - name: Validate HTML
        run: |
          if [ -f "index.html" ]; then
            echo "✅ index.html exists"
          else
            echo "ℹ️ No index.html found (not required)"
          fi
EOF
```

#### 共通手順

```bash
git add .github/workflows/ci.yml
git commit -m "Add CI workflow"
git push origin main
```

---

## Part 3：応用編（15分）

### 3.1 GitHub Pages へのデプロイ

**GitHub Pages** を使えば、リポジトリ内のHTMLファイルを無料でWebサイトとして公開できます。  
GitHub Actionsと組み合わせると、「pushするだけで自動的にサイトが更新される」仕組みが作れます。

```yaml
# .github/workflows/deploy-pages.yml
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
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 3.2 Issue & PR の自動化

#### Issue にラベルを自動追加

```yaml
# .github/workflows/auto-label.yml
name: Auto Label Issues

on:
  issues:
    types: [opened]

jobs:
  label:
    runs-on: ubuntu-latest
    permissions:
      issues: write
    steps:
      - name: Add triage label
        uses: actions/github-script@v7
        with:
          script: |
            github.rest.issues.addLabels({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              labels: ['triage']
            })
```

#### PR作成時にレビュアーを自動アサイン

```yaml
# .github/workflows/auto-assign.yml
name: Auto Assign Reviewers

on:
  pull_request:
    types: [opened]

jobs:
  assign:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    steps:
      - name: Auto assign
        uses: actions/github-script@v7
        with:
          script: |
            const pr = context.payload.pull_request;
            // PR作成者以外のメンバーをレビュアーに設定
            console.log(`PR #${pr.number} created by ${pr.user.login}`);
```

### 3.3 マトリクスビルド

「複数のOS × 複数のバージョン」の組み合わせで一気にテストを実行できます。  
たとえば以下の例では、3つのOS × 3つのNode.jsバージョン = 合計9パターンが同時にテストされます。

```yaml
name: Matrix Test

on: push

jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [18, 20, 22]
    steps:
      - uses: actions/checkout@v4
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: node --version
      - run: npm --version
```

```
┌─────────────────────────────────────┐
│           Matrix (9 jobs)           │
├────────────┬────────────┬───────────┤
│ Ubuntu/18  │ Ubuntu/20  │ Ubuntu/22 │
│ Windows/18 │ Windows/20 │ Windows/22│
│ macOS/18   │ macOS/20   │ macOS/22  │
└────────────┴────────────┴───────────┘
```

### 3.4 再利用可能なワークフロー

同じ処理を何度も書かなくてすむように、ワークフローを「部品」として再利用できます。

```yaml
# .github/workflows/reusable-deploy.yml（再利用可能側）
name: Reusable Deploy

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to ${{ inputs.environment }}
        run: echo "Deploying to ${{ inputs.environment }}..."
```

```yaml
# .github/workflows/production-deploy.yml（呼び出し側）
name: Production Deploy

on:
  push:
    branches: [ main ]

jobs:
  call-deploy:
    uses: ./.github/workflows/reusable-deploy.yml
    with:
      environment: production
```

### 3.5 よく使うActions

| Action | 用途 |
|--------|------|
| `actions/checkout@v4` | リポジトリのコードを取得 |
| `actions/setup-node@v4` | Node.js環境をセットアップ |
| `actions/setup-python@v5` | Python環境をセットアップ |
| `actions/upload-artifact@v4` | ビルド成果物をアップロード |
| `actions/download-artifact@v4` | ビルド成果物をダウンロード |
| `actions/cache@v4` | 依存関係のキャッシュ |
| `actions/github-script@v7` | JavaScriptでGitHub APIを操作 |
| `actions/configure-pages@v4` | GitHub Pagesの設定 |
| `actions/deploy-pages@v4` | GitHub Pagesへデプロイ |

### ✅ ハンズオン：GitHub Pages デプロイ

**Step 1：index.htmlを作成（未作成の場合）**

#### 🪟 Windows（PowerShell）

```powershell
@"
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitHubワークショップ</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 40px 20px;
            background-color: #0d1117;
            color: #c9d1d9;
        }
        h1 { color: #58a6ff; }
        .card {
            background-color: #161b22;
            border: 1px solid #30363d;
            border-radius: 6px;
            padding: 16px;
            margin: 16px 0;
        }
    </style>
</head>
<body>
    <h1>🎉 GitHubワークショップへようこそ！</h1>
    <div class="card">
        <h2>GitHub Actionsで自動デプロイされたサイトです</h2>
        <p>このページは、GitHubにPushするだけで自動的にデプロイされます。</p>
    </div>
</body>
</html>
"@ | Out-File -FilePath "index.html" -Encoding utf8
```

#### 🍎 Mac（Terminal）

```bash
cat << 'EOF' > index.html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitHubワークショップ</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 40px 20px;
            background-color: #0d1117;
            color: #c9d1d9;
        }
        h1 { color: #58a6ff; }
        .card {
            background-color: #161b22;
            border: 1px solid #30363d;
            border-radius: 6px;
            padding: 16px;
            margin: 16px 0;
        }
    </style>
</head>
<body>
    <h1>🎉 GitHubワークショップへようこそ！</h1>
    <div class="card">
        <h2>GitHub Actionsで自動デプロイされたサイトです</h2>
        <p>このページは、GitHubにPushするだけで自動的にデプロイされます。</p>
    </div>
</body>
</html>
EOF
```

**Step 2：デプロイワークフローを作成**

#### 🪟 Windows（PowerShell）

```powershell
@"
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
      url: `${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
"@ | Out-File -FilePath ".github\workflows\deploy-pages.yml" -Encoding utf8
```

#### 🍎 Mac（Terminal）

```bash
cat << 'EOF' > .github/workflows/deploy-pages.yml
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
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
EOF
```

**Step 3：Push & 確認**

```bash
git add .
git commit -m "Add GitHub Pages deploy workflow"
git push origin main
```

1. **Actions** タブでワークフローが成功するのを確認
2. **Settings** → **Pages** でURLを確認
3. サイトにアクセスして表示を確認

---

## まとめ（5分）

### 6回のワークショップで学んだこと

| ワークショップ | テーマ | 主な機能 |
|--------------|--------|---------|
| 準備編 | 事前準備 | アカウント作成、Git インストール |
| WS 1 | 開発サイクル全体像 | Repository, Issue, Branch, PR |
| WS 2 | Repository & Issue | リポジトリ設定, Issue管理 |
| WS 3 | Branch & Pull Request | ブランチ戦略, コードレビュー |
| WS 4 | Project管理 | ボード, 自動化, スプリント |
| WS 5 | GitHub Actions | CI/CD, デプロイ, 自動化 |
| WS 6 | GitHub Copilot | Chat, Agent, AI活用 |
| WS 7 | リリース＆デプロイ | タグ, Releases, Pages, Packages |
| WS 8 | セキュリティ | Dependabot, スキャン, プロテクション |
| WS 9 | 管理 | Repo, Org, Enterprise管理 |

### GitHub開発サイクル完全版

```
1. Issue作成         → やるべきことを明確にする
2. Project管理       → タスクを整理・優先順位付け
3. Branchを作成      → 安全に作業する
4. コード変更 & Push  → 成果を記録する
5. Pull Request      → レビューを受ける
6. GitHub Actions    → 自動テスト & チェック
7. マージ            → mainブランチに統合
8. デプロイ           → 自動で本番環境に反映
9. Issue Close       → タスク完了
```

### 次のステップ

- 🤖 **AI活用**: GitHub Copilot（次回ワークショップ6で学びます！）
- 🔐 **セキュリティ**: Dependabot, Code scanning
- 📦 **パッケージ**: GitHub Packages, Container Registry
- 📊 **分析**: Insights, Actions usage metrics
- 🏢 **組織管理**: Teams, Organization settings

---

## 📚 参考リンク

- [GitHub Actions ドキュメント](https://docs.github.com/ja/actions)
- [ワークフロー構文リファレンス](https://docs.github.com/ja/actions/using-workflows/workflow-syntax-for-github-actions)
- [GitHub Actions マーケットプレイス](https://github.com/marketplace?type=actions)
- [GitHub Pages ドキュメント](https://docs.github.com/ja/pages)
- [GitHub Skills](https://skills.github.com/) - インタラクティブな学習コース
