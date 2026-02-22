# ワークショップ9：管理 DeepDive（60分）

> 📖 [English version](../en/09-administration.md)

## 🎯 このワークショップのゴール

- GitHubの3つの管理レベル（Repository、Organization、Enterprise）を理解する
- リポジトリ設定とアクセス制御をマスターする
- Organization管理：ロール、チーム、ポリシーを学ぶ
- Enterprise契約が必要な機能を把握する（🔒 Enterprise）
- 組織のガバナンスとコンプライアンス戦略を立てる

---

## 📋 アジェンダ

| 時間 | 内容 |
|------|------|
| 0:00 - 0:05 | ワークショップ8の振り返り |
| 0:05 - 0:20 | リポジトリ管理 |
| 0:20 - 0:40 | Organization管理 |
| 0:40 - 0:55 | Enterprise管理 |
| 0:55 - 1:00 | まとめ |

---

## ワークショップ8の振り返り（5分）

ワークショップ8では、Dependabot、シークレットスキャン、コードスキャンなどGitHubのセキュリティ機能を学びました。  
今回は **管理** に焦点を当てます。管理者として、リポジトリ、Organization、Enterpriseをどう管理するかを学びましょう。

---

## Part 1：リポジトリ管理（15分）

### 1.1 GitHubの3つの管理レベル

GitHubの管理は3つの階層で構成されています。各レベルに独自の設定と制御があります。

```
┌─────────────────────────────────────────────────────┐
│  Enterprise（トップレベルのガバナンス）                │
│  🏢 Enterprise契約が必要                              │
│                                                      │
│  ┌───────────────────────────────────────────────┐  │
│  │  Organization（チームレベルの管理）              │  │
│  │  👥 すべてのプランで無償                         │  │
│  │                                                │  │
│  │  ┌─────────────────────────────────────────┐  │  │
│  │  │  Repository（プロジェクトレベルの設定）    │  │  │
│  │  │  📁 すべてのプランで無償                   │  │  │
│  │  └─────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

| レベル | 管理者 | 利用可能なプラン |
|--------|--------|-----------------|
| Repository | リポジトリ管理者 / Orgオーナー | すべてのプラン（Free、Pro、Team、Enterprise） |
| Organization | Orgオーナー | すべてのプラン（Free、Pro、Team、Enterprise） |
| Enterprise | Enterpriseオーナー | 🔒 Enterprise契約が必要 |

### 1.2 リポジトリのロール

リポジトリへのアクセスはロールで制御されます。各ロールは異なる権限レベルを提供します。

| ロール | 権限 | 用途 |
|--------|------|------|
| **Read** | コード、Issue、PRの閲覧 | 関係者、レビュアー |
| **Triage** | Issue、PRの管理（コード書き込み不可） | プロジェクトマネージャー |
| **Write** | コードのプッシュ、Issue/PR管理 | 開発者 |
| **Maintain** | 設定管理（破壊的操作は不可） | テックリード |
| **Admin** | 危険ゾーンを含む完全な制御 | リポジトリオーナー |

> 💡 個人リポジトリでは **コラボレーター** を直接招待します。Organizationリポジトリでは **チーム** を通じてアクセスを管理します。

### 1.3 リポジトリ設定

管理者が知っておくべき主要なリポジトリ設定：

#### 一般設定

| 設定 | 説明 |
|------|------|
| **Visibility（公開範囲）** | Public、Private、またはInternal（🔒 Enterprise） |
| **Default branch** | PRのベースブランチ（通常は `main`） |
| **Features** | Issues、Projects、Wiki、Discussionsの有効/無効 |
| **Merge設定** | マージコミット、スカッシュ、リベースの許可 |
| **Auto-delete head branches** | マージ後にブランチを自動削除 |

> 💡 **Internalリポジトリ** はEnterprise内のすべてのメンバーに公開されます。この公開範囲オプションはEnterprise契約（🔒 Enterprise）でのみ利用可能です。

#### ブランチ保護ルール

重要なブランチを保護し、マージ前にチェックを要求します。

| ルール | 説明 |
|--------|------|
| PRレビューを要求 | マージ前にPRがレビューされている必要がある |
| ステータスチェックを要求 | マージ前にCIがパスしている必要がある |
| 署名付きコミットを要求 | コミットが暗号署名されている必要がある |
| 線形履歴を要求 | マージコミット不可（リベース/スカッシュのみ） |
| 管理者にも適用 | ルールが管理者にも適用される |
| プッシュ可能な人を制限 | ブランチにプッシュできる人を限定 |

#### リポジトリルールセット

ルールセットは、ブランチ保護ルールのより現代的で柔軟な代替手段です。ブランチ、タグ、プッシュを対象にできます。

```
ルールセット: "production-protection"
├── 対象: "main", "release/*" に一致するブランチ
├── ルール:
│   ├── PRを要求（2件の承認）
│   ├── ステータスチェックを要求（CI/CD）
│   ├── フォースプッシュをブロック
│   └── 署名付きコミットを要求
└── バイパス: Organization管理者のみ
```

> 💡 ルールセットはリポジトリレベルとOrganizationレベルの両方で作成できます。Organizationレベルのルールセット（🔒 Enterprise）はすべてのリポジトリに適用できます。

#### 危険ゾーン（Danger Zone）

| アクション | 説明 | 元に戻せる？ |
|-----------|------|:----------:|
| Visibility変更 | Public ↔ Private | ✅ |
| Ownership移転 | 別のオーナー/Orgに移動 | ✅ |
| アーカイブ | 読み取り専用にする | ✅ |
| 削除 | リポジトリを完全に削除 | ❌ |

### 1.4 CODEOWNERS

`CODEOWNERS` ファイルは、特定のファイルが変更された時にレビュアーを自動割り当てします。

```
# .github/CODEOWNERS

