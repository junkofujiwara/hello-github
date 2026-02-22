# ワークショップ3：Branch & Pull Request DeepDive（60分）

> 📖 [English version](../en/03-branch-and-pullrequest.md)

## 🎯 このワークショップのゴール

- ブランチ戦略を理解し、目的に合ったブランチ運用ができるようになる
- Pull Requestを使ったコードレビューの流れを実際に体験する
- 3種類のマージ方法の違いを知り、使い分けられるようになる

---

## 📋 アジェンダ

| 時間 | 内容 |
|------|------|
| 0:00 - 0:05 | 前回の振り返り |
| 0:05 - 0:20 | Branch DeepDive |
| 0:20 - 0:40 | Pull Request DeepDive |
| 0:40 - 0:55 | ハンズオン：コードレビュー実践 |
| 0:55 - 1:00 | まとめ |

---

## 前回の振り返り（5分）

ワークショップ2では、Repository と Issue を深掘りしました。  
今回は、開発サイクルの中核である **Branch** と **Pull Request** を深掘りします。

---

## Part 1：Branch DeepDive（15分）

### 1.1 ブランチの仕組み

ブランチは、コミット履歴を指し示す**ポインタ（しおり）** のようなものです。  
新しいブランチを作ると、同じコミットを指す新しいポインタがもう1つ作られます。

```
       ← Commit A ← Commit B ← Commit C
                                    ↑
                                  main
                                    ↑
                            feature/login
```

ブランチ上で新しいコミットをすると、そのブランチのポインタだけが前に進みます（mainは動きません）：

```
       ← Commit A ← Commit B ← Commit C ← Commit D
                                    ↑           ↑
                                  main    feature/login
```

### 1.2 ブランチ戦略

#### GitHub Flow（初心者にもおすすめ）

もっともシンプルで広く使われているブランチ戦略です。まずはこれを覚えましょう。

```
main ──●──────●──────────●──────●──────●──── (常にデプロイ可能)
        \    /    \      /       \    /
         ●──●      ●──●──●       ●──●
       feature/A  feature/B    feature/C
```

**ルール：**
1. `main` ブランチは常にデプロイ可能な状態を保つ
2. 作業はすべて `main` からブランチを切って行う
3. Pull Requestを使ってレビュー後にマージする
4. マージしたらすぐにデプロイする

#### Git Flow

より厳密なブランチ管理が必要な大規模プロジェクト向けの戦略です。  
GitHub Flowに慣れてから覚えれば十分です。

```
main    ──●───────────────────●───────────── (リリース版)
           \                 /
develop ────●──●──●──●──●──●──●──●──── (開発版)
              \  / \      /
feature/A ─────●    \    /
                 feature/B ──●──●
```

**ブランチの種類：**

| ブランチ | 用途 | 命名例 |
|---------|------|--------|
| `main` | リリース済みコード | - |
| `develop` | 開発中の統合ブランチ | - |
| `feature/*` | 新機能開発 | `feature/user-auth` |
| `release/*` | リリース準備 | `release/v1.0` |
| `hotfix/*` | 緊急バグ修正 | `hotfix/login-fix` |

### 1.3 ブランチの命名規則

ブランチ名は、チームの誰が見ても「何の作業をしているか」がすぐにわかるようにしましょう。

| パターン | 例 | 用途 |
|---------|-----|------|
| `feature/説明` | `feature/add-search` | 新機能 |
| `fix/説明` | `fix/login-error` | バグ修正 |
| `docs/説明` | `docs/update-readme` | ドキュメント |
| `refactor/説明` | `refactor/user-model` | リファクタリング |
| `test/説明` | `test/add-unit-tests` | テスト追加 |

> 💡 **Issue番号を含めるパターン**も一般的です：`feature/42-add-search`

### 1.4 ブランチ保護ルール

`main` ブランチを「壊れないように」守るための設定です。チーム開発では必ず設定しましょう。

**Settings** → **Branches** → **Add branch protection rule**

| ルール | わかりやすい説明 |
|--------|------|
| **Require a pull request before merging** | mainに直接pushできなくする。PRを必須にする |
| **Require approvals** | 誰か1人以上の「OK（承認）」がないとマージできない |
| **Require status checks to pass** | 自動テスト（CI）が成功しないとマージできない |
| **Require conversation resolution** | レビューコメントをすべて解決しないとマージできない |
| **Require signed commits** | 署名付きコミット（本人確認）を必須にする |
| **Include administrators** | 管理者にもこのルールを適用する |

### 1.5 ブランチ操作コマンド

以下のコマンドは Windows（PowerShell / Git Bash）でも Mac（ターミナル）でも共通です：

