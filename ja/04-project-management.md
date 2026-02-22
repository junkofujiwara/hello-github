# ワークショップ4：Project管理 DeepDive（60分）

> 📖 [English version](../en/04-project-management.md)

## 🎯 このワークショップのゴール

- GitHub Projectsを使ったプロジェクト管理の基本を理解する
- ボードビュー・テーブルビューを使いこなせるようになる
- IssueとProjectを組み合わせて、効率よくタスクを管理する方法を体験する
- 自動化ルールを設定して、手作業を減らす方法を学ぶ

---

## 📋 アジェンダ

| 時間 | 内容 |
|------|------|
| 0:00 - 0:05 | 前回の振り返り |
| 0:05 - 0:20 | GitHub Projectsの基本 |
| 0:20 - 0:35 | ビューとカスタマイズ |
| 0:35 - 0:50 | 自動化 & 実践ハンズオン |
| 0:50 - 1:00 | まとめ |

---

## 前回の振り返り（5分）

ワークショップ3では、Branch と Pull Request を深掘りしました。  
今回は、**GitHub Projects** を使ったプロジェクト管理を学びます。

Issue を作って、Branch を切って、PR を出す。この一連の開発フローを**全体を見渡して管理**するのが Projects の役割です。  
付箋（ふせん）をボードに貼って進捗を管理するイメージです。

---

## Part 1：GitHub Projectsの基本（15分）

### 1.1 GitHub Projectsとは

**GitHub Projects** は、IssueやPull Requestを**ボード（カンバン）形式**や**テーブル（一覧表）形式**で見やすく整理できるプロジェクト管理ツールです。  
チームの「今、誰が何をやっているか」が一目でわかるようになります。

```
┌─────────────────────────────────────────────────────────┐
│                   GitHub Projects                        │
│                                                          │
│  📋 Todo        🔄 In Progress     ✅ Done              │
│  ┌──────────┐  ┌──────────┐      ┌──────────┐          │
│  │ Issue #5 │  │ Issue #3 │      │ Issue #1 │          │
│  │ 検索機能  │  │ API実装  │      │ 初期設定  │          │
│  └──────────┘  └──────────┘      └──────────┘          │
│  ┌──────────┐  ┌──────────┐      ┌──────────┐          │
│  │ Issue #6 │  │ Issue #4 │      │ Issue #2 │          │
│  │ テスト   │  │ UI実装   │      │ DB設計   │          │
│  └──────────┘  └──────────┘      └──────────┘          │
└─────────────────────────────────────────────────────────┘
```

### 1.2 Projectの種類

| 種類 | 説明 | 用途 |
|------|------|------|
| **User Project** | 個人のプロジェクト | 個人のタスク管理、複数リポジトリのまとめ管理 |
| **Organization Project** | 組織のプロジェクト | チーム・部門のプロジェクト管理 |

> 💡 1つのProjectに**複数のリポジトリ**のIssueをまとめて追加できるのが大きな特徴です。リポジトリをまたいだタスク管理に便利！

### 1.3 Projectの作成

1. GitHubのプロフィールページまたはリポジトリの **Projects** タブ
2. **New project** をクリック
3. テンプレートを選択：

| テンプレート | 説明 |
|------------|------|
| **Board** | カンバンボード形式 |
| **Table** | スプレッドシート形式 |
| **Roadmap** | タイムライン形式 |
| **Start from scratch** | 白紙からスタート |

4. プロジェクト名を入力
5. **Create project** をクリック

### 1.4 Projectにアイテムを追加

| アイテム | 説明 |
|---------|------|
| **Issue** | リポジトリのIssue |
| **Pull Request** | リポジトリのPR |
| **Draft issue** | まだIssue化していないメモ |

#### 追加方法

**方法1：Project画面から追加**
1. `+ Add item` をクリック
2. リポジトリ名で検索して Issue/PR を選択

**方法2：Issue画面から追加**
1. Issueの右サイドバー → **Projects**
2. 追加したいProjectを選択

**方法3：ドラフトIssue**
1. `+ Add item` をクリック
2. テキストを入力して Enter
3. 後から正式なIssueに変換可能

### ✅ ハンズオン：Projectを作成

