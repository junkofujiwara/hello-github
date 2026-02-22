# ワークショップ7：リリース＆デプロイ DeepDive（60分）

> 📖 [English version](../en/07-release-and-deployment.md)

## 🎯 このワークショップのゴール

- Gitタグと GitHub Releases でバージョン管理を理解する
- リリースノートの自動生成を学ぶ
- GitHub Pages で静的サイトをホスティングする
- GitHub Packages でパッケージを公開・管理する
- 無償機能と有償機能の違いを把握する

---

## 📋 アジェンダ

| 時間 | 内容 |
|------|------|
| 0:00 - 0:05 | ワークショップ6の振り返り |
| 0:05 - 0:20 | タグとリリース |
| 0:20 - 0:35 | GitHub Pages |
| 0:35 - 0:50 | GitHub Packages |
| 0:50 - 1:00 | まとめ |

---

## ワークショップ6の振り返り（5分）

ワークショップ6では、GitHub CopilotによるAI支援開発を学びました。  
今回は **リリース＆デプロイ** に焦点を当てます。GitHubの組み込み機能を使って、プロジェクトのバージョン管理、公開、配布を行う方法を学びましょう。

---

## Part 1：タグとリリース（15分）

### 1.1 Gitタグとは？

**タグ** は特定のコミットへのポインタです。プロジェクト履歴の重要なポイント — 通常はリリースバージョン — を示すために使います。

```
main: A ── B ── C ── D ── E ── F
                ↑              ↑
             v1.0.0         v2.0.0
             (タグ)          (タグ)
```

#### タグの2種類

| 種類 | 説明 | 用途 |
|------|------|------|
| **軽量タグ** | コミットへのポインタのみ | 一時的・プライベートなマーカー |
| **注釈付きタグ** | 作成者、日付、メッセージを含む完全なオブジェクト | リリースバージョン（推奨） |

#### タグの作成

##### 🪟 Windows（PowerShell）/ 🍎 Mac（ターミナル）

```bash
# 注釈付きタグ（リリースに推奨）
git tag -a v1.0.0 -m "Release version 1.0.0"

# 軽量タグ
git tag v1.0.0-beta

# 特定のコミットにタグを付ける
git tag -a v1.0.0 -m "Release version 1.0.0" abc1234

# タグをリモートにプッシュ
git push origin v1.0.0        # 1つのタグをプッシュ
git push origin --tags         # すべてのタグをプッシュ

# タグの一覧表示
git tag -l
git tag -l "v1.*"              # パターンでフィルタ
```

### 1.2 セマンティックバージョニング

バージョン番号の慣習は **セマンティックバージョニング（SemVer）** です：

```
v メジャー . マイナー . パッチ
    ↓         ↓        ↓
  破壊的      新機能    バグ
  変更       追加      修正

例：
  v1.0.0 → v1.0.1  (パッチ: バグ修正)
  v1.0.0 → v1.1.0  (マイナー: 新機能、後方互換)
  v1.0.0 → v2.0.0  (メジャー: 破壊的変更)
```

プレリリース版: `v1.0.0-alpha`、`v1.0.0-beta.1`、`v1.0.0-rc.1`

### 1.3 GitHub Releases

**Release** はGitタグの上に構築されたGitHubの機能です。Releaseは以下を追加します：

- リリースタイトルと説明（リリースノート）
- バイナリアセット（ダウンロード可能なファイル）
- プレリリースまたは最新版の指定
- ディスカッションリンク

```
Gitタグ (v1.0.0)
    ↓
GitHub Release
├── タイトル: "Version 1.0.0 - 初回リリース"
├── リリースノート（変更履歴）
├── アセット: app-v1.0.0.zip, app-v1.0.0.tar.gz
├── プレリリース: いいえ
└── 最新版: はい
```

#### Releaseの作成手順

1. リポジトリ → **Releases**（右サイドバー）に移動
2. **Draft a new release** をクリック
3. タグを選択または作成（例：`v1.0.0`）
4. リリースタイトルを設定
5. リリースノートを記述（または自動生成）
6. 必要に応じてバイナリアセットをアップロード
7. **Publish release** をクリック

### 1.4 リリースノートの自動生成

GitHubはマージ済みPRとコントリビューターからリリースノートを自動生成できます。

**仕組み：**
1. Release作成時に **Generate release notes** をクリック
2. GitHubが前回リリース以降のマージ済みPRを一覧表示
3. ラベルに基づいてカテゴリ分け（機能、バグ修正など）

#### 自動生成のカスタマイズ

`.github/release.yml` を作成：

```yaml
changelog:
  exclude:
    labels:
      - ignore-for-release
    authors:
      - dependabot
  categories:
    - title: 🚀 新機能
      labels:
        - enhancement
        - feature
    - title: 🐛 バグ修正
      labels:
        - bug
        - fix
    - title: 📖 ドキュメント
      labels:
        - documentation
    - title: 🔧 メンテナンス
      labels:
        - chore
        - dependencies
    - title: その他の変更
      labels:
        - "*"
```

