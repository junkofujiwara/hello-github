# 📖 プラットフォーム用語集：GitHub vs GitLab vs Bitbucket vs Azure DevOps

> 📖 [English version](../en/platform-glossary.md)

他のプラットフォームから GitHub に来ると、「あの機能は GitHub では何と呼ぶの？」と迷うことがあります。この用語集では **GitHub**、**GitLab**、**Bitbucket**、**Azure DevOps** の**同等の概念**を対応表で整理しています。

---

## 📋 目次

1. [クイックリファレンス表](#1-クイックリファレンス表)
2. [リポジトリとプロジェクト構造](#2-リポジトリとプロジェクト構造)
3. [ブランチとコードレビュー](#3-ブランチとコードレビュー)
4. [CI/CD と自動化](#4-cicd-と自動化)
5. [プロジェクト管理と Issue](#5-プロジェクト管理と-issue)
6. [セキュリティとアクセス制御](#6-セキュリティとアクセス制御)
7. [パッケージとリリース](#7-パッケージとリリース)
8. [組織と管理](#8-組織と管理)
9. [乗り換え時に注意すべきポイント](#9-乗り換え時に注意すべきポイント)

---

## 1. クイックリファレンス表

| 概念 | GitHub | GitLab | Bitbucket | Azure DevOps |
|------|--------|--------|-----------|--------------|
| **コードホスティング単位** | Repository | Project | Repository | Repository（Project 内） |
| **リポジトリのグループ化** | Organization | Group | Workspace | Project |
| **コードレビュー依頼** | Pull Request (PR) | Merge Request (MR) | Pull Request (PR) | Pull Request (PR) |
| **CI/CD 設定ファイル** | `.github/workflows/*.yml` | `.gitlab-ci.yml` | `bitbucket-pipelines.yml` | `azure-pipelines.yml` |
| **CI/CD システム** | GitHub Actions | GitLab CI/CD | Bitbucket Pipelines | Azure Pipelines |
| **CI/CD 実行環境** | Runner（GitHub ホスト / セルフホスト） | Runner（共有 / 専用） | Runner（クラウド / セルフホスト） | Agent（Microsoft ホスト / セルフホスト） |
| **CI/CD の作業単位** | Job（Workflow 内） | Job（Pipeline 内） | Step（Pipeline 内） | Task（Pipeline 内） |
| **課題管理** | Issues | Issues | Jira（連携）/ Issues（限定的） | Work Items |
| **プロジェクトボード** | Projects（Project Board） | Boards | Jira Board / Trello | Boards |
| **Wiki / ドキュメント** | Wiki / GitHub Pages | Wiki / GitLab Pages | Wiki / Confluence（連携） | Wiki |
| **パッケージレジストリ** | GitHub Packages | GitLab Package Registry | —（Jira/Artifactory 経由） | Azure Artifacts |
| **静的サイトホスティング** | GitHub Pages | GitLab Pages | — | Azure Static Web Apps（別サービス） |
| **シークレット管理** | Actions Secrets | CI/CD Variables（マスク） | Repository Variables / Secured | Pipeline Variables（secret） |
| **コードスキャン** | Code Scanning (CodeQL) | SAST / DAST | —（サードパーティ） | Azure DevOps Extensions |
| **依存関係アラート** | Dependabot | Dependency Scanning | —（サードパーティ） | —（サードパーティ） |
| **コンテナレジストリ** | GitHub Container Registry (ghcr.io) | GitLab Container Registry | —（サードパーティ） | Azure Container Registry（別サービス） |
| **コードスニペット共有** | Gist | Snippets | Snippets | — |
| **インラインコード提案** | Suggested Changes（PR 内） | Suggestions（MR 内） | — | — |

---

## 2. リポジトリとプロジェクト構造

### リポジトリ

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Repository** | 基本単位。ユーザーまたは Organization に属する。 |
| **GitLab** | **Project** | 1つの Git リポジトリ + CI/CD + Issue などを含む単位。 |
| **Bitbucket** | **Repository** | Workspace（Cloud）または Project（Server/Data Center）内に存在。 |
| **Azure DevOps** | **Repository** | Project 内に存在。1つの Project に複数のリポジトリを持てる。 |

### グループ化 / 組織

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Organization** | リポジトリを所有する共有アカウント。Team でアクセス制御。 |
| **GitLab** | **Group / Subgroup** | 階層構造。Group が Project を含み、Subgroup をネストできる。 |
| **Bitbucket** | **Workspace** | 最上位コンテナ（Cloud）。Workspace 内の Project でリポジトリをグループ化。 |
| **Azure DevOps** | **Organization → Project** | Organization が最上位。Project がリポジトリ、パイプライン、ボードなどをまとめる。 |

### フォーク

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Fork** | リポジトリのサーバー側コピーを自分のアカウントに作成。OSS 貢献で一般的。 |
| **GitLab** | **Fork** | 同じ概念。プロジェクトを自分のネームスペースにフォーク。 |
| **Bitbucket** | **Fork** | 同じ概念。同一 Workspace 内または別の Workspace にフォーク。 |
| **Azure DevOps** | **Fork** | サポートされているが、あまり使われない。 |

---

## 3. ブランチとコードレビュー

### Pull Request / Merge Request

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Pull Request (PR)** | ブランチのマージを依頼。レビュー、チェック、ディスカッションが可能。 |
| **GitLab** | **Merge Request (MR)** | PR と機能的に同一。「マージ」を強調した名前。 |
| **Bitbucket** | **Pull Request (PR)** | GitHub の PR と同じ。 |
| **Azure DevOps** | **Pull Request (PR)** | 同じ概念。必須レビュアーやビルド検証などのポリシーを設定可能。 |

> 💡 「Pull Request」と「Merge Request」は同じ意味です ── コードのレビューと統合を依頼するものです。「MR」と言う人は、GitLab 出身の可能性が高いです。

### ブランチ保護

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Branch Protection Rules / Rulesets** | レビュー必須、ステータスチェック必須、プッシュ制限など。 |
| **GitLab** | **Protected Branches** | ロールによるプッシュ/マージの制限。承認ルールの設定も可能。 |
| **Bitbucket** | **Branch Permissions** | ブランチパターンごとに書き込みアクセスを制限。 |
| **Azure DevOps** | **Branch Policies** | レビュアー必須、関連 Work Item 必須、ビルド成功必須など。 |

### コードレビュー

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Review（Approve / Request Changes / Comment）** | レビュアーが正式なレビューステータスを残す。 |
| **GitLab** | **Approval Rules** | 必要な承認数を設定可能。レビュアーと承認者は区別される。 |
| **Bitbucket** | **Approve / Needs Work** | シンプルなレビュー状態。 |
| **Azure DevOps** | **Approve / Reject / Wait for Author** | PR に対する複数の投票オプション。 |

### ドラフト

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Draft Pull Request** | 「Ready for review」にするまでマージ不可。 |
| **GitLab** | **Draft Merge Request** | タイトルに `Draft:` プレフィックスが付く。同じ動作。 |
| **Bitbucket** | — | ネイティブのドラフト PR はない。命名規則（例：`[WIP]`）で代用。 |
| **Azure DevOps** | **Draft Pull Request** | サポートあり。公開するまで完了不可。 |

---

## 4. CI/CD と自動化

### パイプライン / ワークフロー

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Workflow**（**Jobs** → **Steps** を含む） | `.github/workflows/*.yml` で定義。イベントでトリガー。 |
| **GitLab** | **Pipeline**（**Stages** → **Jobs** を含む） | `.gitlab-ci.yml` で定義。Stage は順次実行、同一 Stage 内の Job は並列実行。 |
| **Bitbucket** | **Pipeline**（**Steps** を含む） | `bitbucket-pipelines.yml` で定義。 |
| **Azure DevOps** | **Pipeline**（**Stages** → **Jobs** → **Steps** を含む） | YAML（`azure-pipelines.yml`）またはクラシック（GUI）エディタ。 |

### Runner / Agent

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Runner** | GitHub ホスト（Ubuntu, Windows, macOS）またはセルフホスト。 |
| **GitLab** | **Runner** | 共有、グループ、またはプロジェクト Runner。タグで設定。 |
| **Bitbucket** | **Runner** | Atlassian ホストまたはセルフホスト（Bitbucket Runner）。 |
| **Azure DevOps** | **Agent** | Microsoft ホストまたはセルフホスト Agent。Agent Pool で管理。 |

### 再利用可能な CI コンポーネント

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Action**（Marketplace から）/ **Reusable Workflow** | Action はプラグインエコシステム。Reusable Workflow は他のワークフローを呼び出す。 |
| **GitLab** | **CI/CD Component** / **include** | `include:` でパイプラインテンプレートを共有。Components Catalog（新機能）。 |
| **Bitbucket** | **Pipe** | Marketplace からのビルド済み統合ステップ。 |
| **Azure DevOps** | **Task** / **Template** | Marketplace からの Task。再利用のための Template。 |

### 環境とデプロイ

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Environment** | 名前付きターゲット（例：`production`）。保護ルールとシークレットを設定可能。 |
| **GitLab** | **Environment** | 環境ごとのデプロイを追跡。レビューアプリもサポート。 |
| **Bitbucket** | **Deployment Environment** | デプロイ追跡用の環境を定義。 |
| **Azure DevOps** | **Environment** | デプロイジョブのターゲット。承認とチェックが可能。 |

---

## 5. プロジェクト管理と Issue

### Issue / Work Item

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Issue** | 軽量。ラベル、マイルストーン、担当者をサポート。Markdown ベース。 |
| **GitLab** | **Issue** | より豊富な機能：ウェイト、期限、ヘルスステータス、Epic。 |
| **Bitbucket** | **Jira Issue**（通常） | ネイティブの Issue は最小限。多くのチームは Jira 連携を使用。 |
| **Azure DevOps** | **Work Item**（Bug, Task, User Story, Epic など） | カスタマイズ可能なワークアイテムタイプとプロセスを持つフル機能。 |

### ラベル / タグ

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Labels** | Issue と PR に付ける色分けタグ。 |
| **GitLab** | **Labels** | スコープラベル（例：`priority::high`）で排他的カテゴリを実現。 |
| **Bitbucket** | —（Jira では **Labels**） | ネイティブのラベル機能は限定的。 |
| **Azure DevOps** | **Tags** | Work Item に付ける自由形式のタグ。 |

### マイルストーン / イテレーション

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Milestone** | Issue/PR をグループ化。期限と進捗バー付き。 |
| **GitLab** | **Milestone / Iteration** | Milestone はリリース用、Iteration はスプリント用。 |
| **Bitbucket** | —（Jira では **Fix Version / Sprint**） | スプリント計画は Jira を使用。 |
| **Azure DevOps** | **Iteration（Sprint）** | 階層的なイテレーションパス。フルスプリント計画をサポート。 |

### プロジェクトボード

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Projects**（新）/ Project Board（クラシック） | テーブル、ボード、ロードマップビュー。カスタムフィールド対応。 |
| **GitLab** | **Boards** | ラベルまたは担当者ベースのカンバンボード。 |
| **Bitbucket** | **Jira Board / Trello** | Jira でのスクラムまたはカンバンボード。 |
| **Azure DevOps** | **Boards** | スイムレーン、WIP 制限、累積フロー付きカンバンボード。 |

---

## 6. セキュリティとアクセス制御

### アクセストークン

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Personal Access Token (PAT)** / **Fine-grained PAT** | API と Git 操作に使用。Fine-grained PAT は特定リポジトリにスコープを限定可能。 |
| **GitLab** | **Personal Access Token / Project Access Token / Group Access Token** | 異なるレベルで複数のトークンスコープ。 |
| **Bitbucket** | **App Password** / **Repository Access Token** | App Password はユーザーレベル。 |
| **Azure DevOps** | **Personal Access Token (PAT)** | Organization にスコープ。きめ細かい権限設定。 |

### ロールと権限

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Read / Triage / Write / Maintain / Admin** | リポジトリレベルのロール。Organization ロールでより広いアクセス。 |
| **GitLab** | **Guest / Reporter / Developer / Maintainer / Owner** | プロジェクトとグループレベルの階層的ロール。 |
| **Bitbucket** | **Read / Write / Admin** | シンプルなロールモデル。 |
| **Azure DevOps** | **Reader / Contributor / Project Admin / など** | きめ細かいセキュリティグループベースの権限。 |

### コードスキャン / セキュリティ

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Code Scanning** / **Secret Scanning** / **Dependabot** | GitHub Advanced Security (GHAS) の一部。 |
| **GitLab** | **SAST / DAST / Dependency Scanning / Secret Detection** | GitLab Ultimate に組み込み。 |
| **Bitbucket** | — | サードパーティ連携に依存（例：Snyk, SonarCloud）。 |
| **Azure DevOps** | **Microsoft Defender for DevOps** / Extensions | Marketplace または Defender 経由のセキュリティ統合。 |

---

## 7. パッケージとリリース

### パッケージレジストリ

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **GitHub Packages** | npm, Maven, NuGet, Docker (ghcr.io), RubyGems。 |
| **GitLab** | **Package Registry** | npm, Maven, NuGet, PyPI, Conan, Go など。 |
| **Bitbucket** | — | ビルトインレジストリなし。Artifactory などの外部ツールを使用。 |
| **Azure DevOps** | **Azure Artifacts** | npm, Maven, NuGet, Python, Universal Packages。 |

### リリース

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Release** | Git タグベース。バイナリを添付可能。リリースノートの自動生成。 |
| **GitLab** | **Release** | GitHub と同様。マイルストーンとタグに紐付け。 |
| **Bitbucket** | **Downloads** | ファイルダウンロードセクション。正式なリリース機能はなし。Jira Releases を使用。 |
| **Azure DevOps** | **Release**（クラシック）/ Pipeline artifacts | クラシック Release（GUI）または YAML パイプラインデプロイ。 |

---

## 8. 組織と管理

### チーム管理

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Teams**（Organization 内） | ネストされたチーム。チームごとにリポジトリアクセスを割り当て。`@org/team` でメンション。 |
| **GitLab** | **Groups / Subgroups** | メンバーは親グループからアクセス権を継承。 |
| **Bitbucket** | **User Groups**（Workspace 内） | 権限管理のためのグループ。 |
| **Azure DevOps** | **Teams**（Project 内） | 各チームが独自のボード、バックログ、イテレーションを持つ。 |

### シングルサインオン（SSO）

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **SAML SSO** / **SCIM**（Enterprise） | Organization レベルまたは Enterprise レベルの SSO。 |
| **GitLab** | **SAML SSO / SCIM / LDAP** | グループレベルの SAML。インスタンスレベルの LDAP。 |
| **Bitbucket** | **Atlassian Access**（SAML / SCIM） | Atlassian 組織で管理。 |
| **Azure DevOps** | **Azure AD (Entra ID)** | Microsoft ID プラットフォームとネイティブ統合。 |

### 監査とコンプライアンス

| プラットフォーム | 用語 | 補足 |
|-----------------|------|------|
| **GitHub** | **Audit Log** | Organization および Enterprise の監査ログ。API 利用可能。 |
| **GitLab** | **Audit Events** | インスタンス、グループ、プロジェクトレベルのイベント。 |
| **Bitbucket** | **Audit Log**（Atlassian Admin 経由） | Workspace レベルの監査ログ。 |
| **Azure DevOps** | **Auditing** | Organization レベルの監査ストリーム。 |

---

## 9. 乗り換え時に注意すべきポイント

### GitLab ユーザーが GitHub に来たとき

| GitLab | GitHub | ヒント |
|--------|--------|--------|
| Merge Request (MR) | **Pull Request (PR)** | 同じ概念、名前が違うだけ。 |
| `.gitlab-ci.yml` | **`.github/workflows/*.yml`** | GitHub は1つではなく複数のワークフローファイルを使用。 |
| Stages → Jobs | **Jobs → Steps** | 用語の階層構造が異なる。 |
| `include:` テンプレート | **Reusable Workflows / Composite Actions** | CI ロジック共有の仕組みが異なる。 |
| スコープラベル（`priority::high`） | **Labels**（手動の命名規則） | GitHub のラベルはフラット。`priority: high` のような命名規則で代用。 |
| Epics | **Projects**（ロードマップビュー） | GitHub には「Epic」がない。Projects で上位レベルの追跡を行う。 |

### Bitbucket ユーザーが GitHub に来たとき

| Bitbucket | GitHub | ヒント |
|-----------|--------|--------|
| Workspace | **Organization** | リポジトリとチームの最上位コンテナ。 |
| Pipelines / Pipes | **Actions / Workflows** | 「Pipes」≈ Marketplace の「Actions」。 |
| App Password | **Personal Access Token (PAT)** | GitHub の PAT はきめ細かいスコープオプションあり。 |
| Jira 連携 | **GitHub Issues + Projects** | または GitHub for Jira アプリで Jira を引き続き使用可能。 |

### Azure DevOps ユーザーが GitHub に来たとき

| Azure DevOps | GitHub | ヒント |
|--------------|--------|--------|
| Work Items | **Issues** | シンプルだが Projects のカスタムフィールドで拡張可能。 |
| Boards | **Projects**（ボードビュー） | GitHub Projects はカンバンスタイルのボードをサポート。 |
| Azure Pipelines | **GitHub Actions** | YAML ベースだが構文とエコシステムが異なる。 |
| Agent | **Runner** | 同じ概念 — CI/CD ジョブを実行するマシン。 |
| Artifacts | **GitHub Packages** | サポートされるフォーマットが異なる。 |
| Azure Repos | **GitHub Repositories** | どちらも Git ベース。移行は簡単。 |

---

## 💡 スムーズな移行のためのヒント

1. **1対1で翻訳しない** — 各プラットフォームには独自の思想があります。古いワークフローを正確に再現するのではなく、プラットフォームの強みに適応しましょう。
2. **Marketplace を探索する** — GitHub の [Actions Marketplace](https://github.com/marketplace?type=actions) は、GitLab の CI/CD Components、Bitbucket の Pipes、Azure DevOps の Task Extensions に相当します。
3. **GitHub CLI（`gh`）を使う** — PR の作成、ワークフロー実行の確認、Issue の管理など、一般的なタスクをターミナルから高速化できます。
4. **移行ガイドを読む**：
   - [GitLab から GitHub への移行](https://docs.github.com/ja/migrations/importing-source-code/using-github-importer/importing-a-repository-with-github-importer)
   - [Bitbucket から GitHub への移行](https://docs.github.com/ja/migrations/importing-source-code/using-github-importer/importing-a-repository-with-github-importer)
   - [Azure DevOps から GitHub への移行](https://docs.github.com/ja/migrations/importing-source-code/using-github-importer/importing-a-repository-with-github-importer)

---

[README に戻る](../README.ja.md)