1. **Board テンプレート**で新しいProjectを作成
   - 名前：`GitHubワークショッププロジェクト`
2. ワークショップ2で作成したIssueをProjectに追加
3. ドラフトIssueを2つ以上追加

---

## Part 2：ビューとカスタマイズ（15分）

### 2.1 ビューの種類

| ビュー | 説明 | 適した用途 |
|--------|------|-----------|
| **Board** | カンバンボード | 日々のタスク管理、スプリント管理 |
| **Table** | テーブル（スプレッドシート風） | 一覧表示、フィルタリング、ソート |
| **Roadmap** | タイムライン | 長期計画、マイルストーン管理 |

### 2.2 ボードビュー

カンバン方式でアイテムを管理します。ドラッグ&ドロップでアイテムを移動できます。

```
┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
│  📋 Todo  │  │ 🔄 In    │  │ 👀 In    │  │  ✅ Done │
│           │  │ Progress │  │  Review  │  │          │
│  #5 検索  │  │  #3 API  │  │  #4 UI   │  │  #1 設定 │
│  #6 テスト│  │          │  │          │  │  #2 DB   │
└──────────┘  └──────────┘  └──────────┘  └──────────┘
```

#### カラムのカスタマイズ

**例1：開発チーム向け**
```
Backlog → Todo → In Progress → In Review → Done
```

**例2：カスタマーサポート向け**
```
New → Triaged → In Progress → Waiting → Resolved
```

### 2.3 テーブルビュー

```
┌────────┬──────────┬──────────┬──────────┬──────────┐
│ Title  │ Status   │ Priority │ Assignee │ Sprint   │
├────────┼──────────┼──────────┼──────────┼──────────┤
│ #1 設定│ Done     │ High     │ Alice    │ Sprint 1 │
│ #2 DB  │ Done     │ High     │ Bob      │ Sprint 1 │
│ #3 API │ Progress │ Medium   │ Alice    │ Sprint 2 │
│ #4 UI  │ Review   │ Medium   │ Charlie  │ Sprint 2 │
│ #5 検索│ Todo     │ Low      │ -        │ Sprint 3 │
└────────┴──────────┴──────────┴──────────┴──────────┘
```

### 2.4 カスタムフィールド

| フィールド型 | 説明 | 例 |
|------------|------|-----|
| **Text** | テキスト入力 | メモ、参照URL |
| **Number** | 数値 | ストーリーポイント、見積もり時間 |
| **Date** | 日付 | 期限、開始日 |
| **Single select** | 単一選択 | 優先度、カテゴリ |
| **Iteration** | イテレーション | スプリント |

#### おすすめのカスタムフィールド

```
Priority:    🔴 High / 🟡 Medium / 🟢 Low
Size:        🐘 Large / 🐕 Medium / 🐁 Small
Sprint:      Sprint 1 / Sprint 2 / Sprint 3 ...
Category:    Frontend / Backend / Infrastructure / Documentation
```

### 2.5 フィルタとグループ化

#### フィルタ

```
assignee:@me                              # 自分のタスク
label:bug                                 # バグのみ
status:Todo,In Progress                   # 特定ステータス
assignee:@me label:bug status:"In Progress"  # 組み合わせ
```

#### グループ化

- **Status** でグループ化 → カンバン風の表示
- **Assignee** でグループ化 → 担当者別の表示
- **Priority** でグループ化 → 優先度別の表示

### 2.6 複数ビューの活用

| ビュー名 | 種類 | 用途 |
|---------|------|------|
| Sprint Board | Board | 現在のスプリントのカンバン |
| All Items | Table | 全アイテムの一覧 |
| My Tasks | Table | 自分のタスク一覧 |
| Roadmap | Roadmap | 長期計画 |
| Bug Tracker | Table | バグのみ表示 |

### ✅ ハンズオン：ビューをカスタマイズ

1. **カスタムフィールドを追加**
   - Priority（Single select）: High / Medium / Low
   - Size（Single select）: Large / Medium / Small
2. **テーブルビューを追加**（名前：`All Items`、Priorityでソート）
3. **フィルタ付きビューを追加**（名前：`My Tasks`、フィルタ：`assignee:@me`）

---

## Part 3：自動化 & 実践ハンズオン（15分）

