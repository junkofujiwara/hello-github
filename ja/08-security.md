# ワークショップ8：セキュリティ DeepDive（60分）

> 📖 [English version](../en/08-security.md)

## 🎯 このワークショップのゴール

- GitHubのセキュリティ機能と、無償/有償の違いを理解する
- Dependabotで依存関係の脆弱性を管理する方法を学ぶ
- シークレットスキャンとプッシュプロテクションを理解する
- コードスキャンとCodeQLの基本を知る
- GitHub Advanced Security（有償）が必要なケースを把握する

---

## 📋 アジェンダ

| 時間 | 内容 |
|------|------|
| 0:00 - 0:05 | ワークショップ7の振り返り |
| 0:05 - 0:20 | セキュリティ全体像と無償機能 |
| 0:20 - 0:35 | Dependabotとサプライチェーンセキュリティ |
| 0:35 - 0:50 | シークレットスキャン、コードスキャン＆ハンズオン |
| 0:50 - 1:00 | まとめ |

---

## ワークショップ7の振り返り（5分）

ワークショップ7では、タグ、Releases、GitHub Pages、GitHub Packagesを学びました。  
今回は **セキュリティ** に焦点を当てます。コード、シークレット、サプライチェーンをGitHubの組み込み機能でどう守るかを学びましょう。

---

## Part 1：セキュリティの全体像（15分）

### 1.1 なぜセキュリティが重要なのか

セキュリティは専門家だけの仕事ではありません。すべての開発者の責任です。GitHubはセキュリティを開発ワークフローに直接統合しているため、問題を **早期に** — 本番環境に届く前に — 発見できます。

```
┌──────────────────────────────────────────────────────────┐
│              GitHub セキュリティ                           │
│                                                           │
│  🔗 サプライチェーン  🔑 シークレット  🔍 コード          │
│  依存関係管理        漏洩防止          脆弱性スキャン      │
│                                                           │
│  🛡️ ブランチルール   📋 アドバイザリ  📊 セキュリティ概要 │
│  保護ルール          CVEデータベース   リスクダッシュボード │
└──────────────────────────────────────────────────────────┘
```

### 1.2 無償機能と有償機能

GitHubは多くのセキュリティ機能を **すべてのプランで無償** 提供しています。一部の高度な機能は **GitHub Advanced Security**（有償）の購入が必要です。