### 1.5 GitHub Actionsでリリースを自動化

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

### ✅ ハンズオン：Releaseの作成

1. タグを作成：
   ```bash
   git tag -a v0.1.0 -m "First release"
   git push origin v0.1.0
   ```
2. **Releases** → **Draft a new release** に移動
3. タグ `v0.1.0` を選択
4. **Generate release notes** をクリック
5. Releaseを公開

---

## Part 2：GitHub Pages（15分）

### 2.1 GitHub Pagesとは？

**GitHub Pages** はGitHubに組み込まれた無料の静的サイトホスティングサービスです。リポジトリからHTML、CSS、JavaScriptを直接配信できます。

```
リポジトリ（ソースファイル）
    ↓
GitHub Pages（ビルド＆配信）
    ↓
https://username.github.io/repo-name/
```

#### 利用可能範囲

| 機能 | Free | GitHub Team | Enterprise |
|------|:----:|:-----------:|:----------:|
| パブリックリポジトリ → 公開Pages | ✅ | ✅ | ✅ |
| プライベートリポジトリ → 公開Pages | ✅ | ✅ | ✅ |
| プライベートリポジトリ → 非公開Pages | ❌ | ❌ | 🔒 Enterprise |

> 💡 **非公開GitHub Pages**（アクセス制御付き）はGitHub Enterprise Cloud（🔒 Enterprise）が必要です。

### 2.2 GitHub Pagesの設定

#### 方法1：Settingsから（簡単）

1. **Settings** → **Pages**
2. **Source**：ブランチとフォルダを選択
   - ブランチ：`main`（または `gh-pages`）
   - フォルダ：`/ (root)` または `/docs`
3. **Save** をクリック
4. デプロイを待つ → サイトが公開されます！

#### 方法2：GitHub Actionsを使用（推奨）

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

### 2.3 GitHub Pagesサイトの種類

| 種類 | URL | リポジトリ名 |
|------|-----|-------------|
| **ユーザーサイト** | `username.github.io` | `username.github.io` |
| **Organizationサイト** | `orgname.github.io` | `orgname.github.io` |
| **プロジェクトサイト** | `username.github.io/repo` | 任意のリポジトリ |

### 2.4 静的サイトジェネレーター

GitHub Pagesはあらゆる静的サイトジェネレーターと連携できます：

| ジェネレーター | 言語 | 用途 |
|-------------|------|------|
| **Jekyll** | Ruby | ブログ、ドキュメント（組み込みサポート） |
| **Hugo** | Go | 高速ビルド、大規模サイト |
| **Next.js** | JavaScript | Reactベースの静的エクスポート |
| **VitePress** | JavaScript | Vueベースのドキュメント |
| **MkDocs** | Python | 技術ドキュメント |

> 💡 JekyllはGitHub Pagesに組み込みサポートがあり、Actionsワークフロー不要です。他のジェネレーターはGitHub Actionsワークフローでビルド・デプロイが必要です。

### 2.5 カスタムドメイン

GitHub Pagesに独自ドメインを使用できます：

1. **Settings** → **Pages** → **Custom domain**
2. ドメインを入力（例：`docs.example.com`）
3. DNSレコードを追加：

| タイプ | 名前 | 値 |
|--------|------|-----|
| CNAME | `docs` | `username.github.io` |
| A | `@` | `185.199.108.153`（+ 109, 110, 111） |

4. **Enforce HTTPS** を有効化 ✅

### ✅ ハンズオン：GitHub Pagesサイトのデプロイ

1. リポジトリのルートに `index.html` を作成：

```html
<!DOCTYPE html>
<html lang="ja">
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
    <p>このサイトはGitHub Pagesでホストされています。</p>
    <p>リポジトリ: <a href="https://github.com/USERNAME/REPO">GitHub</a></p>
</body>
</html>
```

2. `main` にプッシュ
3. **Settings** → **Pages** → Source: `main` ブランチ、`/ (root)` フォルダ
4. 1〜2分待って `https://username.github.io/repo-name/` にアクセス

---

## Part 3：GitHub Packages（15分）

### 3.1 GitHub Packagesとは？

**GitHub Packages** はGitHubに統合されたパッケージレジストリです。ソースコードと一緒にパッケージを公開、保存、利用できます。

```
ソースコード（リポジトリ）
    ↓ ビルド＆公開
GitHub Packages（レジストリ）
    ↓ インストール / プル
利用者（他のプロジェクト、CI/CD、ユーザー）
```

### 3.2 対応レジストリ

| レジストリ | エコシステム | URL |
|----------|-----------|-----|
| **npm** | JavaScript / Node.js | `npm.pkg.github.com` |
| **Maven** | Java | `maven.pkg.github.com` |
| **Gradle** | Java | `maven.pkg.github.com` |
| **NuGet** | .NET | `nuget.pkg.github.com` |
| **RubyGems** | Ruby | `rubygems.pkg.github.com` |
| **Container（Docker）** | Docker / OCIイメージ | `ghcr.io` |

