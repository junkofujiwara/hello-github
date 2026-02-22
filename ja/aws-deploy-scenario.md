# ☁️ 番外編：GitHub Actions で AWS に Web サイトをデプロイする

> 📖 [English version](../en/aws-deploy-scenario.md)

このガイドは、ワークショップシリーズの**番外シナリオ**です。  
GitHub Actions を使って静的 Web サイトや Web アプリを **Amazon Web Services (AWS)** にデプロイする一連の流れをハンズオン形式で体験します。

---

## 📋 目次

1. [シナリオ概要](#1-シナリオ概要)
2. [事前準備](#2-事前準備)
3. [シナリオ A：S3 + CloudFront に静的サイトをデプロイ](#3-シナリオ-as3--cloudfront-に静的サイトをデプロイ)
4. [シナリオ B：Elastic Beanstalk に Web アプリをデプロイ](#4-シナリオ-belastic-beanstalk-に-web-アプリをデプロイ)
5. [シークレットの管理](#5-シークレットの管理)
6. [トラブルシューティング](#6-トラブルシューティング)
7. [クリーンアップ（リソース削除）](#7-クリーンアップリソース削除)

---

## 1. シナリオ概要

### このガイドで学べること

| トピック | 内容 |
|---------|------|
| **CI/CD パイプライン** | GitHub Actions でビルド → テスト → デプロイの流れを自動化 |
| **AWS 連携** | GitHub と AWS のサービス接続（OIDC / アクセスキー） |
| **シークレット管理** | AWS 認証情報を安全に扱う方法 |
| **静的サイト / Web アプリ** | 2 つのデプロイ先の違いと使い分け |

### デプロイ先の比較

| 項目 | S3 + CloudFront | Elastic Beanstalk |
|------|-----------------|-------------------|
| **用途** | 静的サイト（HTML/CSS/JS）、SPA | サーバーサイドアプリ（Node.js, Python 等） |
| **料金** | 無料利用枠あり（S3: 5GB、CloudFront: 1TB/月） | 無料利用枠あり（t2.micro 750 時間/月） |
| **セットアップ** | S3 バケット作成 + CloudFront 設定 | EB 環境作成でインフラを自動構築 |
| **カスタムドメイン** | ✅ Route 53 / CloudFront で対応 | ✅ Route 53 / CNAME で対応 |
| **SSL** | ✅ ACM（AWS Certificate Manager）で無料 | ✅ ACM で対応 |
| **おすすめ** | ポートフォリオ、ドキュメントサイト | API サーバー、フルスタックアプリ |

---

## 2. 事前準備

### 必要なアカウントとツール

| 項目 | 説明 |
|------|------|
| **GitHub アカウント** | リポジトリと Actions を使用 |
| **AWS アカウント** | [aws.amazon.com](https://aws.amazon.com/jp/free/) で無料アカウントを作成 |
| **AWS CLI**（オプション） | [インストール手順](https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/getting-started-install.html) |

> 💡 AWS の無料利用枠では、12 か月間の多数のサービス無料枠と、常時無料のサービスが利用できます。

### サンプル Web サイトを用意

リポジトリのルートに以下のファイルを作成します。

**`index.html`**：

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello AWS</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 40px 20px;
            background-color: #f5f5f5;
            color: #232f3e;
        }
        h1 { color: #ff9900; }
        .badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 12px;
            background-color: #ff9900;
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
    <h1>🚀 Hello AWS!</h1>
    <p><span class="badge">GitHub Actions</span> でデプロイされました</p>
    <div class="info">
        <h2>このサイトについて</h2>
        <p>GitHub Actions を使って AWS に自動デプロイされた Web サイトです。</p>
        <ul>
            <li>✅ 自動ビルド＆デプロイ</li>
            <li>✅ main ブランチへの push で自動更新</li>
            <li>✅ CloudFront で世界中に高速配信</li>
        </ul>
    </div>
</body>
</html>
```

---

## 3. シナリオ A：S3 + CloudFront に静的サイトをデプロイ

Amazon S3 に静的ファイルをホスティングし、CloudFront（CDN）で配信する、AWS で**もっとも一般的な静的サイトの構成**です。

### Step 1：S3 バケットを作成

1. [AWS マネジメントコンソール](https://console.aws.amazon.com) にサインイン
2. 「**S3**」を検索 → 「**バケットを作成**」
3. 以下を設定：

| 設定項目 | 値 |
|---------|-----|
| **バケット名** | `hello-github-site-<あなたのID>`（グローバルで一意） |
| **リージョン** | ap-northeast-1（東京）など |
| **パブリックアクセスのブロック** | **すべてチェックを外す**（静的ホスティングのため） |

4. 「**バケットを作成**」をクリック

### Step 2：静的ウェブサイトホスティングを有効化

1. 作成したバケットを開く → 「**プロパティ**」タブ
2. 一番下の「**静的ウェブサイトホスティング**」→ 「**編集**」
3. 以下を設定：

| 設定項目 | 値 |
|---------|-----|
| **静的ウェブサイトホスティング** | 有効 |
| **インデックスドキュメント** | `index.html` |
| **エラードキュメント** | `index.html` |

4. 「**変更の保存**」をクリック

### Step 3：バケットポリシーを設定

1. 「**アクセス許可**」タブ → 「**バケットポリシー**」→ 「**編集**」
2. 以下のポリシーを貼り付け（`<YOUR-BUCKET-NAME>` を置き換え）：

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::<YOUR-BUCKET-NAME>/*"
        }
    ]
}
```

### Step 4：IAM ユーザーを作成（GitHub Actions 用）

1. 「**IAM**」→ 「**ユーザー**」→ 「**ユーザーの作成**」
2. ユーザー名：`github-actions-deploy`
3. 「**ポリシーを直接アタッチ**」→ 以下のポリシーを作成してアタッチ：

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::<YOUR-BUCKET-NAME>",
                "arn:aws:s3:::<YOUR-BUCKET-NAME>/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudfront:CreateInvalidation"
            ],
            "Resource": "*"
        }
    ]
}
```

4. 「**セキュリティ認証情報**」タブ → 「**アクセスキーを作成**」
5. **アクセスキー ID** と **シークレットアクセスキー** を控える

### Step 5：GitHub Secrets に登録

GitHub リポジトリの **Settings** → **Secrets and variables** → **Actions** で以下を登録：

| Secret 名 | 値 |
|-----------|-----|
| `AWS_ACCESS_KEY_ID` | IAM ユーザーのアクセスキー ID |
| `AWS_SECRET_ACCESS_KEY` | IAM ユーザーのシークレットアクセスキー |

### Step 6：ワークフローを作成

`.github/workflows/deploy-s3.yml` を作成：

```yaml
name: Deploy to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-1

      - name: Sync files to S3
        run: |
          aws s3 sync . s3://<YOUR-BUCKET-NAME> \
            --exclude ".git/*" \
            --exclude ".github/*" \
            --exclude "README.md" \
            --delete
```

> 💡 `<YOUR-BUCKET-NAME>` は Step 1 で作成したバケット名に置き換えてください。

### Step 7：デプロイを確認

```bash
git add .
git commit -m "Add S3 deployment workflow"
git push origin main
```

1. **Actions** タブでワークフローの実行を確認
2. S3 の静的ウェブサイトホスティングの**エンドポイント URL** にアクセス
   - 例：`http://<YOUR-BUCKET-NAME>.s3-website-ap-northeast-1.amazonaws.com`

### （オプション）CloudFront を追加

CDN で高速配信 + HTTPS 対応するには、CloudFront ディストリビューションを作成します。

1. 「**CloudFront**」→ 「**ディストリビューションを作成**」
2. **オリジンドメイン**：S3 バケットの静的ウェブサイトエンドポイントを入力
3. **デフォルトルートオブジェクト**：`index.html`
4. 「**ディストリビューションを作成**」

CloudFront のキャッシュ無効化をワークフローに追加：

```yaml
      - name: Invalidate CloudFront cache
        run: |
          aws cloudfront create-invalidation \
            --distribution-id ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID }} \
            --paths "/*"
```

---

## 4. シナリオ B：Elastic Beanstalk に Web アプリをデプロイ

サーバーサイドのアプリケーション（Node.js、Python など）をデプロイする場合は Elastic Beanstalk を使います。

### Step 1：サンプル Node.js アプリを作成

**`package.json`**：

```json
{
  "name": "hello-aws",
  "version": "1.0.0",
  "scripts": {
    "start": "node server.js"
  }
}
```

**`server.js`**：

```javascript
const http = require('http');
const port = process.env.PORT || 8080;

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
    res.end(`
        <html>
        <head><title>Hello AWS Elastic Beanstalk</title></head>
        <body style="font-family: sans-serif; max-width: 800px; margin: 40px auto; padding: 20px;">
            <h1>🚀 Hello AWS Elastic Beanstalk!</h1>
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

### Step 2：Elastic Beanstalk 環境を作成

1. [AWS マネジメントコンソール](https://console.aws.amazon.com) にサインイン
2. 「**Elastic Beanstalk**」を検索 → 「**環境の作成**」
3. 以下を設定：

| 設定項目 | 値 |
|---------|-----|
| **環境枠** | ウェブサーバー環境 |
| **アプリケーション名** | `hello-github` |
| **環境名** | `hello-github-env` |
| **プラットフォーム** | Node.js |
| **プラットフォームバージョン** | 推奨バージョン |
| **アプリケーションコード** | サンプルアプリケーション |
| **プリセット** | 単一インスタンス（無料利用枠対象） |

4. 「**次へ**」→ デフォルト設定のまま進む → 「**送信**」

> 💡 環境の作成には数分かかります。

### Step 3：IAM ポリシーを追加

Step 4（シナリオ A）で作成した IAM ユーザーに、以下のポリシーを追加：

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "elasticbeanstalk:*",
                "s3:PutObject",
                "s3:GetObject",
                "s3:ListBucket",
                "autoscaling:*",
                "cloudformation:*",
                "ec2:*",
                "elasticloadbalancing:*",
                "logs:*"
            ],
            "Resource": "*"
        }
    ]
}
```

> ⚠️ 本番環境ではリソースを限定した最小権限のポリシーを使用してください。ここでは学習用に広めの権限を付与しています。

### Step 4：ワークフローを作成

`.github/workflows/deploy-eb.yml` を作成：

```yaml
name: Deploy to Elastic Beanstalk

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Create deployment package
        run: zip -r deploy.zip . -x ".git/*" ".github/*"

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-1

      - name: Upload to S3
        run: |
          aws s3 cp deploy.zip s3://elasticbeanstalk-ap-northeast-1-${{ secrets.AWS_ACCOUNT_ID }}/hello-github/deploy-${{ github.sha }}.zip

      - name: Create new EB application version
        run: |
          aws elasticbeanstalk create-application-version \
            --application-name hello-github \
            --version-label "ver-${{ github.sha }}" \
            --source-bundle S3Bucket="elasticbeanstalk-ap-northeast-1-${{ secrets.AWS_ACCOUNT_ID }}",S3Key="hello-github/deploy-${{ github.sha }}.zip"

      - name: Deploy to EB environment
        run: |
          aws elasticbeanstalk update-environment \
            --application-name hello-github \
            --environment-name hello-github-env \
            --version-label "ver-${{ github.sha }}"

      - name: Wait for deployment
        run: |
          aws elasticbeanstalk wait environment-updated \
            --application-name hello-github \
            --environment-names hello-github-env
```

### Step 5：追加の Secret を登録

| Secret 名 | 値 |
|-----------|-----|
| `AWS_ACCOUNT_ID` | AWS アカウント ID（12 桁の数字） |

### Step 6：デプロイを確認

```bash
git add .
git commit -m "Add Elastic Beanstalk deployment workflow"
git push origin main
```

1. **Actions** タブでワークフローが成功することを確認
2. Elastic Beanstalk コンソールで環境 URL をクリック
   - 例：`http://hello-github-env.ap-northeast-1.elasticbeanstalk.com`

---

## 5. シークレットの管理

### GitHub Secrets に登録する情報

| Secret 名 | 用途 | シナリオ |
|-----------|------|---------|
| `AWS_ACCESS_KEY_ID` | AWS 認証（アクセスキー） | A, B |
| `AWS_SECRET_ACCESS_KEY` | AWS 認証（シークレットキー） | A, B |
| `AWS_ACCOUNT_ID` | AWS アカウント ID | B |
| `CLOUDFRONT_DISTRIBUTION_ID` | CloudFront キャッシュ無効化 | A（オプション） |

### ベストプラクティス

| ルール | 説明 |
|--------|------|
| ✅ IAM ユーザーは専用にする | GitHub Actions 用の IAM ユーザーを個別に作成 |
| ✅ 最小権限の原則 | 必要最低限のアクション・リソースだけを許可 |
| ✅ アクセスキーを定期的にローテーション | 90 日ごとの更新を推奨 |
| ❌ ルートアカウントのキーは使わない | IAM ユーザーを必ず作成して使う |
| ❌ ハードコーディングしない | ワークフローやコードに直接書かない |

### OIDC を使った認証（上級・推奨）

アクセスキーの代わりに、**OpenID Connect (OIDC)** を使うとより安全です。  
アクセスキーの管理が不要になります。

#### 1. AWS で ID プロバイダーを作成

IAM → ID プロバイダー → プロバイダーの追加：

| 設定項目 | 値 |
|---------|-----|
| **プロバイダーのタイプ** | OpenID Connect |
| **プロバイダーの URL** | `https://token.actions.githubusercontent.com` |
| **対象者** | `sts.amazonaws.com` |

#### 2. IAM ロールを作成

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::<ACCOUNT-ID>:oidc-provider/token.actions.githubusercontent.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
                },
                "StringLike": {
                    "token.actions.githubusercontent.com:sub": "repo:<OWNER>/<REPO>:*"
                }
            }
        }
    ]
}
```

#### 3. ワークフローで使用

```yaml
    permissions:
      id-token: write
      contents: read

    steps:
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::<ACCOUNT-ID>:role/github-actions-role
          aws-region: ap-northeast-1