```bash
# ブランチの一覧表示
git branch              # ローカルブランチ
git branch -r           # リモートブランチ
git branch -a           # すべてのブランチ

# ブランチの作成と切り替え
git checkout -b feature/new-feature    # 作成して切り替え
git switch -c feature/new-feature      # 同上（新しいコマンド）

# ブランチの切り替え
git checkout main
git switch main         # 新しいコマンド

# ブランチの削除
git branch -d feature/old-feature      # マージ済みのブランチを削除
git branch -D feature/old-feature      # 強制削除

# リモートブランチの削除
git push origin --delete feature/old-feature

# リモートの最新情報を取得
git fetch origin
git fetch --prune       # 削除されたリモートブランチの参照を掃除
```

---

## Part 2：Pull Request DeepDive（20分）

### 2.1 Pull Requestの役割

Pull Request（PR）は、ただのマージリクエストではありません。チーム開発で**一番大事な場所**です。

| 役割 | 説明 |
|------|------|
| **コードレビュー** | 変更内容を他のメンバーにチェックしてもらう |
| **話し合いの場** | 「こう実装した理由」や「別の方法」について議論できる |
| **品質チェック** | 自動テストが通っているか確認するゲート |
| **知識の共有** | チーム全体でコードの理解を深められる |
| **変更の記録** | なぜその変更をしたかの履歴として残る |

### 2.2 良いPull Requestの書き方

#### PRテンプレートの作成

`.github/PULL_REQUEST_TEMPLATE.md` を作成：

```markdown
## 概要
<!-- この変更の概要を記載 -->

## 関連Issue
<!-- 関連するIssue番号を記載 -->
Closes #

## 変更内容
<!-- 主な変更内容をリストアップ -->
- 
- 
- 

## 変更の種類
- [ ] 🐛 バグ修正
- [ ] ✨ 新機能
- [ ] 📝 ドキュメント更新
- [ ] ♻️ リファクタリング
- [ ] 🧪 テスト追加・修正

## テスト
<!-- テスト方法を記載 -->
- [ ] 既存のテストが通ること
- [ ] 新しいテストを追加した

## スクリーンショット
<!-- UIの変更がある場合は添付 -->

## チェックリスト
- [ ] コードが規約に従っている
- [ ] 必要なテストを追加した
- [ ] ドキュメントを更新した
```

### 2.3 PRのサイズと粒度

PRは小さいほどレビューしやすく、バグも見つけやすくなります。

| PRサイズ | 変更行数の目安 | 判定 |
|---------|-----------|------|
| 🟢 Small | 〜200行 | ✅ レビューしやすい！ |
| 🟡 Medium | 200〜500行 | ⚠️ なんとか許容範囲 |
| 🔴 Large | 500行以上 | ❌ 分割を検討しよう |

> 💡 PRが小さいほど、レビューの質が上がり、マージまでのスピードも速くなります。

#### 大きなPRを避けるテクニック

1. **機能を小さく分割する**: 1つの機能を複数のPRに分ける
2. **リファクタリングは別PRにする**: 機能追加とリファクタリングを混ぜない
3. **段階的に作る**: ベース → 機能 → テスト の順でPRを出す

### 2.4 コードレビューの進め方

#### レビュアー側のポイント

| チェック項目 | 観点 |
|-------------|------|
| **機能性** | 要件を満たしているか |
| **可読性** | コードが読みやすいか |
| **設計** | アーキテクチャに沿っているか |
| **テスト** | 十分なテストがあるか |
| **セキュリティ** | セキュリティ上の問題はないか |
| **パフォーマンス** | パフォーマンスへの影響は |

#### レビューコメントの書き方

レビューでは、**なぜそう思うか** の理由と **どうすればよいか** の提案をセットで書くのがポイントです。

**良いコメントの例：**

```
✅ "この関数は長くなっているので、バリデーション部分を
    別の関数に切り出すと可読性が上がりそうです。"

✅ "ここでnullチェックが必要かもしれません。
    userがundefinedの場合にエラーになりそうです。"

✅ "Nit: 変数名をuserListよりusersにした方が
    一般的な命名規則に沿っています。"
```

**避けるべきコメント：**

```
❌ "これはダメです。" （理由がない）
❌ "なんでこう書いたの？" （攻撃的）
❌ "全部書き直して。" （具体性がない）
```

#### レビューコメントの接頭辞（プレフィックス）

チームで以下のような接頭辞を決めておくと、「これは必ず直してほしいのか、単なる提案なのか」がはっきりします。

| 接頭辞 | 意味 | 対応 |
|--------|------|------|
| `[must]` | 必ず修正が必要 | 修正必須 |
| `[should]` | 修正を推奨 | できれば修正 |
| `[nit]` | 細かい指摘 | 任意 |
| `[question]` | 質問 | 回答が必要 |
| `[suggestion]` | 提案 | 検討 |
| `[praise]` | 良い点 | 対応不要 👏 |

#### レビューのアクション

| アクション | 意味 |
|-----------|------|
| **Comment** | コメントのみ（承認/却下なし） |
| **Approve** | 承認（マージしてよい） |
| **Request changes** | 修正を要求（修正後に再レビュー） |

