# ワークショップ1：GitHub開発サイクル一巡り（60分）

> 📖 [English version](../en/01-github-development-cycle.md)

## 🎯 このワークショップのゴール

- GitHubを使った開発の流れを**最初から最後まで一通り体験**する
- Repository、Issue、Branch、Pull Request、GitHub Actionsのつながりを理解する
- 実際に手を動かして、ひととおりの開発フローをやりきる

---

## 📋 アジェンダ

| 時間 | 内容 |
|------|------|
| 0:00 - 0:05 | 前回の振り返り & 今日のゴール |
| 0:05 - 0:15 | Step 1：リポジトリの作成 |
| 0:15 - 0:22 | Step 2：Issueの作成 |
| 0:22 - 0:32 | Step 3：ブランチ作成 & コード変更 |
| 0:32 - 0:42 | Step 4：Pull Request & マージ |
| 0:42 - 0:52 | Step 5：GitHub Actionsの設定 |
| 0:52 - 1:00 | まとめ |

---

## 前回の振り返り（5分）

前回の準備編で学んだことを思い出しましょう：
- GitHubアカウントの作成
- ソフトウェア開発の流れ（開発サイクル）
- Gitの基本的なしくみ

今回は、実際にGitHub上で開発サイクルを**ひと回り**体験します。

```
 ① Repository作成
      ↓
 ② Issue作成（課題を定義）
      ↓
 ③ Branch作成 & コード変更（開発）
      ↓
 ④ Pull Request & Merge（レビュー & 統合）
      ↓
 ⑤ GitHub Actions（自動化）
```

---

## Step 1：リポジトリの作成（10分）

### 1.1 GitHub上でリポジトリを作成

1. GitHubにログイン
2. 右上の **+** → **New repository** をクリック
3. 以下を入力：
   - **Repository name**: `my-first-project`
   - **Description**: `GitHubワークショップの練習用リポジトリ`
   - **Public** を選択
   - ✅ **Add a README file** にチェック
   - **Add .gitignore**: `None`（今回は不要）
   - **Choose a license**: `MIT License`
4. **Create repository** をクリック

### 1.2 ローカルにクローン

**🪟 Windows（PowerShell）の場合：**

```powershell
# リポジトリをクローン
git clone https://github.com/あなたのユーザー名/my-first-project.git

# ディレクトリに移動
cd my-first-project

# 内容を確認
dir
```

**🍎 Mac（ターミナル）の場合：**

```bash
# リポジトリをクローン
git clone https://github.com/あなたのユーザー名/my-first-project.git

# ディレクトリに移動
cd my-first-project

# 内容を確認
ls
```

### ✅ チェックポイント

- [ ] GitHubにリポジトリが作成された
- [ ] README.mdとLICENSEが存在する
- [ ] ローカルにクローンできた

---

## Step 2：Issueの作成（7分）

### 2.1 Issueとは

**Issue（イシュー）** は、タスクやバグ報告、やりたいことなどを管理するための仕組みです。  
かんたんに言うと、「やるべきこと」をチームで共有するための**To-Doリスト**のようなものです。

### 2.2 Issueを作成する

1. GitHubのリポジトリページで **Issues** タブをクリック
2. **New issue** をクリック
3. 以下を入力：

| 項目 | 入力内容 |
|------|---------|
| **Title** | `自己紹介ページを追加する` |
| **Description** | `README.mdに自己紹介の内容を追加します。名前、趣味、学んでいることを記載します。` |

4. 右側のサイドバーで：
   - **Labels**: `enhancement` を選択
   - **Assignees**: 自分を割り当て
5. **Submit new issue** をクリック

> 💡 Issue番号（`#1`）が自動で割り当てられます。この番号は後で使います。

### ✅ チェックポイント

- [ ] Issue `#1` が作成された
- [ ] ラベルとアサインが設定された

---

## Step 3：ブランチ作成 & コード変更（10分）

### 3.1 ブランチとは

**ブランチ（Branch）** は、メインのコードに影響を与えずに別の作業をするための仕組みです。  
木の「枝」のように、本流（main）から分かれて作業し、終わったら戻します。

```
main ──────●──────●──────────●──────●─── (安定版)
                   \        /
feature/profile ────●──●───  (作業用)
```

### 3.2 ブランチを作成して作業する

