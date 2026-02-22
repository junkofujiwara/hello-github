# ワークショップ2：Repository & Issue DeepDive（60分）

> 📖 [English version](../en/02-repository-and-issue.md)

## 🎯 このワークショップのゴール

- Repositoryの中身と設定を詳しく理解する
- Issueを使いこなすためのテクニックを身につける
- テンプレート、ラベル、マイルストーンの使い方をマスターする

---

## 📋 アジェンダ

| 時間 | 内容 |
|------|------|
| 0:00 - 0:05 | 前回の振り返り |
| 0:05 - 0:25 | Repository DeepDive |
| 0:25 - 0:50 | Issue DeepDive |
| 0:50 - 1:00 | まとめ |

---

## 前回の振り返り（5分）

ワークショップ1では、GitHub開発サイクルを一通り体験しました。  
今回は、その中から **Repository（リポジトリ）** と **Issue（イシュー）** にフォーカスして深掘りしていきます。

---

## Part 1：Repository DeepDive（20分）

### 1.1 リポジトリの構成要素

リポジトリには、ソースコード以外にも多くの大切なファイルやフォルダがあります。プロジェクトの「お作法」として覚えておきましょう。

```
my-project/
├── .github/                    # GitHub固有の設定
│   ├── workflows/              # GitHub Actionsのワークフロー
│   ├── ISSUE_TEMPLATE/         # Issueテンプレート
│   ├── PULL_REQUEST_TEMPLATE.md # PRテンプレート
│   └── CODEOWNERS              # コードオーナー設定
├── src/                        # ソースコード
├── docs/                       # ドキュメント
├── tests/                      # テスト
├── .gitignore                  # Git管理から除外するファイル
├── README.md                   # プロジェクトの説明
├── LICENSE                     # ライセンス
├── CONTRIBUTING.md             # コントリビューションガイド
└── CHANGELOG.md                # 変更履歴
```

### 1.2 README.md の書き方

README.mdは、プロジェクトを訪れた人が最初に目にする「プロジェクトの顔」です。  
何のプロジェクトなのか、どう使うのかがすぐにわかるように書きましょう。

```markdown
# プロジェクト名

簡潔な説明（1-2行）

## 機能

- 機能1の説明
- 機能2の説明

## インストール方法

​```bash
npm install my-project
​```

## 使い方

​```javascript
const myProject = require('my-project');
myProject.doSomething();
​```

## 開発環境のセットアップ

​```bash
git clone https://github.com/user/my-project.git
cd my-project
npm install
npm test
​```

## コントリビューション

コントリビューションは歓迎します！
詳細は [CONTRIBUTING.md](CONTRIBUTING.md) を参照してください。

## ライセンス

[MIT](LICENSE)
```

### 1.3 .gitignore の活用

`.gitignore` は、「このファイルはGitで管理しなくていいよ」と指定するための設定ファイルです。  
パスワードや秘密鍵、ビルドの一時ファイルなど、リポジトリに含めたくないものを記載します。

```gitignore
# OS固有ファイル
.DS_Store          # Mac
Thumbs.db          # Windows

# IDE設定
.vscode/
.idea/

# 依存関係
node_modules/
vendor/

# ビルド成果物
dist/
build/

# 環境変数（重要！）
.env
.env.local

# ログ
*.log
```

