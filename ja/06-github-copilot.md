# ワークショップ6：GitHub Copilot DeepDive（60分）

> 📖 [English version](../en/06-github-copilot.md)

## 🎯 このワークショップのゴール

- GitHub Copilotの概要と仕組みを理解する
- GitHub Copilot（Free版）を有効化してセットアップする
- Copilot Chat を使って対話的にコーディングする
- Copilot Agent（Coding Agent）の概念と使い方を理解する
- GitHub Skills を活用した自律的な学習方法を知る

---

## 📋 アジェンダ

| 時間 | 内容 |
|------|------|
| 0:00 - 0:05 | 前回の振り返り |
| 0:05 - 0:15 | GitHub Copilotの概要 |
| 0:15 - 0:25 | Copilotの有効化とセットアップ |
| 0:25 - 0:40 | Copilot Chat ハンズオン |
| 0:40 - 0:55 | Copilot Agent & Skills |
| 0:55 - 1:00 | まとめ |

---

## 前回の振り返り（5分）

ワークショップ5では、GitHub Actionsを使ったCI/CDと自動化を学びました。  
最終回となるこのワークショップでは、**GitHub Copilot** — AIを活用した開発支援ツールを学びます。

---

## Part 1：GitHub Copilotの概要（10分）

### 1.1 GitHub Copilotとは

**GitHub Copilot**は、AIを活用した開発支援ツールです。コードの自動補完、チャットによる対話的なコーディング支援、コードの説明・修正提案など、多岐にわたる機能を提供します。

```
┌─────────────────────────────────────────────────┐
│              GitHub Copilot                      │
│                                                  │
│  💬 Chat          ✍️ Code Completion              │
│  対話的に質問・依頼   コードの自動補完               │
│                                                  │
│  🤖 Agent         🛠️ Skills                      │
│  自律的にタスク実行   拡張機能・ツール連携            │
│                                                  │
│  📝 Code Review   🔍 Explain                     │
│  コードレビュー支援   コードの説明                   │
└─────────────────────────────────────────────────┘
```

### 1.2 GitHub Copilotの機能一覧

| 機能 | 説明 | 利用場所 |
|------|------|---------|
| **Code Completion** | コードの自動補完・提案 | エディタ内 |
| **Copilot Chat** | 自然言語で質問・依頼 | VS Code、GitHub.com |
| **Copilot Agent** | 自律的にタスクを実行 | VS Code、GitHub.com |
| **Code Review** | PRのコードレビュー支援 | GitHub.com |
| **Copilot in CLI** | コマンドライン支援 | ターミナル |
| **Skills / Extensions** | 外部ツール・MCP連携 | VS Code |

### 1.3 GitHub Copilot Free

GitHub Copilotには複数のプランがあります。**無料のGitHubアカウント**があれば、**Copilot Free**が利用可能です。

| プラン | 料金 | 主な機能 |
|--------|------|---------|
| **Copilot Free** | 無料 | コード補完（月間制限あり）、Chat、Agent（制限あり） |
| **Copilot Pro** | 有償 | 無制限の補完、高度なモデル選択 |
| **Copilot Business** | 有償 | 組織向け、ポリシー管理 |
| **Copilot Enterprise** | 有償 | カスタマイズ、Knowledge Base |