#### ⌨️ コマンドで操作する場合

以下のコマンドは Windows / Mac 共通です：

```bash
# 新しいブランチを作成して切り替え
git checkout -b feature/add-profile

# 現在のブランチを確認
git branch
```

#### 🖥️ VS Code で操作する場合

1. VS Code でクローンしたフォルダを開く（**File** → **Open Folder**）
2. 左下のブランチ名（`main`）をクリック
3. **Create new branch** を選択
4. ブランチ名 `feature/add-profile` を入力して **Enter**

> 💡 VS Code の Git 操作について詳しくは [ツールガイド](tool-guide.md) を参照してください。

### 3.3 ファイルを編集

#### ⌨️ コマンドで操作する場合

`README.md` をテキストエディタで開き、以下のように編集してください：

```markdown
# my-first-project

GitHubワークショップの練習用リポジトリ

## 自己紹介

- **名前**: （あなたの名前）
- **趣味**: （あなたの趣味）
- **学んでいること**: GitHub & Gitの使い方

## このリポジトリについて

GitHubワークショップで作成した最初のプロジェクトです。
```

#### 🖥️ VS Code で操作する場合

1. 左側のエクスプローラーで `README.md` をクリックして開く
2. 上記の内容に編集する
3. **Ctrl+S**（Mac: **Cmd+S**）で保存

> 💡 VS Code なら、ファイルの作成も **エクスプローラー** 上の「新しいファイル」アイコン（📄+）をクリックするだけで行えます。

### 3.4 変更をコミット & プッシュ

#### ⌨️ コマンドで操作する場合

以下のコマンドは Windows / Mac 共通です：

```bash
# 変更を確認
git status
git diff

# ステージングに追加
git add README.md

# コミット（Issue番号を参照）
git commit -m "自己紹介セクションを追加 #1"

# リモートにプッシュ
git push origin feature/add-profile
```

#### 🖥️ VS Code で操作する場合

1. 左サイドバーの **ソース管理** アイコン（分岐マーク）をクリック
2. 変更されたファイル（`README.md`）の横にある **+** をクリックしてステージング
3. 上部のテキストボックスに `自己紹介セクションを追加 #1` と入力
4. **✓（コミット）** ボタンをクリック
5. **…（メニュー）** → **Push** をクリック（または **Sync Changes** をクリック）

> 💡 コミットメッセージに `#1` を含めると、Issue #1 と自動的にリンクされます。

### ✅ チェックポイント

- [ ] `feature/add-profile` ブランチが作成された
- [ ] README.mdを編集した
- [ ] コミットしてプッシュした

---

## Step 4：Pull Request & マージ（10分）

### 4.1 Pull Request（PR）とは

**Pull Request（プルリクエスト）** は、自分の変更を`main`ブランチに取り込んでもらうための**お願い（リクエスト）** です。  
チーム開発では、PRを通じて他のメンバーにコードを確認してもらいます（コードレビュー）。

### 4.2 Pull Requestを作成

1. GitHubのリポジトリページに移動
2. **Compare & pull request** ボタンをクリック（または **Pull requests** タブ → **New pull request**）
3. 以下を入力：

| 項目 | 入力内容 |
|------|---------|
| **base** | `main` |
| **compare** | `feature/add-profile` |
| **Title** | `自己紹介セクションを追加` |
| **Description** | `Closes #1`（改行）`README.mdに自己紹介の情報を追加しました。` |

4. **Create pull request** をクリック

> 💡 `Closes #1` と書くと、このPRがマージされたときにIssue #1が自動的に閉じます。

### 4.3 Pull Requestの中身を確認しよう

PR画面ではいろいろな情報を見ることができます：
- **Conversation**: コメントやレビューのやりとり
- **Commits**: このPRに含まれるコミット（変更の記録）の一覧
- **Files changed**: 変更されたファイルの差分（何がどう変わったか）

### 4.4 【オプション】ペアでコードレビューを体験しよう 👥

