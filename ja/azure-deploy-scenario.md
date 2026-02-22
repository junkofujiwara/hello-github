# ☁️ 番外編：GitHub Actions で Azure に Web サイトをデプロイする

> 📖 [English version](../en/azure-deploy-scenario.md)

このガイドは、ワークショップシリーズの**番外シナリオ**です。  
GitHub Actions を使って静的 Web サイトや Web アプリを **Microsoft Azure** にデプロイする一連の流れをハンズオン形式で体験します。

---

## 📋 目次

1. [シナリオ概要](#1-シナリオ概要)
2. [事前準備](#2-事前準備)
3. [シナリオ A：Azure Static Web Apps にデプロイ](#3-シナリオ-aazure-static-web-apps-にデプロイ)
4. [シナリオ B：Azure App Service にデプロイ](#4-シナリオ-bazure-app-service-にデプロイ)
5. [シークレットの管理](#5-シークレットの管理)
6. [トラブルシューティング](#6-トラブルシューティング)
7. [クリーンアップ（リソース削除）](#7-クリーンアップリソース削除)

---

## 1. シナリオ概要

### このガイドで学べること

| トピック | 内容 |
|---------|------|
| **CI/CD パイプライン** | GitHub Actions でビルド → テスト → デプロイの流れを自動化 |
| **Azure 連携** | GitHub と Azure のサービス接続の仕組み |
| **シークレット管理** | 機密情報を安全に扱う方法 |
| **静的サイト / Web アプリ** | 2 つのデプロイ先の違いと使い分け |

### デプロイ先の比較

| 項目 | Azure Static Web Apps | Azure App Service |
|------|----------------------|-------------------|
| **用途** | 静的サイト（HTML/CSS/JS）、SPA | サーバーサイドアプリ（Node.js, Python 等） |
| **料金** | Free プランあり | Free プラン（F1）あり |
| **セットアップ** | 非常に簡単（GitHub 連携が組み込み） | やや手順が多い |
| **カスタムドメイン** | ✅ 対応 | ✅ 対応 |
| **SSL** | ✅ 自動（無料） | ✅ 対応 |
| **おすすめ** | ポートフォリオ、ドキュメントサイト | API サーバー、フルスタックアプリ |

---

## 2. 事前準備

### 必要なアカウントとツール

| 項目 | 説明 |
|------|------|
| **GitHub アカウント** | リポジトリと Actions を使用 |
| **Azure アカウント** | [azure.microsoft.com](https://azure.microsoft.com/ja-jp/free/) で無料アカウントを作成 |
| **Azure CLI**（オプション） | [インストール手順](https://learn.microsoft.com/ja-jp/cli/azure/install-azure-cli) |

> 💡 Azure の無料アカウントでは、12 か月間の無料サービスと $200 のクレジットが付与されます。

### サンプル Web サイトを用意

リポジトリのルートに以下のファイルを作成します。

**`index.html`**：

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello Azure</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 40px 20px;
            background-color: #f0f6ff;
            color: #24292f;
        }
        h1 { color: #0078d4; }
        .badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 12px;
            background-color: #0078d4;
            color: white;
            font-size: 14px;
        }
        .info {
            background: white;
            border-radius: 8px;
            padding: 20px;
            margin-top: 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <h1>🚀 Hello Azure!</h1>
    <p><span class="badge">GitHub Actions</span> でデプロイされました</p>
    <div class="info">
        <h2>このサイトについて</h2>
        <p>GitHub Actions を使って Azure に自動デプロイされた Web サイトです。</p>
        <ul>
            <li>✅ 自動ビルド＆デプロイ</li>
            <li>✅ main ブランチへの push で自動更新</li>
            <li>✅ Pull Request でプレビュー環境を作成</li>
        </ul>
    </div>
</body>
</html>
```

---

## 3. シナリオ A：Azure Static Web Apps にデプロイ

Static Web Apps は、静的サイトのデプロイに**もっとも簡単な方法**です。  
Azure ポータルから GitHub リポジトリを接続するだけで、ワークフローが自動生成されます。

### Step 1：Azure ポータルで Static Web App を作成

1. [Azure ポータル](https://portal.azure.com) にサインイン
2. 「**リソースの作成**」→ 「**Static Web App**」を検索 → 「**作成**」
3. 以下を設定：

| 設定項目 | 値 |
|---------|-----|
| **サブスクリプション** | 自分のサブスクリプション |
| **リソースグループ** | `rg-hello-github`（新規作成） |
| **名前** | `swa-hello-github`（任意） |
| **プランの種類** | Free |
| **リージョン** | East Asia（東日本）など |
| **ソース** | GitHub |

4. 「**GitHub でサインイン**」→ リポジトリを選択
5. ビルドの設定：

| 設定項目 | 値 |
|---------|-----|
| **ビルドのプリセット** | Custom |
| **アプリの場所** | `/` |
| **API の場所** | （空欄） |
| **出力先** | （空欄） |

6. 「**確認と作成**」→ 「**作成**」

### Step 2：自動生成されたワークフローを確認

Azure が自動的にリポジトリに `.github/workflows/azure-static-web-apps-xxxx.yml` を作成します。

```yaml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main

jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
          lfs: false

      - name: Build And Deploy
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: "upload"
          app_location: "/"
          api_location: ""
          output_location: ""

  close_pull_request_job:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request Job
    steps:
      - name: Close Pull Request
        id: closepullrequest
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: "close"
```

> 💡 `AZURE_STATIC_WEB_APPS_API_TOKEN` は Azure が自動でリポジトリの Secrets に登録します。

### Step 3：デプロイを確認

1. **Actions** タブでワークフローの実行を確認
2. Azure ポータルで Static Web App を開き、**URL** をクリック
3. `https://xxxx.azurestaticapps.net` でサイトが表示されることを確認

### Step 4：変更を加えてみる

```bash
# ブランチを作成
git checkout -b feature/update-site

# index.html を編集（例：タイトルを変更）
# コミット＆プッシュ
git add index.html
git commit -m "Update site title"
git push origin feature/update-site
```

Pull Request を作成すると、**プレビュー環境**が自動的に作られます！  
PR のコメントにプレビュー URL が投稿されるので確認しましょう。

---

## 4. シナリオ B：Azure App Service にデプロイ

サーバーサイドのアプリケーション（Node.js、Python など）をデプロイする場合は App Service を使います。

### Step 1：サンプル Node.js アプリを作成

**`package.json`**：

```json
{
  "name": "hello-azure",
  "version": "1.0.0",
  "scripts": {
    "start": "node server.js"
  }
}
```

**`server.js`**：

```javascript
const http = require('http');
const port = process.env.PORT || 3000;

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
    res.end(`
        <html>
        <head><title>Hello Azure App Service</title></head>
        <body style="font-family: sans-serif; max-width: 800px; margin: 40px auto; padding: 20px;">
            <h1>🚀 Hello Azure App Service!</h1>
            <p>GitHub Actions でデプロイされた Node.js アプリです。</p>
            <p>デプロイ時刻: ${new Date().toISOString()}</p>
        </body>
        </html>
    `);
});

server.listen(port, () => {
    console.log(`Server running on port ${port}`);
});
```

### Step 2：Azure ポータルで App Service を作成

1. [Azure ポータル](https://portal.azure.com) にサインイン
2. 「**リソースの作成**」→ 「**Web アプリ**」→ 「**作成**」
3. 以下を設定：

| 設定項目 | 値 |
|---------|-----|
| **サブスクリプション** | 自分のサブスクリプション |
| **リソースグループ** | `rg-hello-github` |
| **名前** | `app-hello-github`（グローバルで一意） |
| **公開** | コード |
| **ランタイムスタック** | Node 20 LTS |
| **OS** | Linux |
| **リージョン** | East Asia |
| **App Service プラン** | Free (F1) |

4. 「**確認と作成**」→ 「**作成**」

### Step 3：発行プロファイルを取得してシークレットに登録

1. Azure ポータルで作成した App Service を開く
2. 「**概要**」→ 「**発行プロファイルの取得**」をクリック（`.publishsettings` ファイルがダウンロードされる）
3. GitHub リポジトリの **Settings** → **Secrets and variables** → **Actions**
4. 「**New repository secret**」をクリック：
   - **Name**: `AZURE_WEBAPP_PUBLISH_PROFILE`
   - **Secret**: ダウンロードしたファイルの中身をすべて貼り付け

### Step 4：ワークフローを作成

`.github/workflows/azure-webapp.yml` を作成：

```yaml
name: Deploy to Azure App Service

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Deploy to Azure Web App
        uses: azure/webapps-deploy@v3
        with:
          app-name: 'app-hello-github'
          publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
          package: .
```

### Step 5：デプロイを確認

```bash
git add .
git commit -m "Add Azure App Service deployment workflow"
git push origin main
```

1. **Actions** タブでワークフローが成功することを確認
2. `https://app-hello-github.azurewebsites.net` にアクセスしてサイトを確認

---

## 5. シークレットの管理

### GitHub Secrets の仕組み

| 項目 | 説明 |
|------|------|
| **保存場所** | リポジトリの Settings → Secrets and variables → Actions |
| **暗号化** | 保存時に暗号化され、ログにも表示されない |
| **利用方法** | ワークフロー内で `${{ secrets.SECRET_NAME }}` で参照 |
| **スコープ** | リポジトリ単位、Organization 単位、Environment 単位 |

### ベストプラクティス

| ルール | 説明 |
|--------|------|
| ✅ Secrets を使う | 接続文字列やトークンは必ず Secrets に保存 |
| ✅ 最小権限 | 必要最低限のアクセス権だけを付与 |
| ✅ 定期的にローテーション | シークレットの値を定期的に変更 |
| ❌ ハードコーディングしない | ワークフローやコードに直接書かない |
| ❌ ログに出力しない | `echo` や `print` で表示しない |

### Azure サービスプリンシパル（上級）

発行プロファイルの代わりに、サービスプリンシパル（Azure AD アプリ登録）を使うとより安全です。

```bash
# Azure CLI でサービスプリンシパルを作成
az ad sp create-for-rbac \
  --name "github-actions-deploy" \
  --role contributor \
  --scopes /subscriptions/{subscription-id}/resourceGroups/rg-hello-github \
  --sdk-auth
```

出力された JSON を `AZURE_CREDENTIALS` シークレットとして登録し、`azure/login@v2` アクションで使用します。

---

## 6. トラブルシューティング

### よくあるエラーと対処

| エラー | 原因 | 対処 |
|--------|------|------|
| `Error: Publish profile is not valid` | 発行プロファイルの内容が不正 | Azure ポータルから再取得して、改行を含めてすべてコピー |
| `Resource not found` | App Service 名が違う | ワークフローの `app-name` と Azure の名前が一致しているか確認 |
| `Authentication failed` | シークレットの有効期限切れ | 発行プロファイルを再取得、またはサービスプリンシパルの期限を確認 |
| `Build failed` | 依存関係のインストールエラー | `package.json` と Node.js バージョンを確認 |
| `404 Not Found`（デプロイ後） | アプリの配置パスが違う | `app_location` や `package` パスを確認 |

### デバッグのヒント

1. **Actions タブのログを確認** — 各ステップの詳細な出力が表示される
2. **Azure ポータルの「ログストリーム」** — App Service のリアルタイムログを確認
3. **ワークフローにデバッグステップを追加**：

```yaml
- name: Debug - List files
  run: |
    echo "Current directory:"
    pwd
    echo "Files:"
    ls -la
```

---

## 7. クリーンアップ（リソース削除）

ハンズオンが終わったら、**課金を防ぐために必ず**リソースを削除しましょう。

### Azure ポータルから削除

1. [Azure ポータル](https://portal.azure.com) → 「**リソースグループ**」
2. `rg-hello-github` を選択
3. 「**リソースグループの削除**」→ 名前を入力して確認 → 「**削除**」

### Azure CLI から削除

```bash
az group delete --name rg-hello-github --yes --no-wait
```

### GitHub 側のクリーンアップ

- リポジトリの **Settings** → **Secrets** から不要になったシークレットを削除
- Static Web Apps の場合：自動生成されたワークフローファイルも削除（任意）

> ⚠️ リソースグループを削除すると、その中のすべてのリソース（Static Web App、App Service 等）がまとめて削除されます。

---

## 📚 参考リンク

- [Azure Static Web Apps のドキュメント](https://learn.microsoft.com/ja-jp/azure/static-web-apps/)
- [Azure App Service のドキュメント](https://learn.microsoft.com/ja-jp/azure/app-service/)
- [GitHub Actions for Azure](https://github.com/Azure/actions)
- [Azure/static-web-apps-deploy アクション](https://github.com/Azure/static-web-apps-deploy)
- [Azure/webapps-deploy アクション](https://github.com/Azure/webapps-deploy)
- [GitHub Actions シークレットの管理](https://docs.github.com/ja/actions/security-guides/using-secrets-in-github-actions)