```

---

## 6. トラブルシューティング

### よくあるエラーと対処

| エラー | 原因 | 対処 |
|--------|------|------|
| `Access Denied` | IAM 権限不足 | IAM ポリシーに必要なアクション・リソースが含まれているか確認 |
| `NoSuchBucket` | S3 バケット名が違う | ワークフローのバケット名を確認 |
| `InvalidParameterValue` (EB) | アプリケーション名の不一致 | EB のアプリケーション名・環境名を確認 |
| `ExpiredToken` | 認証情報の有効期限切れ | アクセスキーを再作成、または OIDC に移行 |
| `403 Forbidden`（サイト表示時） | バケットポリシーの設定漏れ | バケットポリシーとパブリックアクセス設定を確認 |

### デバッグのヒント

1. **Actions タブのログを確認** — 各ステップの詳細な出力が表示される
2. **AWS CloudWatch Logs** — Elastic Beanstalk のアプリケーションログを確認
3. **EB ヘルスダッシュボード** — 環境のヘルスステータスとイベントを確認
4. **ワークフローにデバッグステップを追加**：

```yaml
- name: Debug - Check AWS identity
  run: aws sts get-caller-identity

- name: Debug - List S3 buckets
  run: aws s3 ls
```

---

## 7. クリーンアップ（リソース削除）

ハンズオンが終わったら、**課金を防ぐために必ず**リソースを削除しましょう。

### シナリオ A のクリーンアップ

```bash
# S3 バケットを空にして削除
aws s3 rm s3://<YOUR-BUCKET-NAME> --recursive
aws s3 rb s3://<YOUR-BUCKET-NAME>