### 3.1 ビルトイン自動化

**Settings** → **Workflows** で以下を設定できます：

| ワークフロー | 説明 |
|-------------|------|
| **Item added to project** | アイテム追加時にStatusをTodoに設定 |
| **Item reopened** | Issue再オープン時にStatusをTodoに戻す |
| **Item closed** | IssueクローズしたらStatusをDoneに変更 |
| **Pull request merged** | PRマージ時にStatusをDoneに変更 |
| **Auto-add to project** | 特定の条件でIssueを自動追加 |

#### 設定方法

1. Project画面の右上 **⋯** → **Workflows**
2. 使いたいワークフローを選択
3. **Edit** をクリック
4. 条件と動作を設定
5. トグルをONにして有効化

### 3.2 GitHub Actions との連携

```yaml
# .github/workflows/project-automation.yml
name: Project Automation

on:
  issues:
    types: [opened, labeled]

jobs:
  add-to-project:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/add-to-project@v1
        with:
          project-url: https://github.com/users/あなたのユーザー名/projects/1
          github-token: ${{ secrets.PROJECT_TOKEN }}
```

### 3.3 スプリント管理

#### イテレーション（Sprint）の設定

1. テーブルビューで **+** → **Iteration** フィールドを追加
2. イテレーションの期間を設定（例：2週間）
3. 各アイテムにスプリントを割り当て

```
Sprint 1 (2/1 - 2/14)
├── Issue #1: ログイン機能     → Done ✅
├── Issue #2: DB設計          → Done ✅
└── Issue #3: API実装         → Done ✅

Sprint 2 (2/15 - 2/28)
├── Issue #4: UI実装          → In Progress 🔄
├── Issue #5: 検索機能        → Todo 📋
└── Issue #6: テスト          → Todo 📋
```

### 3.4 実践ハンズオン：プロジェクトを運用する

#### シナリオ：「チームWebサイト」プロジェクト

**Step 1：Issueを作成**

| Issue | タイトル | ラベル | 優先度 |
|-------|---------|--------|--------|
| #1 | トップページを作成する | enhancement | High |
| #2 | ナビゲーションメニューを追加する | enhancement | High |
| #3 | フッターのリンク切れを修正する | bug | Medium |
| #4 | チームメンバー紹介ページを追加する | enhancement | Medium |
| #5 | レスポンシブデザインに対応する | enhancement | Low |
| #6 | パフォーマンステストを実施する | task | Low |

**Step 2：Projectで管理**

1. 全Issueをプロジェクトに追加
2. PriorityとSizeフィールドを設定
3. Sprint 1に #1, #2, #3 を割り当て
4. Sprint 2に #4, #5, #6 を割り当て

**Step 3：ワークフローを設定**

1. `Item closed → Done` を有効化
2. `Item added → Todo` を有効化

**Step 4：作業をシミュレーション**

1. #1 のStatusを `In Progress` に変更
2. #1 のIssueに作業コメントを追加
3. #1 をCloseして、自動で `Done` に移動することを確認

---

## まとめ（10分）

### 今日学んだこと

- ✅ GitHub Projectsの作成と基本操作
- ✅ ボードビュー、テーブルビュー、ロードマップビュー
- ✅ カスタムフィールドの活用
- ✅ フィルタとグループ化
- ✅ ビルトイン自動化ワークフロー
- ✅ スプリント管理の基本

### 次回予告：ワークショップ5「GitHub Actions DeepDive」

- ワークフローの構文を詳しく学ぶ
- CI/CDパイプラインの構築
- 自動テスト、ビルド、デプロイの設定
- カスタムアクションの利用

---

## 📚 参考リンク

- [GitHub Projects ドキュメント](https://docs.github.com/ja/issues/planning-and-tracking-with-projects)
- [Projects のベストプラクティス](https://docs.github.com/ja/issues/planning-and-tracking-with-projects/learning-about-projects/best-practices-for-projects)
- [Projects の自動化](https://docs.github.com/ja/issues/planning-and-tracking-with-projects/automating-your-project)
- [Projects のビューのカスタマイズ](https://docs.github.com/ja/issues/planning-and-tracking-with-projects/customizing-views-in-your-project)