> 💰 最新の料金は [GitHub Copilot の料金ページ](https://github.com/features/copilot#pricing) を参照してください。

> 💡 このワークショップでは**Copilot Free**を使用します。無料のGitHubアカウントがあれば追加費用なしで利用を開始できます。

### 1.4 対応しているエディタ・環境

| 環境 | 対応状況 |
|------|---------|
| **Visual Studio Code** | ✅ フル対応（推奨） |
| **Visual Studio** | ✅ 対応 |
| **JetBrains IDE** | ✅ 対応 |
| **Neovim** | ✅ 対応 |
| **GitHub.com** | ✅ Chat、Agent対応 |
| **GitHub Mobile** | ✅ Chat対応 |
| **CLI (Terminal)** | ✅ Copilot in CLI |

---

## Part 2：Copilotの有効化とセットアップ（10分）

### 2.1 Copilot Freeの有効化

1. [github.com](https://github.com) にサインイン
2. 右上のプロフィールアイコン → **Settings**
3. 左サイドバー → **Copilot**
4. **GitHub Copilot Free** を選択して有効化
5. 利用規約に同意

> 💡 すでにCopilot Pro/Business/Enterpriseを利用している場合はこの手順は不要です。

### 2.2 VS Code拡張機能のインストール

#### 🪟 Windows / 🍎 Mac 共通

1. VS Codeを開く
2. 拡張機能パネルを開く（`Ctrl+Shift+X` / `Cmd+Shift+X`）
3. 以下の拡張機能を検索してインストール：

| 拡張機能 | 説明 |
|---------|------|
| **GitHub Copilot** | コード補完・Chat・Agent |

4. インストール後、VS Codeの右下に Copilot アイコンが表示されることを確認

### 2.3 GitHubアカウントの認証

1. VS Code下部の **Copilotアイコン** をクリック
2. **Sign in to GitHub** を選択
3. ブラウザが開くので、GitHubにサインイン
4. 認証を許可
5. VS Codeに戻り、Copilotが有効になっていることを確認

### 2.4 動作確認

VS Codeで新しいファイルを作成して、コード補完が動作することを確認します。

1. 新しいファイルを作成（例：`test.js`）
2. 以下のように入力を開始：

```javascript
// 2つの数値を足し算する関数
function add(
```

3. Copilotがグレーの文字で補完候補を表示することを確認
4. `Tab` キーで補完を受け入れ

### ✅ ハンズオン：セットアップ確認

- [ ] Copilot Freeが有効化されている
- [ ] VS CodeにGitHub Copilot拡張機能がインストールされている
- [ ] GitHubアカウントで認証済み
- [ ] コード補完が動作する

---

## Part 3：Copilot Chat ハンズオン（15分）

### 3.1 Copilot Chatとは

**Copilot Chat**は、自然言語でAIと対話しながらコーディングできる機能です。質問、コード生成、デバッグ、リファクタリングなど、幅広い用途で活用できます。

### 3.2 Chatの開き方

| 方法 | 操作 |
|------|------|
| **チャットパネル** | `Ctrl+Alt+I`（Windows）/ `Cmd+Alt+I`（Mac） |
| **インラインチャット** | `Ctrl+I`（Windows）/ `Cmd+I`（Mac） |
| **クイックチャット** | `Ctrl+Shift+I`（Windows）/ `Cmd+Shift+I`（Mac） |

### 3.3 Chatの基本的な使い方

#### 質問する

```
Gitのブランチ戦略について教えてください
```

#### コードを生成する

```
HTMLで、ナビゲーションバーを含むレスポンシブなヘッダーを作成してください
```

#### コードを説明してもらう

ファイルを開いた状態で：
```
このコードが何をしているか説明してください
```

#### エラーを修正する

```
このエラーの原因と修正方法を教えてください: TypeError: Cannot read property 'map' of undefined
```

### 3.4 チャット参加者（Participants）

チャットで `@` を使って特定の参加者を指定できます：

| 参加者 | 説明 | 例 |
|--------|------|-----|
| `@workspace` | ワークスペース全体を文脈として使う | `@workspace このプロジェクトの構成を説明して` |
| `@vscode` | VS Codeの設定・操作について | `@vscode ダークテーマに変更するには？` |
| `@terminal` | ターミナルの出力を文脈として使う | `@terminal このエラーの原因は？` |

### 3.5 スラッシュコマンド

| コマンド | 説明 | 例 |
|---------|------|-----|
| `/explain` | コードの説明 | `/explain この関数の処理内容` |
| `/fix` | コードの修正 | `/fix このバグを修正して` |
| `/tests` | テストコードの生成 | `/tests この関数のユニットテストを作成` |
| `/new` | 新しいプロジェクト/ファイルの作成 | `/new Expressサーバーを作成` |
| `/doc` | ドキュメント・コメントの生成 | `/doc この関数にドキュメントを追加` |

### 3.6 コンテキストの活用

#### ファイル参照

チャットで `#` を使ってファイルを参照できます：

```
#index.html のスタイルを改善する方法を提案してください
```

#### 選択範囲の活用

1. エディタでコードを選択
2. Copilot Chat を開く
3. 選択したコードが自動的にコンテキストとして使われる

### ✅ ハンズオン：Copilot Chatを体験

**課題1：HTMLページの生成**

Copilot Chatに以下を依頼してください：

```
hello-githubリポジトリのindex.htmlに、GitHub Copilotの紹介セクションを追加するHTMLコードを生成してください。ダークテーマで、カード形式のレイアウトにしてください。
```

**課題2：コードの説明**

ワークショップ5で作成したGitHub Actionsのワークフローファイルを開いて：

```
/explain このワークフローの各ステップを説明してください
```

**課題3：エラーの修正**

わざとエラーのあるコードを入力して、Copilotに修正してもらいましょう：

```javascript
// このコードにはバグがあります
function greet(name) {
    console.log("Hello, " + nane);
}
greet();
```

```
/fix このコードのバグを修正してください
```

---

## Part 4：Copilot Agent & Skills（15分）

### 4.1 Copilot Agentとは

**Copilot Agent（Coding Agent）** は、Copilotが自律的にタスクを計画・実行する機能です。単純な補完やチャットとは異なり、複数のファイルにまたがる変更や、タスクの分解・実行を自動で行います。

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  ユーザーの    │────▶│  Copilot     │────▶│   結果       │
│  指示         │     │  Agent       │     │              │
│              │     │              │     │ - ファイル編集 │
│ "認証機能を   │     │ - 計画立案   │     │ - テスト追加   │
│  追加して"    │     │ - コード生成  │     │ - PR作成      │
│              │     │ - テスト実行  │     │              │
└──────────────┘     └──────────────┘     └──────────────┘
```

### 4.2 Agentの使い方

#### VS Codeでの利用

1. Copilot Chatを開く
2. チャットモードを **Agent** に切り替え（ドロップダウンで選択）
3. タスクを自然言語で依頼

```
このプロジェクトにCSSファイルを追加して、index.htmlにリンクしてください。
レスポンシブデザインにして、ダークモードもサポートしてください。
```

#### GitHub.comでの利用（Copilot Coding Agent）

1. GitHub.comのリポジトリページで Issue を作成
2. Issue に Copilot を割り当て（Assignee に `copilot` を指定）
3. Copilotが自動でブランチ作成、コード変更、PR作成を実行

```
Issue タイトル: Add a contact form to the website
Issue 本文: 
- Create a contact form with name, email, and message fields
- Add form validation
- Style with CSS to match existing design
```

### 4.3 Agent vs Chat の違い

| 項目 | Chat | Agent |
|------|------|-------|
| **範囲** | 1つの質問・回答 | 複数ステップのタスク |
| **ファイル操作** | 提案のみ | 直接編集可能 |
| **自律性** | ユーザーが都度指示 | 計画を立てて自律実行 |
| **ターミナル操作** | なし | コマンド実行可能 |
| **複数ファイル** | コンテキスト参照 | 複数ファイル同時編集 |

### 4.4 Skills（スキル）とは

**Skills（Extensions / MCP）** は、Copilotの能力を拡張する仕組みです。外部のツール、API、データソースと連携して、より高度なタスクを実行できます。

#### MCP（Model Context Protocol）

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Copilot     │────▶│  MCP Server  │────▶│  External    │
│  Agent       │     │              │     │  Service     │
│              │     │  - DB接続     │     │              │
│              │◀────│  - API呼出    │◀────│  - DB        │
│              │     │  - ファイル操作 │     │  - API       │
└──────────────┘     └──────────────┘     └──────────────┘
```

MCPは、AIモデルと外部ツールをつなぐオープンなプロトコルです。

| 用途 | 例 |
|------|-----|
| **データベース接続** | SQLの生成・実行 |
| **API連携** | REST APIの呼び出し |
| **ファイルシステム** | ファイルの読み書き |
| **ブラウザ操作** | Webページの取得 |
| **カスタムツール** | 独自のツール連携 |

#### VS Codeでの設定例

`.vscode/mcp.json` を作成：

```json
{
  "servers": {
    "my-mcp-server": {
      "type": "stdio",
      "command": "node",
      "args": ["path/to/mcp-server.js"]
    }
  }
}
```

### 4.5 GitHub Skills でCopilotを学ぶ

[GitHub Skills](https://skills.github.com/) では、Copilotに関するインタラクティブなコースが用意されています。

| コース | 内容 |
|--------|------|
| **Code with GitHub Copilot** | Copilotの基本的な使い方 |
| **Copilot Autofix** | セキュリティ脆弱性の自動修正 |
| **Build and deploy** | Copilotを使ったアプリ構築 |

> 💡 GitHub Skillsの各コースはリポジトリ形式で提供され、実際にコードを書きながら学習できます。

### ✅ ハンズオン：Agent を体験

**課題1：Agentモードでファイル生成**

VS CodeでCopilot Chatを **Agent** モードに切り替えて、以下を依頼：

```
hello-githubリポジトリに about.html ファイルを作成してください。
内容は「GitHubワークショップについて」のページです。
index.htmlと同じスタイルを使い、ワークショップの概要を紹介する内容にしてください。
```

Agentが計画を提示したら内容を確認し、承認して実行します。

**課題2：GitHub.com で Copilot に Issue を割り当て（デモ）**

> ⚠️ Copilot Free版では一部制限があります。インストラクターのデモを確認してください。

1. GitHub.comでリポジトリを開く
2. 新しいIssueを作成
3. Copilotの動作フローを確認

**課題3：プロンプトの工夫**

より良い結果を得るためのプロンプトの書き方を練習します：

| ❌ 曖昧なプロンプト | ✅ 良いプロンプト |
|------------------|----------------|
| ページを作って | index.htmlにナビゲーションバーを追加してください。リンク先はHome、About、Contactの3つです。レスポンシブ対応にしてください。 |
| バグを直して | 23行目のTypeErrorを修正してください。nameが未定義の場合にデフォルト値を使うようにしてください。 |
| テストを書いて | add関数のユニットテストを作成してください。正常系（正の数、負の数、ゼロ）と異常系（文字列入力）のケースを含めてください。 |

---

## まとめ（5分）

### 今日学んだこと

- ✅ GitHub Copilotの概要と仕組み
- ✅ Copilot Freeの有効化とVS Codeセットアップ
- ✅ Copilot Chatによる対話的コーディング
- ✅ スラッシュコマンドとコンテキスト活用
- ✅ Copilot Agentの概念と使い方
- ✅ Skills / MCP による機能拡張
- ✅ GitHub Skillsでの自律学習

### ワークショップシリーズ全体の振り返り

| WS | テーマ | 主な機能 |
|----|--------|---------|
| 準備編 | 事前準備 | アカウント, Git インストール |
| WS 1 | 開発サイクル全体像 | Repository, Issue, Branch, PR |
| WS 2 | Repository & Issue | リポジトリ設定, Issue管理 |
| WS 3 | Branch & Pull Request | ブランチ戦略, レビュー |
| WS 4 | Project管理 | ボード, 自動化, スプリント |
| WS 5 | GitHub Actions | CI/CD, デプロイ, 自動化 |
| WS 6 | GitHub Copilot | Chat, Agent, Skills |
| WS 7 | リリース＆デプロイ | タグ, Releases, Pages, Packages |
| WS 8 | セキュリティ | Dependabot, スキャン, プロテクション |
| WS 9 | 管理 | Repo, Org, Enterprise管理 |

### 次のステップ

- 🧪 **GitHub Skills** で各コースにチャレンジ
- 🤖 **Copilot** を日常の開発ワークフローに組み込む
- 🔌 **MCP** サーバーを構築して独自ツール連携を試す
- 🏢 **チーム導入** を検討（Copilot Business / Enterprise）
- 📖 **GitHub Universe** や **GitHub Blog** で最新情報をキャッチアップ

---

## 📚 参考リンク

- [GitHub Copilot ドキュメント](https://docs.github.com/ja/copilot)
- [GitHub Copilot Free について](https://docs.github.com/ja/copilot/about-github-copilot/github-copilot-free)
- [VS Code での Copilot の使い方](https://code.visualstudio.com/docs/copilot/overview)
- [GitHub Skills](https://skills.github.com/)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
- [GitHub Copilot Trust Center](https://resources.github.com/copilot-trust-center/)