> ワークショップに一緒に参加しているお友達や同僚がいたら、**お互いのPRをレビューし合ってみましょう！**  
> 一人で参加している場合はこのセクションをスキップして [4.5 マージする](#45-マージする) に進んでください。

実際のチーム開発では、コードをマージする前に他のメンバーにレビューしてもらうのが一般的です。ここでは、その流れを体験します。

#### Step A：コラボレーターを招待する

まず、相手にあなたのリポジトリへのアクセス権を付与します：

1. リポジトリページで **Settings** タブをクリック
2. 左メニューの **Collaborators** をクリック（パスワードの再入力が必要な場合があります）
3. **Add people** をクリック
4. ペア相手の **GitHubユーザー名** または **メールアドレス** を入力
5. 表示された候補から相手を選び、**Add (ユーザー名) to this repository** をクリック
6. 相手に招待メールが届くので、**Accept invitation** をクリックしてもらう

```
┌──────────────────────────────────────┐
│  Settings → Collaborators            │
│                                      │
│  Manage access                       │
│  ┌────────────────────────────────┐  │
│  │ 👤 あなた          Owner       │  │
│  │ 👤 ペア相手        Pending...  │  │
│  └────────────────────────────────┘  │
│                                      │
│  [Add people]                        │
└──────────────────────────────────────┘
```

> 💡 **コラボレーター（Collaborator）** とは、リポジトリに対して読み書きの権限を持つメンバーのことです。パブリックリポジトリは誰でも「見る」ことはできますが、「変更を加える」にはコラボレーターに追加される必要があります。

#### Step B：レビュアーをリクエストする

PRを作成した後（またはすでに作成済みのPRで）：

1. PR画面の右サイドバーにある **Reviewers** の歯車アイコンをクリック
2. ペア相手のユーザー名を検索して選択
3. 相手にレビューリクエストの通知が届く

#### Step C：レビューする（ペア相手の作業）

レビューを依頼された側は、以下の手順でレビューします：

1. 通知またはPRページから、対象のPull Requestを開く
2. **Files changed** タブをクリック
3. コードを確認し、気になる行にカーソルを合わせて **+** をクリック → コメントを入力
4. 全体を確認し終えたら、右上の **Review changes** をクリック
5. 以下のいずれかを選択：
   - **Comment** — コメントのみ（承認でも却下でもない）
   - **Approve** ✅ — 変更を承認する
   - **Request changes** — 修正を求める
6. **Submit review** をクリック

```
┌──────────────────────────────────────┐
│  Review changes                      │
│  ┌────────────────────────────────┐  │
│  │ いいね！自己紹介がわかりやすい  │  │
│  │ です。                         │  │
│  └────────────────────────────────┘  │
│                                      │
│  ○ Comment                           │
│  ● Approve                           │
│  ○ Request changes                   │
│                                      │
│              [Submit review]         │
└──────────────────────────────────────┘
```

> 💡 **レビューのコツ**: 初めてのレビューでは、「ここがわかりやすい！」「この部分はこう書くともっと良くなりそう」など、気軽にコメントしてみましょう。完璧なレビューをする必要はありません！

#### Step D：お互いのリポジトリでレビューし合う

ペア相手も同じ手順でPRを作成しているはずなので、**お互いのリポジトリを行き来して**レビューし合いましょう：

1. あなた → ペア相手のリポジトリでPRをレビュー
2. ペア相手 → あなたのリポジトリでPRをレビュー

### 4.5 マージする

1. PRの画面で変更内容を確認（レビューを受けた場合は、Approve されていることを確認）
2. **Merge pull request** をクリック
3. **Confirm merge** をクリック
4. マージ完了！🎉

### 4.6 ローカルを更新

以下のコマンドは Windows / Mac 共通です：

```bash
# mainブランチに戻る
git checkout main

# リモートの変更を取得
git pull

# 不要になったブランチを削除
git branch -d feature/add-profile
```

### ✅ チェックポイント

- [ ] Pull Requestが作成された
- [ ] PRにIssueへの参照（`Closes #1`）が含まれている
- [ ] （オプション）ペア相手をコラボレーターに追加し、レビューし合った
- [ ] マージが完了した
- [ ] Issue #1 が自動的にクローズされた

---

## Step 5：GitHub Actionsの設定（10分）

### 5.1 GitHub Actionsとは

**GitHub Actions** は、リポジトリで何かが起きたとき（pushやPR作成など）に、**決めておいた処理を自動で実行してくれる仕組み**です。  
たとえば「pushされたらテストを自動実行する」といったことができます。

### 5.2 簡単なワークフローを作成

1. GitHubのリポジトリページで **Actions** タブをクリック
2. **set up a workflow yourself** をクリック
3. ファイル名を `hello.yml` に変更
4. 以下の内容を入力：

```yaml
name: Hello GitHub Actions

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  greet:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Hello
        run: echo "Hello, GitHub Actions! 🚀"

      - name: Show date
        run: date

      - name: List files
        run: ls -la
```

5. **Commit changes** をクリック（直接 `main` ブランチにコミット）

### 5.3 ワークフローの実行を確認

1. **Actions** タブをクリック
2. 実行されたワークフローをクリック
3. **greet** ジョブをクリック
4. 各ステップのログを確認

> 💡 `main` ブランチへのpushをトリガーに自動実行されました！

### 5.4 ワークフローの構造

```
name:         ワークフロー名
on:           トリガー（いつ実行するか）
jobs:         ジョブの定義
  job名:
    runs-on:  実行環境
    steps:    処理ステップ
      - uses: 既存のアクションを利用
      - run:  コマンドを実行
```

### ✅ チェックポイント

- [ ] `.github/workflows/hello.yml` が作成された
- [ ] ワークフローが自動実行された
- [ ] Actionsタブで実行結果を確認できた

---

## まとめ（8分）

### 今日体験した開発サイクル

```
✅ Step 1: Repository作成     → コードの保管場所を用意
✅ Step 2: Issue作成          → やるべきことを定義
✅ Step 3: Branch & Commit    → 安全に開発
✅ Step 4: Pull Request       → レビュー & 統合
✅ Step 5: GitHub Actions     → 自動化
```

### 💡 GitHub Copilot：AIが開発をサポート

今回のワークショップでは触れませんでしたが、GitHubには**GitHub Copilot**というAIアシスタントが用意されています。開発サイクルのさまざまな場面で活用できます。

#### GitHub Copilot とは

GitHub Copilot は、AIを活用した**コーディングアシスタント**です。コードの自動補完、チャットによる質問応答、コードの説明・修正提案など、開発者の生産性を大幅に向上させます。

#### 開発サイクルでの活用イメージ

| 開発サイクルのフェーズ | Copilot の活用例 |
|----------------------|-----------------|
| **計画（Issue）** | Issue の説明文やテンプレートの作成支援 |
| **開発（Coding）** | コードの自動補完、関数の生成、リファクタリング提案 |
| **レビュー（PR）** | Pull Request の要約生成、コードレビュー支援 |
| **テスト（Actions）** | テストコードの自動生成 |
| **ドキュメント** | コメントやドキュメントの自動生成 |

#### Copilot の主な機能

- **Copilot Code Completion** — エディタ上でリアルタイムにコード補完を提案
- **Copilot Chat** — 自然言語でコードについて質問・相談できるチャット機能
- **Copilot Agent** — 複雑なタスクを自律的に実行するAIエージェント（Issue の対応やマルチファイル編集など）
- **Copilot for Pull Requests** — PR の要約を自動生成

> 📝 GitHub Copilot の詳細はワークショップ6で深掘りします。まずは「AIが開発サイクル全体をサポートしてくれる」ということを覚えておいてください。

### 今後のDeepDive予告

| ワークショップ | テーマ | 深掘りする内容 |
|-------------|--------|--------------|
| ワークショップ2 | Repository & Issue | テンプレート、ラベル、マイルストーン |
| ワークショップ3 | Branch & Pull Request | ブランチ戦略、コードレビュー、マージ戦略 |
| ワークショップ4 | Project管理 | ボードビュー、自動化、スプリント管理 |
| ワークショップ5 | GitHub Actions | CI/CD、自動テスト、デプロイ |
| ワークショップ6 | GitHub Copilot | Chat、Agent、AI活用 |
| ワークショップ7 | リリース＆デプロイ | タグ、Releases、Pages、Packages |
| ワークショップ8 | セキュリティ | Dependabot、スキャン、プロテクション |
| ワークショップ9 | 管理 | Repo、Org、Enterprise管理 |

---

## 📚 参考リンク

- [GitHub Flow](https://docs.github.com/ja/get-started/using-github/github-flow)
- [Issueの作成](https://docs.github.com/ja/issues/tracking-your-work-with-issues/creating-an-issue)
- [Pull Requestの作成](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)
- [GitHub Actions クイックスタート](https://docs.github.com/ja/actions/quickstart)