# すべてのファイルのデフォルトオーナー
* @org/tech-leads

# フロントエンドチームがUI関連ファイルを担当
/src/components/ @org/frontend-team
*.css @org/frontend-team
*.tsx @org/frontend-team

# バックエンドチームがAPI関連ファイルを担当
/src/api/ @org/backend-team

# DevOpsチームがインフラ関連を担当
/.github/ @org/devops-team
Dockerfile @org/devops-team
```

### ✅ ハンズオン：リポジトリ設定を確認

1. リポジトリの **Settings** タブに移動
2. General、Branches、Collaborators & Teamsを確認
3. **Danger Zone** セクションを確認

---

## Part 2：Organization管理（20分）

### 2.1 Organizationとは？

**Organization** は、チームが複数のプロジェクトにわたって協力できる共有アカウントです。Organizationは以下を提供します：

- リポジトリの集中管理
- チームベースのアクセス制御
- 共有設定とポリシー
- 請求管理

```
Organization: "my-company"
├── チーム
│   ├── @my-company/engineering（全リポジトリにWrite権限）
│   ├── @my-company/frontend（フロントエンドリポジトリにWrite権限）
│   ├── @my-company/backend（バックエンドリポジトリにWrite権限）
│   └── @my-company/security（Security managerロール）
├── リポジトリ
│   ├── web-app（Private）
│   ├── api-server（Private）
│   ├── docs（Public）
│   └── infrastructure（Private）
└── 設定
    ├── メンバー権限
    ├── セキュリティポリシー
    └── 請求