> 💰 最新の料金は [GitHub Advanced Security 料金ページ](https://github.com/security/plans) をご確認ください。

#### 無償機能（すべてのプラン）

| 機能 | 説明 |
|------|------|
| **セキュリティポリシー** | `SECURITY.md` — 脆弱性の報告方法をユーザーに伝える |
| **依存関係グラフ** | プロジェクトのすべての依存関係を可視化 |
| **Dependabotアラート** | 脆弱性のある依存関係の通知を受け取る |
| **Dependabotセキュリティアップデート** | 脆弱性を修正するPRを自動生成 |
| **Dependabotバージョンアップデート** | 依存関係を最新に保つPRを自動生成 |
| **セキュリティアドバイザリ** | 脆弱性を非公開で議論・修正 |
| **リポジトリルールセット** | コード標準とセキュリティルールの適用 |
| **シークレットスキャン（パートナーアラート）** | 漏洩したシークレットをサービス提供者に通知（パブリックリポジトリ） |
| **プッシュプロテクション（ユーザー向け）** | シークレットの誤コミットをブロック（パブリックリポジトリ） |
| **SBOM エクスポート** | ソフトウェア部品表をエクスポート |

#### パブリックリポジトリのみ無償

有償製品の一部機能は、**パブリックリポジトリでは無償** で利用できます：

| 機能 | パブリック | プライベート |
|------|:---------:|:----------:|
| コードスキャン（CodeQL） | ✅ 無償 | 🔒 有償 |
| シークレットスキャン（ユーザーアラート） | ✅ 無償 | 🔒 有償 |
| プッシュプロテクション | ✅ 無償 | 🔒 有償 |
| Copilot Autofix | ✅ 無償 | 🔒 有償 |
| Dependency Review | ✅ 無償 | 🔒 有償 |

#### 有償：GitHub Secret Protection 🔒

> ⚠️ **有償** — GitHub Secret Protectionの購入が必要です（GitHub TeamまたはEnterpriseプラン）。

| 機能 | 説明 |
|------|------|
| **シークレットスキャン（ユーザーアラート）** | プライベートリポジトリで漏洩したトークン/認証情報を検出 |
| **Copilotシークレットスキャン** | AI による非構造化シークレット（パスワード等）の検出 |
| **プッシュプロテクション（プライベートリポジトリ）** | プライベートリポジトリでシークレットのコミットをブロック |
| **デリゲートバイパス** | プッシュプロテクションのバイパス承認ワークフロー |
| **カスタムパターン** | 組織固有のシークレットパターンを定義 |
| **セキュリティ概要** | 組織全体のリスクダッシュボード |

#### 有償：GitHub Code Security 🔒

> ⚠️ **有償** — GitHub Code Securityの購入が必要です（GitHub TeamまたはEnterpriseプラン）。

| 機能 | 説明 |
|------|------|
| **コードスキャン（プライベートリポジトリ）** | CodeQLによるプライベートリポジトリの静的解析 |
| **Copilot Autofix** | コードスキャンアラートのAI生成修正 |
| **Dependabotカスタム自動トリアージ** | 大規模な自動却下、スヌーズ、修正トリガー |
| **Dependency Review（プライベートリポジトリ）** | PRでの依存関係変更レビュー |
| **セキュリティキャンペーン** | セキュリティアラートの大規模修正 |
| **セキュリティ概要** | 組織全体のリスクダッシュボード |

### 1.3 セキュリティポリシー

リポジトリに `SECURITY.md` を作成して、セキュリティ問題の報告方法をユーザーに伝えましょう。

```markdown
# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| 1.x     | ✅        |
| < 1.0   | ❌        |

## Reporting a Vulnerability

セキュリティの脆弱性を見つけた場合は、責任を持って報告してください：

1. 公開 Issue を **作成しないでください**
2. 連絡先: security@example.com
3. 48時間以内に対応します
4. 公開前に修正を行います
```

### ✅ ハンズオン：セキュリティポリシーの作成

1. リポジトリで **Security** タブをクリック
2. **Set up a security policy** をクリック（または `.github/SECURITY.md` を作成）
3. 上記のテンプレートを参考にポリシーを記述

---

## Part 2：Dependabotとサプライチェーンセキュリティ（15分）

### 2.1 Dependabotとは？

**Dependabot** は、プロジェクトの依存関係を監視し、脆弱性が発見された場合にアラートを発します。脆弱なパッケージを更新する **PRを自動生成** することもできます。

```
┌──────────┐     ┌──────────────┐     ┌──────────────┐
│  GitHub   │────▶│  Dependabot  │────▶│  結果        │
│ Advisory  │     │              │     │              │
│ Database  │     │ - 依存関係確認│     │ - ⚠️ アラート│
│           │     │ - 脆弱性検出 │     │ - 🔄 PR      │
│  (CVEs)   │     │ - 自動修正   │     │ - ✅ 更新完了 │
└──────────┘     └──────────────┘     └──────────────┘
```

### 2.2 依存関係グラフ

依存関係グラフは、プロジェクトの直接的な依存関係と推移的な依存関係をすべて分析・表示します。

**確認方法：**
1. リポジトリに移動
2. **Insights** タブ → **Dependency graph** をクリック

対応エコシステム：npm、pip、Maven、Gradle、Composer、Cargo、Go modules など

### 2.3 Dependabotアラート

依存関係に既知の脆弱性が見つかると、GitHubがDependabotアラートを作成します。

**有効化の方法：**
1. **Settings** → **Code security**（または **Code security and analysis**）
2. **Dependabot alerts** を有効化

各アラートには以下が含まれます：
- 重大度レベル（Critical、High、Medium、Low）
- 影響を受けるパッケージとバージョン範囲
- 推奨される修正バージョン
- CVE詳細とアドバイザリリンク

### 2.4 Dependabotセキュリティアップデート

Dependabotは脆弱な依存関係を修正するPRを自動作成できます。

**有効化の方法：**
1. **Settings** → **Code security**
2. **Dependabot security updates** を有効化

```
Dependabotアラート：axios 0.21.1 に既知の脆弱性あり（CVE-2023-XXXX）
    ↓
Dependabotが自動でPRを作成：
    "Bump axios from 0.21.1 to 0.21.4"
    ↓
レビューしてマージ ✅
```

### 2.5 Dependabotバージョンアップデート

セキュリティ修正だけでなく、Dependabotは定期的に新バージョンをチェックして **すべての依存関係を最新に保つ** ことができます。

`.github/dependabot.yml` を作成します：

```yaml
version: 2
updates:
  # npm の依存関係
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 10
    labels:
      - "dependencies"

  # GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
    labels:
      - "ci"
```

| 設定 | 説明 |
|------|------|
| `package-ecosystem` | 監視するエコシステム（npm、pip など） |
| `directory` | マニフェストファイルの場所 |
| `schedule.interval` | チェック頻度（`daily`、`weekly`、`monthly`） |
| `open-pull-requests-limit` | 同時に開くPRの最大数 |
| `labels` | 生成されたPRに付けるラベル |

### 2.6 ソフトウェア部品表（SBOM）

プロジェクトの依存関係の完全なリストをSBOMとしてエクスポートできます：

1. **Insights** → **Dependency graph** に移動
2. **Export SBOM** をクリック
3. SPDX形式でダウンロード

> 💡 SBOMは規制遵守やサプライチェーンの透明性のために、ますます求められるようになっています。

### ✅ ハンズオン：Dependabotのセットアップ

1. **Settings → Code security で Dependabotアラートを有効化**
2. **Dependabotセキュリティアップデートを有効化**
3. **`.github/dependabot.yml` を作成** してバージョンアップデートを設定

#### 🪟 Windows（PowerShell）

```powershell
# ディレクトリを作成
New-Item -ItemType Directory -Path ".github" -Force

# dependabot.yml を作成
@"
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
    labels:
      - "dependencies"
      - "ci"
"@ | Out-File -FilePath ".github\dependabot.yml" -Encoding utf8
```

#### 🍎 Mac（ターミナル）

```bash
# ディレクトリを作成
mkdir -p .github

# dependabot.yml を作成
cat << 'EOF' > .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
    labels:
      - "dependencies"
      - "ci"
EOF
```

#### 共通手順

```bash
git add .github/dependabot.yml
git commit -m "Add Dependabot configuration"
git push origin main
```

---

## Part 3：シークレットスキャンとコードスキャン（15分）

### 3.1 シークレットスキャン

**シークレットスキャン** は、リポジトリに誤ってコミットされたシークレット（APIキー、トークン、パスワード）を検出します。

```
開発者が誤ってコミット：
    API_KEY=sk-1234567890abcdef
        ↓
シークレットスキャンが検出：
    ⚠️ "GitHub Personal Access Token found in config.js"
        ↓
アラート作成 → 開発者がキーをローテーション 🔄
```

#### 利用可能範囲

| 範囲 | 無償 | 有償（GitHub Secret Protection） |
|------|:----:|:------:|
| パートナーアラート（パブリックリポジトリ） | ✅ | ✅ |
| ユーザーアラート（パブリックリポジトリ） | ✅ | ✅ |
| ユーザーアラート（プライベートリポジトリ） | ❌ | 🔒 有償 |
| Copilotシークレットスキャン（AI） | ❌ | 🔒 有償 |
| カスタムパターン | ❌ | 🔒 有償 |

#### 有効化の方法（パブリックリポジトリ）

1. **Settings** → **Code security**
2. **Secret scanning** を有効化

### 3.2 プッシュプロテクション

**プッシュプロテクション** は、シークレットがリポジトリにプッシュされる前にブロックします。シークレットがGitHubに到達する **前に** プッシュを阻止します。

```
$ git push origin main
remote: error: GH009: Secrets detected!
remote:
remote: — GitHub Personal Access Token found in config.js:3
remote:
remote: This push was blocked.
```

#### 利用可能範囲

| 範囲 | 無償 | 有償 |
|------|:----:|:----:|
| ユーザー向けプッシュプロテクション（パブリックリポジトリ） | ✅ | ✅ |
| プッシュプロテクション（プライベートリポジトリ） | ❌ | 🔒 有償（Secret Protection） |
| デリゲートバイパス | ❌ | 🔒 有償（Secret Protection） |

> 💡 ユーザー向けプッシュプロテクションは、個人アカウントでは **デフォルトで有効** です。**Settings** → **Code security and analysis** で管理できます。

### 3.3 コードスキャン

**コードスキャン** は、静的解析を使ってコードのセキュリティ脆弱性やコーディングエラーを検出します。GitHubの組み込みツールは **CodeQL** です。

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│  コード   │────▶│  CodeQL  │────▶│  アラート │
│  Push/PR │     │  解析    │     │          │
│          │     │          │     │ - XSS    │
│          │     │ - ビルド  │     │ - SQLi   │
│          │     │ - クエリ  │     │ - CSRF   │
│          │     │ - 分析    │     │ - etc.   │
└──────────┘     └──────────┘     └──────────┘
```

#### 利用可能範囲

| 範囲 | 無償 | 有償（GitHub Code Security） |
|------|:----:|:------:|
| CodeQL（パブリックリポジトリ） | ✅ | ✅ |
| CodeQL（プライベートリポジトリ） | ❌ | 🔒 有償 |
| Copilot Autofix（パブリックリポジトリ） | ✅ | ✅ |
| Copilot Autofix（プライベートリポジトリ） | ❌ | 🔒 有償 |
| セキュリティキャンペーン | ❌ | 🔒 有償 |

#### コードスキャンの設定（パブリックリポジトリ）

1. **Settings** → **Code security**
2. **Code scanning** を有効化 → **Set up** → **Default**
3. またはワークフローを手動作成：

```yaml
# .github/workflows/codeql.yml
name: "CodeQL"

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 6 * * 1'  # 毎週月曜 6:00 UTC

jobs:
  analyze:
    name: Analyze
    runs-on: ubuntu-latest
    permissions:
      security-events: write
    strategy:
      matrix:
        language: [ 'javascript' ]
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: ${{ matrix.language }}

      - name: Autobuild
        uses: github/codeql-action/autobuild@v3

      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v3
```

### 3.4 Dependency Review

**Dependency Review** は、PRにおける依存関係変更のセキュリティ影響を、マージ前に確認できます。

#### 利用可能範囲

| 範囲 | 無償 | 有償（GitHub Code Security） |
|------|:----:|:------:|
| パブリックリポジトリ | ✅ | ✅ |
| プライベートリポジトリ | ❌ | 🔒 有償 |

#### Dependency Review Action

```yaml
# .github/workflows/dependency-review.yml
name: Dependency Review

on: pull_request

permissions:
  contents: read

jobs:
  dependency-review:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Dependency Review
        uses: actions/dependency-review-action@v4
        with:
          fail-on-severity: moderate
```

### 3.5 GitHub Advanced Security（有償）まとめ

> ⚠️ 以下の製品は **有償** であり、GitHub TeamまたはEnterpriseプランが必要です。最新の料金は [GitHubセキュリティプランページ](https://github.com/security/plans) をご確認ください。

| 製品 | 概要 | 主な機能 |
|------|------|---------|
| **GitHub Secret Protection** 🔒 | シークレット漏洩防止 | シークレットスキャン（プライベート）、プッシュプロテクション（プライベート）、カスタムパターン、AI検出 |
| **GitHub Code Security** 🔒 | 脆弱性の発見と修正 | コードスキャン（プライベート）、Copilot Autofix（プライベート）、Dependency Review（プライベート）、セキュリティキャンペーン |

### ✅ ハンズオン：セキュリティ機能を探索

**演習1：リポジトリのSecurityタブを確認**

1. リポジトリに移動
2. **Security** タブをクリック
3. 確認：セキュリティ概要、Dependabotアラート、コードスキャンアラート、シークレットスキャンアラート

**演習2：デフォルトのセキュリティ設定を有効化**

1. **Settings** → **Code security**
2. リポジトリで利用可能なすべての無償機能を有効化：
   - Dependabotアラート ✅
   - Dependabotセキュリティアップデート ✅
   - シークレットスキャン（パブリックリポジトリの場合） ✅

**演習3：Dependabotアラートの確認（ある場合）**

1. **Security** タブ → **Dependabot alerts**
2. 未対応のアラートを確認
3. 推奨修正バージョンをチェック
4. DependabotのPRがあればレビューしてマージ

---

## まとめ（10分）

### 今日学んだこと

- ✅ GitHubの無償セキュリティ機能と有償機能（Advanced Security）の違い
- ✅ セキュリティポリシー（`SECURITY.md`）の作成方法
- ✅ Dependabotアラート、セキュリティアップデート、バージョンアップデート
- ✅ シークレットスキャンとプッシュプロテクション
- ✅ CodeQLによるコードスキャン
- ✅ PRのDependency Review

### セキュリティベストプラクティス チェックリスト

| 項目 | 状態 |
|------|------|
| Dependabotアラートを有効化 | ☐ |
| Dependabotセキュリティアップデートを有効化 | ☐ |
| `.github/dependabot.yml` でバージョンアップデートを設定 | ☐ |
| `SECURITY.md` を作成 | ☐ |
| シークレットスキャンを有効化（パブリックリポジトリの場合） | ☐ |
| プッシュプロテクションを有効化 | ☐ |
| コードスキャンを設定（パブリックリポジトリの場合） | ☐ |
| Dependency Review Actionを追加 | ☐ |
| シークレットをコードにコミットしない | ☐ |
| 機密データには環境変数 / Secretsを使用 | ☐ |

### ワークショップシリーズ全体の振り返り

| WS | テーマ | 主な内容 |
|----|--------|---------|
| 準備 | 事前準備 | アカウント、Git設定 |
| WS 1 | 開発サイクル概要 | Repository、Issue、Branch、PR |
| WS 2 | リポジトリとIssue | リポジトリ設定、Issue管理 |
| WS 3 | ブランチとPR | ブランチ戦略、コードレビュー |
| WS 4 | プロジェクト管理 | ボード、自動化、スプリント |
| WS 5 | GitHub Actions | CI/CD、デプロイ、自動化 |
| WS 6 | GitHub Copilot | Chat、Agent、Skills |
| WS 7 | リリース＆デプロイ | タグ、Releases、Pages、Packages |
| WS 8 | セキュリティ | Dependabot、スキャン、プロテクション |
| WS 9 | 管理 | Repo、Org、Enterprise管理 |

### 次のステップ

- 🔒 **すべての無償セキュリティ機能を有効化** しましょう
- 📋 **Dependabot設定ファイルを作成** しましょう
- 🔑 **スキャンで見つかったシークレットをローテーション** しましょう
- 🏢 **GitHub Advanced Security（有償）を評価** — プライベートリポジトリや組織向け
- 📖 **GitHub Security Lab** — [securitylab.github.com](https://securitylab.github.com/)
- 🎓 **GitHub認定資格** — [GitHub Advanced Security認定](https://resources.github.com/learn/certifications/) を検討

---

## 📚 参考リンク

- [GitHubセキュリティ機能](https://docs.github.com/ja/code-security/getting-started/github-security-features)
- [GitHub Advanced Securityについて](https://docs.github.com/ja/get-started/learning-about-github/about-github-advanced-security)
- [Dependabotクイックスタート](https://docs.github.com/ja/code-security/getting-started/dependabot-quickstart-guide)
- [シークレットスキャンについて](https://docs.github.com/ja/code-security/secret-scanning/introduction/about-secret-scanning)
- [コードスキャンについて](https://docs.github.com/ja/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning)
- [GitHub Advisory Database](https://github.com/advisories)
- [GitHubセキュリティプラン](https://github.com/security/plans)