### 3.3 ストレージと料金

| プラン | ストレージ | データ転送 |
|--------|----------|----------|
| **Free** | 500 MB | 1 GB / 月 |
| **Pro** | 2 GB | 10 GB / 月 |
| **Team** | 2 GB | 10 GB / 月 |
| **Enterprise** | 50 GB | 100 GB / 月 |

> 💡 パブリックパッケージはすべてのプランで **無制限** のストレージとデータ転送が可能です。

> 💰 最新の料金は [GitHub 料金ページ](https://github.com/pricing) をご確認ください。

### 3.4 コンテナレジストリ（ghcr.io）

**GitHub Container Registry**（`ghcr.io`）は最も多く使われるGitHub Packagesレジストリです。Docker/OCIコンテナイメージをホストします。

#### コンテナイメージの公開

```bash
# GitHub Container Registryにログイン
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin

# イメージをビルド
docker build -t ghcr.io/USERNAME/my-app:v1.0.0 .

# イメージをプッシュ
docker push ghcr.io/USERNAME/my-app:v1.0.0

# イメージをプル（別のマシンから）
docker pull ghcr.io/USERNAME/my-app:v1.0.0
```

#### GitHub Actionsで自動化

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

### 3.5 npmパッケージの公開

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

### 3.6 パッケージの公開範囲とアクセス

| 設定 | パブリックリポジトリ | プライベートリポジトリ |
|------|:------------------:|:--------------------:|
| パブリックパッケージ | ✅ | ✅ |
| プライベートパッケージ | ✅ | ✅ |
| Organizationアクセス制御 | ✅ | ✅ |
| リポジトリアクセスの継承 | ✅ | ✅ |

**パッケージの公開範囲を管理：**
1. プロフィールまたはOrganization → **Packages** に移動
2. パッケージを選択 → **Package settings**
3. 公開範囲を変更（パブリック/プライベート）
4. アクセスを管理（チーム、コラボレーター）

### ✅ ハンズオン：GitHub Packagesを探索

**演習1：パッケージを閲覧**

1. [github.com/features/packages](https://github.com/features/packages) にアクセス
2. プロフィール → **Packages** タブに移動
3. `ghcr.io` でパブリックなコンテナイメージを探索

**演習2：アセット付きReleaseの作成**

1. シンプルなプロジェクトを作成（例：README + スクリプト）
2. タグ付け：`git tag -a v1.0.0 -m "Initial release" && git push origin v1.0.0`
3. **Releases** → Releaseを編集
4. ZIPファイルをバイナリアセットとしてアップロード
5. Releaseを公開

**演習3：公開ワークフローの設定（オプション）**

1. リポジトリに `Dockerfile` または `package.json` を追加
2. 上記のActionsワークフローの1つを作成
3. タグをプッシュして公開をトリガー

---

## まとめ（10分）

### 今日学んだこと

- ✅ Gitタグとセマンティックバージョニング
- ✅ GitHub Releasesとリリースノートの自動生成
- ✅ GitHub Actionsでリリースを自動化
- ✅ GitHub Pagesで静的サイトをホスティング
- ✅ GitHub PackagesとContainer Registry（ghcr.io）
- ✅ CI/CDワークフローでパッケージを公開

### リリース＆デプロイ チェックリスト

| タスク | 状態 |
|--------|------|
| セマンティックバージョニングでタグを使用 | ☐ |
| `.github/release.yml` でリリースノートのカテゴリを設定 | ☐ |
| GitHub Actionsでリリースを自動化 | ☐ |
| GitHub Pagesサイトをデプロイ | ☐ |
| Pagesのカスタムドメインを設定（必要に応じて） | ☐ |
| GitHub Packagesにパッケージを公開 | ☐ |
| `ghcr.io` でコンテナイメージビルドを設定 | ☐ |

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

- 🏷️ **セマンティックバージョニングでリリースタグ** を開始しましょう
- 🌐 **ドキュメントサイトをデプロイ** — GitHub Pagesで公開
- 📦 **最初のパッケージを公開** — GitHub Packagesに登録
- 🔄 **リリースワークフローを自動化** — GitHub Actionsで設定
- 📖 **GitHub Skills** — [skills.github.com](https://skills.github.com/)

---

## 📚 参考リンク

- [Releasesについて](https://docs.github.com/ja/repositories/releasing-projects-on-github/about-releases)
- [自動生成リリースノート](https://docs.github.com/ja/repositories/releasing-projects-on-github/automatically-generated-release-notes)
- [GitHub Pages ドキュメント](https://docs.github.com/ja/pages)
- [GitHub Packages ドキュメント](https://docs.github.com/ja/packages)
- [Container Registryの操作](https://docs.github.com/ja/packages/working-with-a-github-packages-registry/working-with-the-container-registry)
- [セマンティックバージョニング](https://semver.org/lang/ja/)