# CloudFront ディストリビューションを無効化→削除（作成した場合）
# AWS コンソールから操作するのが簡単です
```

### シナリオ B のクリーンアップ

```bash
# Elastic Beanstalk 環境を終了
aws elasticbeanstalk terminate-environment \
  --environment-name hello-github-env

# アプリケーションを削除
aws elasticbeanstalk delete-application \
  --application-name hello-github \
  --terminate-env-by-force
```

### AWS コンソールから削除

1. **Elastic Beanstalk** → アプリケーション → 「**アクション**」→ 「**アプリケーションの削除**」
2. **S3** → バケットを選択 → 「**空にする**」→ 「**削除**」
3. **CloudFront** → ディストリビューション → 「**無効化**」→ 「**削除**」

### GitHub 側のクリーンアップ

- リポジトリの **Settings** → **Secrets** から不要になったシークレットを削除
- 不要になったワークフローファイルを削除（任意）

### IAM のクリーンアップ

- **IAM** → **ユーザー** → `github-actions-deploy` → 「**削除**」
- OIDC を使った場合：**ID プロバイダー** と **ロール** も削除

> ⚠️ Elastic Beanstalk 環境を削除すると、関連する EC2 インスタンスやロードバランサーもまとめて削除されます。

---

## 📚 参考リンク

- [Amazon S3 静的ウェブサイトホスティング](https://docs.aws.amazon.com/ja_jp/AmazonS3/latest/userguide/WebsiteHosting.html)
- [Amazon CloudFront ドキュメント](https://docs.aws.amazon.com/ja_jp/cloudfront/)
- [AWS Elastic Beanstalk ドキュメント](https://docs.aws.amazon.com/ja_jp/elasticbeanstalk/)
- [GitHub Actions for AWS](https://github.com/aws-actions)
- [aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials)
- [GitHub Actions と OIDC で AWS 認証](https://docs.github.com/ja/actions/security-guides/configuring-openid-connect-in-amazon-web-services)
- [GitHub Actions シークレットの管理](https://docs.github.com/ja/actions/security-guides/using-secrets-in-github-actions)