### 2.5 マージ戦略

GitHubでは3つのマージ方法を選べます。それぞれの違いを理解しておきましょう。

#### ① Merge commit（デフォルト）

```
main:    A ── B ──────── M
                \       /
feature:         C ── D
```

- すべてのコミット履歴が保持される
- マージコミットが作成される
- 履歴が正確に残る

#### ② Squash and merge

```
main:    A ── B ── CD'
                \
feature:         C ── D  (これらは1つにまとめられる)
```

- featureブランチのコミットが1つにまとめられる
- mainの履歴がクリーンになる
- 細かいコミット履歴は失われる

#### ③ Rebase and merge

```
main:    A ── B ── C' ── D'
                \
feature:         C ── D  (リベースされて直線的になる)
```

- コミットが直線的に並ぶ
- マージコミットが作成されない
- 履歴が直線的でクリーン

#### マージ戦略の使い分け

| 戦略 | 適したケース |
|------|-------------|
| **Merge commit** | 履歴をすべて残したい、大きなfeature |
| **Squash and merge** | 細かいコミットをまとめたい、小さな修正 |
| **Rebase and merge** | 直線的な履歴を保ちたい |

### 2.6 コンフリクトの解決

2つのブランチで**同じファイルの同じ場所**をそれぞれ変更すると、Gitはどちらの変更を採用すればよいかわかりません。これが**コンフリクト（衝突）** です。

#### コンフリクトが起きたときの見た目

以下のように、両方の変更が `<<<<<<< ` と `>>>>>>>` で囲まれて表示されます。  
自分で正しい内容に書き直す必要があります。

```
<<<<<<< feature/add-profile
名前: Alice
=======
名前: Bob
>>>>>>> main
```

#### 解決手順（Windows / Mac 共通）

```bash
# 1. mainブランチの変更を取り込む
git checkout feature/add-profile
git merge main

# 2. コンフリクトが発生したファイルを確認
git status

# 3. ファイルを編集してコンフリクトを解決
# <<<<<<< と >>>>>>> の間を正しい内容に修正

# 4. 解決したファイルをステージング
git add <解決したファイル>

# 5. コミット
git commit -m "コンフリクトを解決"

# 6. プッシュ
git push origin feature/add-profile
```

> 💡 GitHub上でもシンプルなコンフリクトは解決できます。PR画面で **Resolve conflicts** ボタンを使います。

---

## Part 3：ハンズオン - コードレビュー実践（15分）

### シナリオ

ペア（または個人）で以下の作業を行います。

#### Step 1：Issueを作成

```
Title: Webサイトのフッターを追加する
Labels: enhancement
```

#### Step 2：ブランチを作成してコードを変更

以下のコマンドは Windows / Mac 共通です：

```bash
git checkout -b feature/add-footer
```

以下の `index.html` を作成：

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Project</title>
</head>
<body>
    <header>
        <h1>My Project</h1>
    </header>
    <main>
        <p>GitHubワークショップのプロジェクトです。</p>
    </main>
    <footer>
        <p>&copy; 2026 My Project. All rights reserved.</p>
    </footer>
</body>
</html>
```

```bash
git add index.html
git commit -m "フッター付きのindex.htmlを追加 #Issue番号"
git push origin feature/add-footer
```

#### Step 3：Pull Requestを作成

- PRテンプレートに沿って記入
- `Closes #Issue番号` を含める

#### Step 4：コードレビュー

レビュアーとして以下を確認：
- [ ] HTMLの構造は正しいか
- [ ] 適切なmeta情報があるか
- [ ] コードのインデントは統一されているか
- [ ] コミットメッセージは適切か

レビューコメントを**最低2つ**つけてください。

#### Step 5：修正 & マージ

- レビューコメントに対応
- 承認を得てマージ

---

## まとめ（5分）

### 今日学んだこと

- ✅ ブランチ戦略（GitHub Flow と Git Flow）
- ✅ ブランチ保護ルールの設定
- ✅ PRの書き方とテンプレートの活用
- ✅ コードレビューの進め方とコメントのコツ
- ✅ マージ戦略の違い（Merge、Squash、Rebase）
- ✅ コンフリクト（衝突）の解決方法

### 次回予告：ワークショップ4「Project管理 DeepDive」

- GitHub Projectsの使い方
- ボードビューとテーブルビュー
- 自動化ルール
- スプリント管理とイテレーション

---

## 📚 参考リンク

- [GitHub Flow](https://docs.github.com/ja/get-started/using-github/github-flow)
- [Pull Requestについて](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [コードレビューについて](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews)
- [マージ方法について](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges)
- [ブランチ保護ルール](https://docs.github.com/ja/repositories/configuring-branches-and-merges-in-your-repository/managing-a-branch-protection-rule/about-protected-branches)