```

### 2.2 Organizationのロール

| ロール | 説明 | 利用可能なプラン |
|--------|------|-----------------|
| **Owner** | 完全な管理者アクセス | すべてのプラン |
| **Member** | デフォルトのロール、リポジトリ作成可能 | すべてのプラン |
| **Billing manager** | 請求管理のみ | すべてのプラン |
| **Moderator** | ユーザーのブロック、コメント管理 | すべてのプラン |
| **Security manager** | 全リポジトリのセキュリティアラートを表示/管理 | GitHub Team / Enterprise |
| **Outside collaborator** | 特定リポジトリのみアクセス、メンバーではない | すべてのプラン |
| **カスタムロール** | カスタム権限セットを定義 | 🔒 Enterprise |

> ⚠️ **カスタムOrganizationロール** はEnterprise契約（🔒 Enterprise）でのみ利用可能です。

### 2.3 チーム

チームはOrganizationメンバーのグループで、アクセス管理を効率的に行う手段です。

#### チーム機能

| 機能 | 説明 |
|------|------|
| **ネストチーム** | 親チーム内にサブチームを作成 |
| **チームメンション** | IssueやPRで `@org/team-name` で通知 |
| **チームディスカッション** | チーム内コミュニケーション |
| **コードレビュー割り当て** | レビュー担当をチームメンバーから自動割り当て |
| **IdP同期** | IDプロバイダーのグループとチームを同期（🔒 Enterprise） |

#### チームの作成手順

1. Organization → **Teams** タブに移動
2. **New team** をクリック
3. チーム名、説明、公開範囲を設定
4. メンバーを追加
5. 適切なロールでリポジトリへのアクセスを付与

### 2.4 Organization設定

#### メンバー権限

| 設定 | 説明 |
|------|------|
| **Base permissions** | 全メンバーの全リポジトリに対するデフォルトロール（None、Read、Write、Admin） |
| **Repository creation** | メンバーにリポジトリ作成を許可（Public、Private、または両方） |
| **Repository forking** | プライベートリポジトリのフォークを許可/禁止 |
| **Pages creation** | GitHub Pagesの公開を制御 |
| **Outside collaborators** | 外部コラボレーターの追加にオーナー承認を要求 |

> 💡 ベストプラクティス：ベースパーミッションを **Read** または **None** に設定し、チームを通じてアクセスを管理しましょう。

#### ActionsとPackages設定

| 設定 | 説明 |
|------|------|
| **Actions permissions** | GitHub Actionsの全許可、選択許可、無効化 |
| **Required workflows** | 全リポジトリに特定のワークフローを強制（🔒 Enterprise） |
| **Self-hosted runners** | リポジトリ間で共有ランナーを管理 |
| **Packages access** | パッケージの公開範囲とアクセスを制御 |

#### セキュリティ設定

| 設定 | 説明 |
|------|------|
| **2FA要求** | 全メンバーに二要素認証を要求 |
| **SAML SSO** | IDプロバイダーによるシングルサインオン（🔒 Enterprise） |
| **SCIMプロビジョニング** | IdPからの自動ユーザープロビジョニング（🔒 Enterprise） |
| **IPアローリスト** | IPアドレスによるアクセス制限（🔒 Enterprise） |
| **セキュリティ構成** | リポジトリ全体にセキュリティ設定を一括適用 |

### 2.5 監査ログ

監査ログはOrganization内で行われたアクションを記録します。セキュリティ調査やコンプライアンスに役立ちます。

**アクセス方法：** Organization → **Settings** → **Audit log**

| 機能 | Free/Team | Enterprise |
|------|:---------:|:----------:|
| 監査ログの閲覧（Web） | ✅ | ✅ |
| 検索とフィルター | ✅ | ✅ |
| エクスポート（JSON/CSV） | ✅ | ✅ |
| 監査ログストリーミング | ❌ | 🔒 Enterprise |
| APIアクセス | ✅ | ✅ |

> 🔒 **監査ログストリーミング**（Splunk、Azure、S3などの外部SIEMツールへの送信）はEnterprise契約が必要です。

### 2.6 Organizationレベルのルールセット

Organizationレベルで作成されたルールセットは複数のリポジトリに適用されます。

| 機能 | Free/Team | Enterprise |
|------|:---------:|:----------:|
| リポジトリレベルのルールセット | ✅ | ✅ |
| Organizationレベルのルールセット | ❌ | 🔒 Enterprise |
| ルールインサイト（評価モード） | ❌ | 🔒 Enterprise |

### ✅ ハンズオン：Organization設定を探索

1. Organizationに移動（または github.com/organizations/new で作成）
2. **Settings** をクリック
3. **Member privileges**、**Teams**、**Security** を確認
4. **Audit log** を確認（利用可能な場合）

**チームの作成：**

1. Organization → **Teams** → **New team**
2. 名前：`developers`
3. 自分をメンバーとして追加
4. チームにリポジトリへのアクセスを付与

---

## Part 3：Enterprise管理（15分）

### 3.1 GitHub Enterpriseとは？

> 🔒 **Enterprise契約が必要** — このセクションの内容はすべて、GitHub Enterprise CloudまたはGitHub Enterprise Serverの購入が必要です。

GitHub Enterpriseは、高度なセキュリティ、コンプライアンス、管理機能が必要な組織にトップレベルのガバナンスを提供します。

```
┌─────────────────────────────────────────────────────────┐
│  Enterpriseアカウント（🔒 Enterprise契約）                │
│                                                          │
│  ├── ポリシー（全Orgに強制適用）                          │
│  ├── 請求（一括管理）                                     │
│  ├── ID管理（SAML SSO、SCIM、EMU）                       │
│  ├── 監査ログストリーミング                                │
│  ├── IPアローリスト                                       │
│  └── コストセンター                                       │
│                                                          │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐        │
│  │ Org: CoA    │  │ Org: CoB    │  │ Org: CoC   │        │
│  │ 50リポジトリ │  │ 20リポジトリ│  │ 30リポジトリ│        │
│  └────────────┘  └────────────┘  └────────────┘        │
└─────────────────────────────────────────────────────────┘
```

### 3.2 GitHubプラン比較

| 機能 | Free | Team | Enterprise 🔒 |
|------|:----:|:----:|:----------:|
| パブリックリポジトリ | ∞ | ∞ | ∞ |
| プライベートリポジトリ | ∞ | ∞ | ∞ |
| コラボレーター | ∞ | ∞ | ∞ |
| Actions分数/月 | 2,000 | 3,000 | 50,000 |
| Packagesストレージ | 500 MB | 2 GB | 50 GB |
| PRレビュー必須 | ❌ | ✅ | ✅ |
| Code owners | ❌ | ✅ | ✅ |
| ブランチ保護 | ❌ | ✅ | ✅ |
| Internalリポジトリ | ❌ | ❌ | 🔒 Enterprise |
| SAML SSO | ❌ | ❌ | 🔒 Enterprise |
| SCIMプロビジョニング | ❌ | ❌ | 🔒 Enterprise |
| Enterprise Managed Users | ❌ | ❌ | 🔒 Enterprise |
| Orgレベルのルールセット | ❌ | ❌ | 🔒 Enterprise |
| 監査ログストリーミング | ❌ | ❌ | 🔒 Enterprise |
| IPアローリスト | ❌ | ❌ | 🔒 Enterprise |
| GitHub Connect | ❌ | ❌ | 🔒 Enterprise |
| 99.9% SLA | ❌ | ❌ | 🔒 Enterprise |
| データレジデンシー | ❌ | ❌ | 🔒 Enterprise |

> 💰 最新の料金は [GitHub 料金ページ](https://github.com/pricing) をご確認ください。

### 3.3 Enterpriseポリシー（🔒 Enterprise）

Enterprise管理者はEnterprise内の **すべてのOrganization** に適用されるポリシーを設定できます。

| ポリシー領域 | 例 |
|------------|------|
| **Repository** | 作成、削除、公開範囲、移転、フォークの制御 |
| **ブランチ/タグ** | すべてのOrganizationにルールセットを強制 |
| **Actions** | 使用可能なActionsの制限、ランナーグループの管理 |
| **Copilot** | 機能の有効/無効、モデルアクセスの制御 |
| **コードセキュリティ** | Organization全体にセキュリティ設定を強制 |
| **IPアローリスト** | IPアドレスによるEnterpriseリソースへのアクセス制限 |
| **認証** | SAML SSO、SCIM、セッションポリシー |

```
Enterpriseポリシー: "2FA必須"
    ↓