> 💡 [gitignore.io](https://www.toptal.com/developers/gitignore) で言語やフレームワークに応じた `.gitignore` を自動生成できます。

### 1.4 リポジトリの設定

**Settings** タブで設定できる主要な項目：

| 設定項目 | 説明 |
|----------|------|
| **General** | リポジトリ名、説明、Visibilityの変更 |
| **Collaborators** | チームメンバーの追加と権限管理 |
| **Branches** | ブランチ保護ルール（次回詳しく） |
| **Pages** | GitHub Pages（静的サイトホスティング） |
| **Secrets and variables** | Actions用のシークレット管理 |

### 1.5 リポジトリのVisibility

| 種類 | 説明 | 用途 |
|------|------|------|
| **Public** | 誰でも閲覧可能 | オープンソース、学習用 |
| **Private** | 招待されたメンバーのみ | 企業プロジェクト、個人プロジェクト |

### ✅ ハンズオン：リポジトリを整備する

ワークショップ1で作成したリポジトリに以下を追加してください：

1. `.gitignore` ファイルを作成
2. `README.md` を上記のテンプレートに沿って充実させる

**🪟 Windows（PowerShell）の場合：**

```powershell
# ブランチを作成
git checkout -b improve/repository-setup

# .gitignore を作成
@("node_modules/", ".env", ".DS_Store", "Thumbs.db") | Out-File -Encoding utf8 .gitignore

# コミット & プッシュ
git add .
git commit -m "リポジトリの基本設定を追加"
git push origin improve/repository-setup
```

**🍎 Mac（ターミナル）の場合：**

```bash
# ブランチを作成
git checkout -b improve/repository-setup

# .gitignore を作成
echo "node_modules/" > .gitignore
echo ".env" >> .gitignore
echo ".DS_Store" >> .gitignore
echo "Thumbs.db" >> .gitignore

# コミット & プッシュ
git add .
git commit -m "リポジトリの基本設定を追加"
git push origin improve/repository-setup
```

---

## Part 2：Issue DeepDive（25分）

### 2.1 Issueの役割

Issueはバグ報告だけでなく、いろいろな目的に使える万能ツールです：

| 用途 | 例 |
|------|-----|
| 🐛 **バグ報告** | 「ログインボタンが押しても反応しない」 |
| ✨ **機能のリクエスト** | 「ダークモードを追加してほしい」 |
| 📋 **タスクの管理** | 「データベースの設計をする」 |
| 💬 **相談・議論** | 「どのフレームワークを使うか話し合い」 |
| 📝 **ドキュメント** | 「APIドキュメントを書き直す」 |

### 2.2 良いIssueの書き方

#### バグ報告の例

```markdown
## バグの概要
ログインページでパスワードを入力してもログインできない。

## 再現手順
1. https://example.com/login にアクセス
2. ユーザー名とパスワードを入力
3. 「ログイン」ボタンをクリック
4. エラーメッセージ「認証に失敗しました」が表示される

## 期待される動作
正しい認証情報でログインできること。

## 実際の動作
常に「認証に失敗しました」が表示される。

## 環境
- OS: Windows 11 / macOS Sequoia
- ブラウザ: Chrome 120
- バージョン: v2.1.0

## スクリーンショット
（必要に応じて添付）
```

#### 機能要望の例

```markdown
## 概要
ユーザーがダークモードを利用できるようにする。

## 背景・動機
- 夜間の利用時に目が疲れるとのフィードバックが多い
- 競合サービスではダークモードが標準搭載

## 提案する実装
- 設定画面にテーマ切り替えオプションを追加
- ライト/ダーク/システム設定の3オプション
- CSS変数を使ったテーマ管理

## 完了条件
- [ ] テーマ切り替えUIの実装
- [ ] ダークモードのCSS作成
- [ ] テーマ設定の永続化
```

### 2.3 ラベル（Labels）

ラベルは、IssueやPRに付ける**分類タグ**です。色分けされるので、一目で種類がわかります。

#### デフォルトラベル

| ラベル | 色 | 用途 |
|--------|-----|------|
| `bug` | 🔴 赤 | バグ報告 |
| `enhancement` | 🔵 青 | 機能追加 |
| `documentation` | 🟣 紫 | ドキュメント |
| `good first issue` | 🟢 緑 | 初心者向けの課題 |
| `help wanted` | 🟡 黄 | 助けが欲しい |
| `duplicate` | ⚪ グレー | 重複 |
| `invalid` | ⚪ グレー | 無効 |
| `wontfix` | ⚪ グレー | 対応しない |

#### カスタムラベルの作成

**Issues** → **Labels** → **New label** で独自のラベルを作成できます。

おすすめのカスタムラベル：

| ラベル | 用途 |
|--------|------|
| `priority: high` | 優先度：高 |
| `priority: medium` | 優先度：中 |
| `priority: low` | 優先度：低 |
| `status: in-progress` | 作業中 |
| `status: blocked` | ブロック中 |
| `type: task` | タスク |
| `type: question` | 質問 |

### 2.4 マイルストーン（Milestones）

マイルストーンは、複数のIssueを「バージョン」や「リリース」ごとにまとめてグループ管理する仕組みです。  
「v1.0に必要な機能はどこまで進んでいるか？」が一目でわかるようになります。

#### マイルストーンの作成

1. **Issues** → **Milestones** → **New milestone**
2. 以下を設定：
   - **Title**: `v1.0 リリース`
   - **Due date**: 期日
   - **Description**: `最初のリリースに含める機能`
3. **Create milestone** をクリック

#### マイルストーンの活用

```
マイルストーン: v1.0 リリース
├── Issue #1: ログイン機能の実装     ✅ Closed
├── Issue #2: ユーザー登録機能       ✅ Closed
├── Issue #3: プロフィール表示       🔵 Open
└── Issue #4: パスワードリセット     🔵 Open

進捗: ████████░░ 50% (2/4完了)
```

### 2.5 Issueテンプレート

Issueテンプレートを使うと、あらかじめ決まったフォーマットでIssueを作成できます。  
バグ報告なら「再現手順」、機能要望なら「提案の背景」といった項目を自動で表示してくれるので、書き忘れを防げます。

#### テンプレートの作成方法

1. リポジトリの **Settings** → **General** → **Features** セクション
2. **Issues** の **Set up templates** をクリック
3. テンプレートを追加

#### バグ報告テンプレートの例

`.github/ISSUE_TEMPLATE/bug_report.md` を作成：

```markdown
---
name: バグ報告
about: バグを報告する
title: '[BUG] '
labels: bug
assignees: ''
---

## バグの概要
<!-- バグの簡潔な説明 -->

## 再現手順
1. 
2. 
3. 

## 期待される動作
<!-- 本来どう動くべきか -->

## 実際の動作
<!-- 実際にどう動いたか -->

## 環境
- OS: Windows / macOS
- ブラウザ: 
- バージョン: 

## スクリーンショット
<!-- 必要に応じて添付 -->
```

#### 機能要望テンプレートの例

`.github/ISSUE_TEMPLATE/feature_request.md` を作成：

```markdown
---
name: 機能要望
about: 新しい機能を提案する
title: '[FEATURE] '
labels: enhancement
assignees: ''
---

## 概要
<!-- 機能の簡潔な説明 -->

## 背景・動機
<!-- なぜこの機能が必要か -->

## 提案する実装
<!-- どのように実装するか -->

## 完了条件
- [ ] 
- [ ] 
- [ ] 
```

### 2.6 Issueの便利な機能

#### タスクリスト

Issue内でチェックリストを使えます：

```markdown
## やること
- [x] デザインの確定
- [x] データベーステーブル作成
- [ ] API実装
- [ ] フロントエンド実装
- [ ] テスト作成
```

進捗がIssue一覧でプログレスバーとして表示されます。

#### Issue間の参照

```markdown
関連Issue: #5
ブロックされている: #3 の完了を待つ必要がある
```

#### キーワードによるIssueの自動クローズ

PRやコミットメッセージに以下のキーワードを書くと、マージ時にIssueが自動的にクローズ（完了）されます。とても便利な機能です！

| キーワード | 例 |
|-----------|-----|
| `close` / `closes` / `closed` | `Closes #1` |
| `fix` / `fixes` / `fixed` | `Fixes #1` |
| `resolve` / `resolves` / `resolved` | `Resolves #1` |

### ✅ ハンズオン：Issueを活用する

以下の作業を行ってください：

1. **ラベルを作成**（3つ以上のカスタムラベル）
2. **マイルストーンを作成**
3. **Issueテンプレートを作成**（バグ報告 & 機能要望）
4. **テンプレートを使ってIssueを3つ以上作成**
   - 1つはバグ報告テンプレートを使用
   - 1つは機能要望テンプレートを使用
   - 各Issueにラベルとマイルストーンを設定

---

## まとめ（10分）

### 今日学んだこと

- ✅ リポジトリの構成要素（README、.gitignore、LICENSE など）
- ✅ リポジトリの設定とVisibility（公開・非公開）
- ✅ わかりやすいIssueの書き方
- ✅ ラベルとマイルストーンの活用法
- ✅ Issueテンプレートの作り方

### 次回予告：ワークショップ3「Branch & Pull Request DeepDive」

- ブランチ戦略（GitHub Flow、Git Flow）
- Pull Requestのベストプラクティス
- コードレビューの進め方
- マージ戦略（Merge、Squash、Rebase）

---

## 📚 参考リンク

- [リポジトリの作成と管理](https://docs.github.com/ja/repositories)
- [Issue について](https://docs.github.com/ja/issues/tracking-your-work-with-issues/about-issues)
- [Issueテンプレートの設定](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests)
- [.gitignoreについて](https://docs.github.com/ja/get-started/getting-started-with-git/ignoring-files)
- [ライセンスの選び方](https://choosealicense.com/)
