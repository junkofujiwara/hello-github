# 🛠️ ツールガイド：GitHub Desktop & VS Code でのブランチ管理

> 📖 [English version](../en/tool-guide.md)

このガイドは、ワークショップシリーズの**補助資料**です。  
Git の操作をコマンドライン（CLI）以外のツールでも行えるように、**GitHub Desktop** と **VS Code** の使い方をまとめています。

---

## 📋 目次

1. [ツールの比較](#1-ツールの比較)
2. [GitHub Desktop](#2-github-desktop)
3. [VS Code での Git 操作](#3-vs-code-での-git-操作)
4. [Git CLI クイックリファレンス](#4-git-cli-クイックリファレンス)
5. [ツール連携のヒント](#5-ツール連携のヒント)

---

## 1. ツールの比較

| 項目 | Git CLI | GitHub Desktop | VS Code |
|------|---------|---------------|---------|
| **対象** | すべての開発者 | Git 初心者〜中級者 | コードを書く開発者 |
| **操作** | コマンド入力 | GUI（ボタン操作） | GUI + 統合ターミナル |
| **利点** | 全機能を使える、自動化しやすい | 視覚的でわかりやすい | コーディングと Git 操作を一画面で完結 |
| **インストール** | [git-scm.com](https://git-scm.com) | [desktop.github.com](https://desktop.github.com) | [code.visualstudio.com](https://code.visualstudio.com) |

> 💡 どのツールを使っても、裏側では同じ Git の操作が行われます。状況に応じて使い分けましょう。

---

## 2. GitHub Desktop

### 2.1 GitHub Desktop とは

**GitHub Desktop** は、GitHub が提供する**無料の GUI クライアント**です。  
コマンドを覚えなくても、ボタン操作だけで Git の基本操作ができます。

### 2.2 インストールと初期設定

1. [https://desktop.github.com](https://desktop.github.com) からダウンロード
2. インストール後、**Sign in to GitHub.com** をクリック
3. ブラウザで GitHub アカウントを認証
4. Git の設定（名前・メールアドレス）を確認して **Finish**

### 2.3 リポジトリのクローン

1. **File** → **Clone repository** を選択
2. GitHub.com タブで対象のリポジトリを選択（またはURLを入力）
3. ローカルの保存先を選択
4. **Clone** をクリック

```
┌─────────────────────────────────┐
│  Clone a repository             │
│                                 │
│  GitHub.com │ URL               │
│  ┌───────────────────────────┐  │
│  │ user/my-first-project     │  │
│  │ user/hello-github         │  │
│  └───────────────────────────┘  │
│                                 │
│  Local path: C:\GitHub\...      │
│                    [Clone]      │
└─────────────────────────────────┘
```

### 2.4 ブランチの作成と切り替え

#### 新しいブランチを作成する

1. 上部の **Current branch** ボタンをクリック
2. **New branch** をクリック
3. ブランチ名を入力（例：`feature/add-profile`）
4. **Create branch** をクリック

```
┌──────────────────────────────┐
│  Current branch ▼            │
│  ┌────────────────────────┐  │
│  │ 🔍 Filter              │  │
│  ├────────────────────────┤  │
│  │ ● main                 │  │
│  │   feature/add-profile  │  │
│  ├────────────────────────┤  │
│  │ [New branch]           │  │
│  └────────────────────────┘  │
└──────────────────────────────┘
```

#### ブランチを切り替える

1. **Current branch** ボタンをクリック
2. 切り替えたいブランチをクリック

### 2.5 変更のコミットとプッシュ

1. ファイルを編集すると、左側の **Changes** タブに変更が表示される
2. コミットに含めるファイルにチェックを入れる
3. 左下に**コミットメッセージ**を入力
4. **Commit to ブランチ名** をクリック
5. 上部の **Push origin** をクリックしてリモートに送信

```
┌──────────────────────────────────────┐
│  Changes (2)         History         │
│  ┌────────────────────────────────┐  │
│  │ ☑ README.md                   │  │
│  │ ☑ index.html                  │  │
│  └────────────────────────────────┘  │
│                                      │
│  Summary: 自己紹介を追加 #1          │
│  Description: (任意の説明)           │
│                                      │
│  [Commit to feature/add-profile]     │
└──────────────────────────────────────┘
```

### 2.6 Pull Request の作成

1. ブランチにコミット＆プッシュ済みの状態で
2. **Branch** → **Create pull request** を選択  
   （または上部に表示される **Create Pull Request** ボタンをクリック）
3. ブラウザが開き、GitHub 上で PR を作成する画面に移動

### 2.7 変更の取得（Pull / Fetch）

| 操作 | 説明 | ボタン |
|------|------|--------|
| **Fetch origin** | リモートの最新情報を確認（ダウンロードはしない） | 上部の **Fetch origin** |
| **Pull origin** | リモートの変更をローカルに取り込む | Fetch後に表示される **Pull origin** |

---

## 3. VS Code での Git 操作

### 3.1 VS Code の Git 機能

VS Code には Git 操作が**標準で組み込まれて**います。追加のインストールは不要です。  
左側のサイドバーにある **ソース管理（Source Control）** アイコン（分岐のマーク）をクリックすると操作パネルが開きます。

### 3.2 リポジトリのクローン

1. **Ctrl+Shift+P**（Mac: **Cmd+Shift+P**）でコマンドパレットを開く
2. `Git: Clone` と入力して選択
3. リポジトリの URL を入力（または **Clone from GitHub** を選択）
4. ローカルの保存先を選択

### 3.3 ブランチの作成と切り替え

#### ステータスバーから操作する（最も簡単）

VS Code の左下に現在のブランチ名が表示されています：

```
┌────────────────────────────────────────────┐
│  ... エディタ ...                           │
├────────────────────────────────────────────┤
│ ⎇ main  ←ここをクリック                     │
└────────────────────────────────────────────┘
```

1. 左下の **ブランチ名**（例：`main`）をクリック
2. 表示される一覧から：
   - 既存ブランチを選んで**切り替え**
   - **Create new branch** を選んで**新規作成**

#### コマンドパレットから操作する

| 操作 | コマンド |
|------|---------|
| ブランチ作成 | `Git: Create Branch` |
| ブランチ切り替え | `Git: Checkout to...` |
| ブランチ一覧 | `Git: Branch` |
| ブランチ削除 | `Git: Delete Branch` |

### 3.4 変更のコミットとプッシュ

1. ファイルを編集・保存する
2. 左サイドバーの **ソース管理** アイコンをクリック
3. 変更されたファイルの一覧が表示される
4. ファイル名の横の **+** をクリックしてステージング
5. 上部のテキストボックスに**コミットメッセージ**を入力
6. **✓（コミット）** ボタンをクリック
7. **…（メニュー）** → **Push** でリモートに送信

```
┌─────────────────────────────────┐
│  SOURCE CONTROL                 │
│  ┌───────────────────────────┐  │
│  │ メッセージを入力...  [✓]    │  │
│  └───────────────────────────┘  │
│                                 │
│  Changes                        │
│    M  README.md           [+]   │
│    U  index.html          [+]   │
│                                 │
│  Staged Changes                 │
│    M  README.md           [-]   │
└─────────────────────────────────┘
```

> 💡 **M** = 変更済み（Modified）、**U** = 未追跡（Untracked・新規ファイル）、**D** = 削除済み（Deleted）

### 3.5 マージコンフリクトの解決

VS Code はコンフリクトの解決に特に便利です：

1. コンフリクトが発生すると、ファイル内にマーカーが表示される
2. 各コンフリクト箇所に以下のボタンが表示される：
   - **Accept Current Change** — 自分の変更を採用
   - **Accept Incoming Change** — 相手の変更を採用
   - **Accept Both Changes** — 両方を残す
   - **Compare Changes** — 差分を比較

```
<<<<<<< HEAD (Current Change)
自分の変更内容
=======
相手の変更内容
>>>>>>> feature/other-branch (Incoming Change)
```

### 3.6 おすすめ VS Code 拡張機能

| 拡張機能 | 説明 |
|---------|------|
| **GitHub Pull Requests and Issues** | VS Code 内で PR と Issue を管理 |
| **GitLens** | 行ごとの変更履歴、ブランチの可視化など Git の機能を強化 |
| **GitHub Copilot** | AI によるコード補完・チャット |
| **Git Graph** | ブランチの分岐・マージを視覚的に表示 |

拡張機能のインストール方法：

1. 左サイドバーの **拡張機能** アイコン（四角のマーク）をクリック
2. 検索バーに拡張機能名を入力
3. **Install** をクリック

---

## 4. Git CLI クイックリファレンス

ワークショップで頻繁に使うコマンドのまとめです。  
**Windows（PowerShell / Git Bash）** でも **Mac（ターミナル）** でも共通です。

### ブランチ操作

```bash
# ブランチ一覧を表示
git branch

# リモートのブランチも含めて表示
git branch -a

# 新しいブランチを作成して切り替え
git checkout -b feature/my-feature

# ブランチを切り替え
git checkout main

# ブランチを削除
git branch -d feature/my-feature
```

### 変更の管理

```bash
# 変更の状態を確認
git status

# 変更の差分を確認
git diff

# 全ファイルをステージング
git add .

# 特定のファイルをステージング
git add README.md

# コミット
git commit -m "変更内容の説明"
```

### リモート操作

```bash
# リモートにプッシュ
git push origin feature/my-feature

# リモートから最新を取得
git pull origin main

# リモートの情報を取得（マージはしない）
git fetch
```

### 便利なコマンド

```bash
# コミット履歴を表示
git log --oneline

# 直前のコミットメッセージを修正
git commit --amend -m "修正したメッセージ"

# 変更を一時退避
git stash

# 退避した変更を復元
git stash pop
```

---

## 5. ツール連携のヒント

### 使い分けの目安

```
┌──────────────────────────────────────────────────┐
│  こんなときは…           → このツールがおすすめ     │
├──────────────────────────────────────────────────┤
│  はじめて Git を触る     → GitHub Desktop          │
│  コードを書きながら      → VS Code                 │
│  自動化・スクリプト      → Git CLI                 │
│  コンフリクト解決        → VS Code                 │
│  リポジトリの全体像把握   → GitHub Desktop / GitLens │
└──────────────────────────────────────────────────┘
```

### ツール間の併用

どのツールも同じ Git リポジトリを操作しているため、**自由に組み合わせて使えます**：

- GitHub Desktop でブランチを作成 → VS Code でコーディング → CLI でプッシュ
- VS Code でコミット → GitHub Desktop で履歴を確認
- CLI でクローン → GitHub Desktop で変更を管理

> 💡 大切なのは、**自分にとって使いやすいツールを選ぶこと**です。慣れてきたら複数のツールを場面に応じて使い分けると効率が上がります。

---

## 📚 参考リンク

- [GitHub Desktop ドキュメント](https://docs.github.com/ja/desktop)
- [VS Code での Git 操作](https://code.visualstudio.com/docs/sourcecontrol/overview)
- [VS Code 拡張機能: GitHub Pull Requests and Issues](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github)
- [VS Code 拡張機能: GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [Git 公式ドキュメント](https://git-scm.com/doc)