すべてのOrganizationに強制適用：
    ├── Org: Development ✅ 2FA必須
    ├── Org: QA          ✅ 2FA必須
    └── Org: Operations  ✅ 2FA必須
```

### 3.4 IDとアクセス管理（🔒 Enterprise）

#### SAML シングルサインオン（SSO）

SAML SSOは、GitHubを企業のIDプロバイダー（Entra ID、Oktaなど）と統合します。

```
開発者 → GitHubログイン → IdPにリダイレクト → 認証 → アクセス許可
```

| 機能 | 説明 |
|------|------|
| **SAML SSO** | 企業IdPによるシングルサインオン |
| **SCIM** | ユーザーの自動プロビジョニング/デプロビジョニング |
| **チーム同期** | GitHubチームとIdPグループを同期 |

#### Enterprise Managed Users（EMU）

EMUは最高レベルのID制御を提供します。ユーザーアカウントはEnterprise **完全管理** — IdPによってプロビジョニング、制御、削除されます。

| 標準Enterprise | EMU付きEnterprise |
|:-------------:|:----------------:|
| ユーザーは個人のGitHubアカウントを持つ | ユーザーはEnterprise管理のアカウントを持つ |
| パブリックリポジトリに貢献可能 | Enterprise内でのみ貢献可能 |
| ユーザーが自分のプロフィールを管理 | プロフィールはIdPが管理 |
| SSOがリンクIDを追加 | IdPが唯一のIDソース |

### 3.5 Enterpriseロール（🔒 Enterprise）

| ロール | 説明 |
|--------|------|
| **Enterprise owner** | すべての設定への完全な管理者アクセス |
| **Enterprise member** | Enterprise内のすべてのユーザーのデフォルトロール |
| **Billing manager** | 請求とコストセンターの管理 |
| **Guest collaborator** | 特定リポジトリへの限定アクセス |

### 3.6 デプロイオプション（🔒 Enterprise）

| オプション | 説明 |
|-----------|------|
| **GitHub Enterprise Cloud** | GitHubがgithub.com上でホスト |
| **GitHub Enterprise Cloud（データレジデンシー付き）** | 専用サブドメインでホスト（例：your-company.ghe.com） |
| **GitHub Enterprise Server** | 自社インフラでセルフホスト |
| **GitHub Connect** | Enterprise ServerとEnterprise Cloudを接続 |

### 3.7 請求とコストセンター（🔒 Enterprise）

Enterpriseアカウントはすべてのorganizationの請求を一元管理します。

| 機能 | 説明 |
|------|------|
| **一括請求** | すべてのOrganizationを1つの請求で管理 |
| **コストセンター** | ビジネスユニットごとに支出を配分 |
| **Azure請求** | Azure サブスクリプションに使用量を請求 |
| **Visual Studioサブスクリプション** | VS EnterpriseにGitHub Enterpriseを含む |
| **支出制限** | Actions、Packages、Codespacesの制限を設定 |

### ✅ ハンズオン：管理スコープを理解する

現在のロールに基づいて、該当する演習を実施してください：

**リポジトリ管理者向け：**
1. リポジトリ → **Settings** に移動
2. **Branches** を確認 → `main` にブランチ保護ルールを追加
3. `CODEOWNERS` ファイルを作成

**Organizationオーナー向け：**
1. Organization → **Settings** に移動
2. **Member privileges** を確認し、ベースパーミッションを設定
3. チームを作成し、リポジトリアクセスを割り当て
4. **Audit log** を確認

**Enterprise導入検討者向け：**
1. [GitHub Enterprise](https://github.com/enterprise) でサービス内容を確認
2. [Enterprise Cloudトライアル](https://docs.github.com/ja/enterprise-cloud@latest/admin/overview/setting-up-a-trial-of-github-enterprise-cloud) オプションを確認
3. 自組織に必要なEnterprise機能をリストアップ

---

## まとめ（5分）

### 今日学んだこと

- ✅ GitHubの3つの管理レベル（Repository → Organization → Enterprise）
- ✅ リポジトリのロール、設定、ブランチ保護、ルールセット
- ✅ Organizationのロール、チーム、メンバー管理
- ✅ Enterprise専用機能（SAML SSO、SCIM、EMU、ポリシー、監査ログストリーミング）
- ✅ GitHubプラン比較とデプロイオプション

### 管理チェックリスト

| レベル | タスク | 状態 |
|--------|--------|------|
| **Repository** | ブランチ保護ルールを設定 | ☐ |
| **Repository** | CODEOWNERSファイルを作成 | ☐ |
| **Repository** | Danger Zone設定を確認 | ☐ |
| **Organization** | ベースパーミッションを適切に設定 | ☐ |
| **Organization** | アクセス管理用のチームを作成 | ☐ |
| **Organization** | 全メンバーに2FAを要求 | ☐ |
| **Organization** | 監査ログを定期的に確認 | ☐ |
| **Enterprise** 🔒 | SAML SSOを構成 | ☐ |
| **Enterprise** 🔒 | SCIMプロビジョニングをセットアップ | ☐ |
| **Enterprise** 🔒 | Enterprise全体のポリシーを定義 | ☐ |
| **Enterprise** 🔒 | 監査ログストリーミングを構成 | ☐ |

### プラン別機能クイックリファレンス

| 機能 | Free | Team | Enterprise 🔒 |
|------|:----:|:----:|:----------:|
| リポジトリ設定 | ✅ | ✅ | ✅ |
| ブランチ保護 | 基本 | ✅ | ✅ |
| Organization管理 | ✅ | ✅ | ✅ |
| チーム | ✅ | ✅ | ✅ |
| PRレビュー必須 | ❌ | ✅ | ✅ |
| Code owners | ❌ | ✅ | ✅ |
| Security managerロール | ❌ | ✅ | ✅ |
| カスタムOrgロール | ❌ | ❌ | 🔒 Enterprise |
| Internalリポジトリ | ❌ | ❌ | 🔒 Enterprise |
| SAML SSO | ❌ | ❌ | 🔒 Enterprise |
| SCIM / EMU | ❌ | ❌ | 🔒 Enterprise |
| Enterpriseポリシー | ❌ | ❌ | 🔒 Enterprise |
| 監査ログストリーミング | ❌ | ❌ | 🔒 Enterprise |
| IPアローリスト | ❌ | ❌ | 🔒 Enterprise |

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

- 📋 **現在の設定を監査** — リポジトリとOrganizationの設定を確認
- 👥 **チームを整理** — チームベースのアクセス制御をセットアップ
- 🔒 **Enterprise導入を検討** — 自組織にEnterprise機能が必要か評価
- 📖 **GitHub Enterpriseドキュメント** — [docs.github.com/enterprise-cloud](https://docs.github.com/ja/enterprise-cloud@latest)
- 🎓 **GitHub認定資格** — [GitHub Administration認定](https://resources.github.com/learn/certifications/) を検討

---

## 📚 参考リンク

- [リポジトリ設定の管理](https://docs.github.com/ja/repositories/managing-your-repositorys-settings-and-features)
- [Organizationについて](https://docs.github.com/ja/organizations)
- [Organizationのロール](https://docs.github.com/ja/organizations/managing-peoples-access-to-your-organization-with-roles/roles-in-an-organization)
- [Enterpriseアカウントについて](https://docs.github.com/ja/enterprise-cloud@latest/admin/managing-your-enterprise-account/about-enterprise-accounts)
- [Enterprise Managed Usersについて](https://docs.github.com/ja/enterprise-cloud@latest/admin/identity-and-access-management/understanding-iam-for-enterprises/about-enterprise-managed-users)
- [GitHubのプラン](https://docs.github.com/ja/get-started/learning-about-github/githubs-plans)
- [GitHub 料金ページ](https://github.com/pricing)
