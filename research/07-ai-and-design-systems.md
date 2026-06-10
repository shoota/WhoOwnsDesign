---
title: "AI/LLMとデザインシステム:AIにデザインを消費させて構築させる"
topic: AI-native design systems, LLM context provisioning, design-to-code, MCP, design tokens
date: 2026-06-05
status: research-log
---

# AI/LLMとデザインシステム:AIにデザインを消費させて構築させる

> **調査目的:**  
> AIエージェントがデザインシステム(原則・トークン・制約・パターン)を読み取り、それに準拠したWeb UIを構築するために、人間側が何をどう用意すべきかの最前線を把握する。WhoOwnsDesignリポジトリの核心課題である「何を・どの粒度で・どの形式で用意するか」に直接接続する調査。

---

## 目次

1. [LLM/AIエージェントによるUI生成の現状](#1-llmaiエージェントによるui生成の現状)
2. [Design-to-Codeの手法と限界](#2-design-to-codeの手法と限界)
3. [AIにデザインシステムをコンテキストとして与える方法](#3-aiにデザインシステムをコンテキストとして与える方法)
4. [「AIが従えるデザインガイドライン」の書き方](#4-aiが従えるデザインガイドラインの書き方)
5. [AI生成UIの品質保証と検証](#5-ai生成uiの品質保証と検証)
6. [AI時代にデザインシステムドキュメントはどう変わるか](#6-ai時代にデザインシステムドキュメントはどう変わるか)
7. [既存デザインシステムのAI対応動向](#7-既存デザインシステムのai対応動向)
8. [課題・リスク](#8-課題リスク)
9. [出典](#出典)
10. [このリポジトリへの示唆](#このリポジトリへの示唆)

---

## 1. LLM/AIエージェントによるUI生成の現状

### 1.1 主要ツールの概観

| ツール | 提供元 | アプローチ | 主要特徴 |
|---|---|---|---|
| **v0** | Vercel | チャット→React生成 | shadcn/ui + Tailwind CSS。Figmaインポート対応。GitHub連携 |
| **Figma Make** | Figma | テキストプロンプト→React生成 | Figmaキャンバス統合。デザインシステム変数との連携 |
| **Builder.io Visual Copilot** | Builder.io | Figma→コード変換 | 独自AIパイプライン。Design System Indexingで既存コンポーネントに接続 |
| **Locofy.ai** | Locofy | Figma→コード変換 | 独自Large Design Model。Lightning/Classicの2モード |
| **Anima** | Anima | Figma Dev Mode統合 | Storybook双方向同期。VS Code統合。インタラクション表現が強み |
| **Cursor** | Anysphere | IDE内AIコーディング | `.cursorrules`でコードベース文脈を保持 |
| **Claude Code** | Anthropic | CLIエージェント | `CLAUDE.md`/`AGENTS.md`でプロジェクト規約を永続化 |
| **GitHub Copilot** | Microsoft/GitHub | IDE補完+Workspace | `.github/copilot-instructions.md`でルール設定 |

### 1.2 v0 by Vercel の詳細

- **動作:** テキスト/画像プロンプト → shadcn/ui + Tailwind CSS + Radix UIで構成されたReactコンポーネントを生成
- **強み:** リアルタイムプレビュー、Vercelへの直接デプロイ、Figmaデザインのインポート
- **限界:** バックエンドロジック(API、DB、認証)は生成しない。デザインシステムの独自トークンへの対応は限定的

### 1.3 AIによるUI生成の「現実的な完成度」

複数の評価によると、AIは作業の **約75%** を完了させるが、残り25%は人間によるリファクタリングが必要。ツールごとの特性は次のとおり:

- **Locofy:** 最もクリーンなコンポーネント構造。Flexboxを一貫して使用。ジュニア開発者が読めるコード
- **Builder.io:** マルチステージAIパイプラインで200万以上のデータポイントを学習。既存コードベース連携が特に強力
- **Anima:** プロトタイプとデザイン検証に特化。本番コード用途には向かない

---

## 2. Design-to-Codeの手法と限界

### 2.1 主要なアプローチ

#### (A) Figma → コード変換

Figmaファイルを解析してコードを生成する手法。成否は **Figmaファイルの構造品質** に依存する。

```
成功する条件:
- セマンティックなレイヤー名付け
- Auto Layoutの適切な適用
- Figma Variablesによるデザイントークン管理
- Code Connectによるコンポーネントマッピング

失敗する条件:
- ピクセル座標・ブレンドモードが散在するファイル
- 無名レイヤーの乱立
- トークン不使用の直接的なhex値
```

**Figma REST APIの課題:** 1つのFigmaページが数千行のJSONを生成する。このすべてをプロンプトに入れるとコンテキストウィンドウを超え、LLMがピクセル座標やブレンドモードを「読む」ことで出力品質が著しく低下する。Figma MCP Serverはこの問題を解決するために、ピクセル位置をレイアウト関係(「親要素の中央」など)に変換し、rawなhex値をトークン参照に置き換えてLLMに渡す。

#### (B) スクリーンショット → コード

マルチモーダルモデル(GPT-4 Vision、Claude、Gemini)を使ってスクリーンショットやモックアップからコードを生成する。

- Claude 3 Sonnet: 複製精度 70.31%
- GPT-4 Vision: 65.10%
- ユースケース: 既存UIのリバースエンジニアリング、ラフスケッチのプロトタイプ化

**限界:** デザインシステムとの接続がなく、生成コードはデザイントークンを含まない。ハルシネーションリスクが高い。

### 2.2 Design-to-Codeの共通限界

| 課題領域 | 詳細 |
|---|---|
| **アクセシビリティ** | 自動ツールはWCAG問題の約30%しか検出できない |
| **コード品質** | AIは人間開発者より20〜40%多いCSSと8倍以上のコード重複を生成する |
| **アニメーション** | 複雑なアニメーション・重複要素・インタラクションは信頼性の低い出力になる |
| **デザインシステム連携** | 「現在のどのAIツールもデザインシステムを効果的にサポートしていない」(2025年評価) |
| **開発効率** | 経験豊富な開発者がAI補助付きで19%遅くなったという研究も存在する |

> **重要な発見:** 成功を決めるのはツールの選択ではなく **ファイルの準備品質** である。

---

## 3. AIにデザインシステムをコンテキストとして与える方法

### 3.1 コンテキスト提供の全体像

```
人間が用意するもの              AIエージェントが受け取るもの
─────────────────────────────────────────────────────
デザイントークン (W3C形式)  →  セマンティックな変数名と用途説明
Code Connect マッピング    →  Figmaコンポーネント ↔ コードコンポーネントの対応表
llms.txt / llms-full.txt  →  構造化コンポーネント概要(5K〜1M+トークン)
DESIGN.md                 →  YAML形式のトークン + Markdown形式の設計意図
AGENTS.md / CLAUDE.md     →  プロジェクト規約・制約・禁止事項
Storybook MCP             →  コンポーネントメタデータ・propsの型・ストーリー・テスト
Builder.io Mapper Files   →  FigmaコンポーネントID ↔ コードコンポーネントの変換ルール
MCP Server                →  リアルタイムでデザインシステム情報をAIに提供
```

### 3.2 デザイントークンの機械可読提供

#### W3C Design Tokens Specification (2025.10)

2025年10月、W3C Design Tokens Community Groupがデザイントークン仕様の **初の安定版** を公表。ベンダー中立のフォーマットでデザイン決定を共有するための標準となった。

#### AIが読めるトークンの条件

AIにとって「意味のある」トークンには3つの要素が必要:

1. **セマンティック命名:** `blue-5` → `color-feedback-error`(用途を名前から理解できる)
2. **Description:** 「エラーメッセージ・破壊的ボタン背景・無効入力ボーダーに使用」などの用途説明
3. **関係性:** エラー背景色とエラーテキスト色の対応など、トークンファミリーの文書化

```json
// NG: AIには何のためのトークンか分からない
{
  "blue-5": { "value": "#2563EB" }
}

// OK: セマンティック命名 + description + 関係性
{
  "color-feedback-error": {
    "value": "#DC2626",
    "description": "Use for error messages, destructive button backgrounds, and invalid input borders.",
    "type": "color",
    "wcag": "AA",
    "pairedWith": ["color-feedback-error-text", "color-feedback-error-border"]
  }
}
```

#### 三層構造トークンアーキテクチャ

```
Primitive (原始値)        semantic (意味)              Component (コンポーネント)
color-red-600         →  color-feedback-error      →  button-destructive-bg
spacing-4             →  spacing-component-sm      →  button-padding-y
font-size-16          →  text-body-md              →  input-font-size
```

> **実証例:** Atlassianのケーススタディでは、28ファイル418個のハードコード値をゼロに削減し、64個のスペックファイルを作成した。

### 3.3 Figma Code Connect とFigma MCP Server

**発表日:** 2025年6月4日(MCP Server ベータ)

#### Code Connect の役割

Code Connectは、Figmaデザインファイルとコードリポジトリをつなぐブリッジ。設定すると、AIエージェントはコンポーネントを一から生成する代わりに **実際のコードコンポーネント** を参照して使用できる。

```
従来:  AI → Figma API (生のJSONデータ) → 新しいコンポーネントを生成
以後:  AI → Figma MCP Server → Code Connect → 既存コンポーネントを使用
```

#### Figma MCP Serverの機能

1. **Pattern Metadata:** コンポーネント参照、変数名、スタイル情報を提供
2. **Screenshots:** 異なるズームレベルでのビジュアルコンテキストを提供
3. **Code Generation:** LLMがコードベースに組み込めるReact/Tailwind表現を生成
4. **Automated Rule Generation:** コードベースをスキャンしてトークン定義・コンポーネントライブラリ・スタイル階層・命名規則をまとめた構造化ルールファイルを自動生成

> **効果:** コードの精度向上、LLMトークン使用量の削減。デザイントークン参照が適切に設定されていれば、AIは `#2563EB` ではなく `--color-brand-primary` を使用する。

### 3.4 llms.txt / llms-full.txt

**提案者:** Jeremy Howard (Answer.AI), 2024年9月  
**概念:** robots.txtを模した設計だが、検索クローラーではなくLLM取得パイプライン向け

#### Nord Design Systemの実装例

Nord Design System (Nordhealth) は現時点で最も包括的なllms.txt実装の一つ:

- **`/llms.txt`** (~5K tokens): 全コンポーネントの構造化概要 + ドキュメントリンク
- **`/llms-full.txt`** (~1M+ tokens): 実装詳細・コード例・デザイントークン・マイグレーションガイドを含む包括的ドキュメント

さらに **Agent Skills** という仕組みも提供:
- Claude Code、Cursor、Codex CLI、GitHub Copilot、Windsurf、Clineなど40以上のAIコーディングエージェントに対応
- インストールするとAIアシスタントがNordのコンポーネント・トークン・CSSフレームワーク・ベストプラクティスを理解する

### 3.5 DESIGN.md (Google Labs, 2026年4月)

**最も新しい、かつ重要な標準候補**

Google Labsが2026年4月21日にApache 2.0でオープンソース化したフォーマット仕様。72時間で5,200スターを獲得。

#### 構造

```yaml
# DESIGN.md
---
# YAMLフロントマター: 機械可読なデザイントークン
colors:
  primary: "#2563EB"
  error: "#DC2626"
  surface: "#FFFFFF"
typography:
  fontFamily: "Inter, sans-serif"
  baseSize: 16
spacing:
  unit: 4  # 4px grid
components:
  button:
    borderRadius: 6
    paddingX: 16
    paddingY: 8
accessibility:
  wcagLevel: "AA"
---

# Markdownボディ: 人間可読な設計意図とガイドライン

## デザイン原則
ボタンは行動を起こすためのもの。情報表示にはLinkを使う。
Primary buttonはページ内に1つ以下を原則とする...
```

**特徴:**
- AIエージェントはセッション開始時に一度読み込み、以降は再指示不要
- トークンスキーマはまだコミュニティ議論中(アルファ版)
- WCAG準拠チェックをAIが自律的に行える

### 3.6 AGENTS.md / CLAUDE.md / .cursorrules

AIコーディングエージェント向けのプロジェクトレベル設定ファイル。

| ファイル | 対象ツール | 特徴 |
|---|---|---|
| `AGENTS.md` | Codex CLI, Cursor, Claude Code, Continue.dev, Aider, OpenHands | OpenAI発祥の慣習がGoogle・Sourcegraph・Anthropic等に拡がった事実上の標準。2025年末〜2026年にかけて主要エージェントで採用が進んだ |
| `CLAUDE.md` | Claude Code | AGENTS.mdにフォールバック。階層的探索 |
| `.cursorrules` | Cursor | テキスト/Markdown形式 |
| `.github/copilot-instructions.md` | GitHub Copilot | Markdownのみ |

**デザインシステムルールの組み込みベストプラクティス:**

```markdown
# AGENTS.md (抜粋)

## デザインシステム規約

### コンポーネント使用
- UIコンポーネントは必ず `src/components/design-system/` から import する
- `Button` コンポーネントを使用する際: Primary は1画面に1つ以下
- 新規コンポーネントを作成する前に既存コンポーネントリストを確認する [→ docs/components.md]

### デザイントークン
- 色の指定には必ず CSS変数を使用: `--color-brand-primary` (hex直書き禁止)
- スペーシングは4px グリッドに従う: `spacing-1`=4px, `spacing-2`=8px
- 詳細: [design/tokens/README.md]

### 禁止事項
- `style="color: #..."` のインラインスタイル
- Tailwindクラスと CSS変数の混在
- shadcn/uiコンポーネントの直接改変 (拡張コンポーネントを作成すること)
```

**重要な制約:** ファイルサイズが500行を超えると大半が無視される。50行の集中したファイルが1,000行の網羅的ファイルを上回る。デザインシステム詳細は別ファイルに分離してリンク参照する。

### 3.7 Storybook MCP

**ステータス:** 2025年12月にEarly Access開始

StorybookをMCPサーバーとして公開し、AIエージェントがコンポーネントのメタデータをリアルタイムでクエリできるようにする。

**提供情報:**
- 検証済みprop types
- 使用例・JSDocの説明
- テストスイート
- コンポーネント間の関係

**効果:** AIが汎用的な訓練データパターンではなく、チームの実際のデザインシステムから正しいコンポーネントを選択・使用できる。「マージ不能なスラッジ」から「シームレスに統合されたコンポーネント」への転換。

### 3.8 Builder.io Design System Intelligence

Builder.ioは `index-repo` コマンドでコードベースをスキャンし、3フェーズで分析:

1. **コンポーネント探索とグループ化:** 全コンポーネントをスキャンしアーキテクチャ上の関係を分析
2. **詳細なコンポーネントドキュメント生成:** コンポーネントグループごとにMDXファイルを生成
3. **データ保存:** コンポーネント情報をBuilderのサーバーにアップロード

**Mapper Files:** FigmaコンポーネントとコードコンポーネントをIDレベルで接続。AIはFigmaの `Button` を見たとき自分でButtonを作らず、コードベースの `<Button>` を参照する。

---

## 4. 「AIが従えるデザインガイドライン」の書き方

### 4.1 根本原則: 記述ではなく実行可能な契約

> "LLMs don't infer intent from prose, diagrams, or Figma pages. They operate best when given explicit, machine-readable contracts."

AIへのガイドラインは **「説明する」のではなく「実行させる」** ことを目的とする。

### 4.2 Two Schema Approach (AI-Native設計の推奨構造)

**Schema 1: Design System Semantic Schema**  
「このコンポーネントは何か」を定義

```json
{
  "component": "Button",
  "identity": "Primary action trigger for user-initiated operations",
  "allowedContexts": ["forms", "dialogs", "cards"],
  "forbiddenContexts": ["navigation", "info-display"],
  "states": ["default", "hover", "active", "disabled", "loading"],
  "accessibilityRole": "button",
  "keyboardBehavior": "Enter, Space to activate",
  "containmentRules": "Cannot contain another Button"
}
```

**Schema 2: Generative Rule & Token Resolution Schema**  
「このコンテキストでUIはどう適応すべきか」を定義

```json
{
  "rule": "button-color-resolution",
  "conditions": [
    { "variant": "primary", "mode": "light", "token": "color-brand-primary" },
    { "variant": "primary", "mode": "dark", "token": "color-brand-primary-dark" },
    { "variant": "destructive", "mode": "light", "token": "color-feedback-error" }
  ],
  "precedence": ["mode", "variant", "state"],
  "safeMutationLimit": ["color", "size"],
  "immutable": ["shape", "accessibility-role"]
}
```

### 4.3 効果的なルール記述の原則

#### Do / Don't の明示

```markdown
## Buttonの使用規則

### Do
- ユーザーがアクション(送信・保存・削除)を起こす場合に使用する
- フォーム内の主要なCTAには `variant="primary"` を使用する
- ローディング状態には `isLoading` propを使用する

### Don't
- ページ内に Primary Button を2つ以上置かない
- 情報表示やナビゲーションにButtonを使わない (→ Link または TextLink を使用)
- `onClick` を省略しない
```

#### 検証可能な制約

曖昧: 「ボタンは控えめに使う」  
明確: 「1ビューポート内に Primary Button は1つ以下。複数必要な場合は Secondary または Tertiary を使う」

#### 構造化の原則

- **Positive framing:** 「〜してはいけない」より「〜の場合は〜を使う」が有効
- **計測可能な制約:** 「たくさん使わない」ではなく「最大N個」
- **代替案の提示:** 禁止するだけでなく「代わりにXを使え」と示す
- **スコープの明示:** どのコンテキスト・状態・変数に適用されるか明記

### 4.4 Progressive Disclosure (段階的開示)

コンテキストウィンドウの限界を考慮したアーキテクチャ:

```
.claude/
├── AGENTS.md              # エントリポイント (50〜300行)
│   └── → docs/design/     # 詳細はリンク参照
│
docs/design/
├── tokens.md              # トークン一覧と用途
├── components/
│   ├── button.md          # Buttonの詳細仕様
│   ├── input.md           # Inputの詳細仕様
│   └── ...
├── patterns/
│   ├── forms.md           # フォームパターン
│   └── layout.md          # レイアウトパターン
└── tokens.css             # 実際のCSSカスタムプロパティ
```

AIは必要なときに必要な文書だけ読む。全体を常時コンテキストに入れない。

---

## 5. AI生成UIの品質保証と検証

### 5.1 ビジュアルリグレッションテスト

| ツール | 特徴 |
|---|---|
| **Chromatic** | Storybook統合。ビジュアル・インタラクション・アクセシビリティの各問題をシップ前に検出 |
| **Applitools Eyes** | Vision AIでスクリーン内容を理解して意味のある変化を判定。2026年1月に Storybook Addon と Figma Plugin を追加 |
| **Percy (BrowserStack)** | AI Visual Review Agent (2025年末) が偽陽性を40%自動フィルタリング |

### 5.2 アクセシビリティ検証

- 自動ツールはWCAG問題の **約30%** しか検出できない
- 2025年6月施行のEU Accessibility Actにより、アクセシビリティ違反は制裁金対象。ただし罰則額はEU指令自体では規定されておらず加盟国ごとに異なる(EU指令は「効果的・比例的・抑止的」な罰則の設定を加盟国に義務づけるにとどまり、金額・算定方式は各国の国内立法による)
- AI生成UIの主なa11y問題: alt textの欠如、不適切なheading構造、キーボードナビゲーション不備、カラーコントラスト不足

### 5.3 デザイントークン準拠チェック

**自動化の手法:**

```bash
# Audit Scriptの例 (hvpandya.com の実装より)
# コードベースをスキャンしてハードコードされたCSS値を検出
# 正しいトークン名を提案
# CI-readyな終了コードを返す
node scripts/audit-tokens.js --exit-on-error
```

- Atlassianの実装: トークン監査スクリプトが28ファイルの418個のハードコード値を0に削減
- Builder.ioのインデックスシステム: コンポーネントのプロップ型とトークン使用を継続的に監視

### 5.4 検証ループのアーキテクチャ

```
AI生成 → 静的解析(トークン準拠) → ビジュアルリグレッション → a11yチェック → デザインレビュー
         ↑                        ↑                        ↑
     ESLint/lint     Chromatic/Applitools           axe-core/pa11y
```

最も効果的な緩和パターン: **AIをジェネレーターとして扱い、検証ループの中で動作させる。モデルの即興の余地を減らし、システムのチェック能力を高める。**

---

## 6. AI時代にデザインシステムドキュメントはどう変わるか

### 6.1 Figma Schema 2025カンファレンスからの示唆

「品質とクラフトマンシップはかつてないほど重要。プロダクト・デザイン・エンジニアリングの境界が曖昧になる中で」

**5つのシフト (Figma, 2025年):**

1. **ガイドからクラフトのキャリアへ:** デザインシステムはチームの美的センスを積極的にエンコードするリポジトリになる
2. **根拠のある探索:** AIが素早くバリエーションを生成し、デザインシステムがそれを「グラウンディング」する
3. **AI消費のための構築:** チームは人間が類推できる暗黙の知識を明示的に言語化する必要がある
4. **ガバナンスへの拡張:** DSチームは完成した作業をレビューするのではなく、制作ツールに制約を埋め込む
5. **AIによるシステム維持:** デザインシステムはAIが更新を提案する「生きたインフラ」に進化

### 6.2 ドキュメント形式の変化

| 従来 | AI時代 |
|---|---|
| 長い散文的ドキュメント | Atomic, コンポーネント直結の小さなユニット |
| Figmaのコメント・Loomの動画 | TypeScriptスキーマ・JSON仕様・YAML |
| 「なぜ」を暗黙的に | 「なぜ」を明示的に(設計意図を文書化) |
| 人間だけが読む | 人間とAIの両方が読む |
| 静的参照文書 | MCPサーバー経由でリアルタイム提供 |
| 完成したコンポーネントのカタログ | コンポーネントの許容条件・禁止条件・構成ルール |

### 6.3 「コンテキストエンジニアリング」としてのDS管理

2025年後半から「プロンプトエンジニアリング」の後継として「コンテキストエンジニアリング」が台頭。デザインシステムチームはコンポーネントライブラリの管理者であると同時に、**AIエージェントのコンテキスト設計者** としての役割を担いつつある。

> 効果: Anthropicのコンテキストエンジニアリングに関する記事(出典40)は、具体的な削減率を数値化していない。コンテキスト構造化によるトークン削減や精度向上の効果は事例ごとに異なり、「60〜80%削減」等の具体的な数値を同記事に帰属させることはできない。

### 6.4 ドキュメント品質の定量的根拠

(Atlassian ADS MCP + 構造化コンテンツ, 出典19・20。MCP未使用エージェントとの比較)

- AIの精度: 特定クエリで最大52%向上
- タスク完了速度: ADS特化タスクで平均34%向上
- ツール呼び出し回数: 26%削減
- トークン消費: 16%削減
- エラー数: 11%削減

---

## 7. 既存デザインシステムのAI対応動向

### 7.1 Shopify Polaris

**2025年安定版リリース。Web Components ベースに刷新。**

- **AI対応の動き:** "These new Polaris components are designed with modern development workflows in mind, including those that incorporate Large Language Models (LLMs)"
- **Shopify Dev MCP Server:** 最新API文書をMCPサーバー経由で提供。Cursor・GitHub Copilot・Claude Codeからアクセス可能
- **フレームワーク非依存:** Web Components により React/Vue/Vanilla JS どれとでも動作

### 7.2 Atlassian Design System (ADS)

**最も積極的な「コンテキストエンジン」化の事例**

- **ADS MCP Server (`@atlaskit/ads-mcp`):** デザイントークン・アイコン・コンポーネント情報をプログラム的にアクセス可能にするMCPサーバー
- **ADS Skills:** Rovo(AtlassianのAIエージェント)がADS準拠のデザイン・開発タスクを実行できる特殊化された能力
- **構造化コンテンツ:** Confluence/Figma/コードリポジトリ等に散在していた設計知識をTypeScriptスキーマで一元化
- **Rovo Devによる実証:** "Redesigning a Homepage in 20 Minutes with Figma MCP + ADS MCP + Rovo Dev CLI"

### 7.3 Material Design (Google)

- DESIGN.md仕様(Google Stitch→オープンソース)を通じて、デザインシステムとAI生成UIの橋渡しを標準化しようとしている
- Material Design 3はトークンファーストのアーキテクチャを採用しており、AI消費には比較的適した構造

### 7.4 Nord Design System (Nordhealth)

- デザインシステムとして先進的なAI対応の実装例
- llms.txt / llms-full.txt の実装
- Agent Skills の提供 (40+エージェント対応)

### 7.5 現状の全体評価

> 2025年12月のUX Collectiveの分析によると、**全デザインシステムの23%** しかFigma MCP活用に十分な構造を持っていないとされる。(注: この数字は一つの分析からの引用であり、産業全体の公式統計ではない)

---

## 8. 課題・リスク

### 8.1 AIハルシネーション

#### UI生成における具体的なハルシネーション例

| 種類 | 例 |
|---|---|
| **Phantom UI要素** | バックエンドサポートのないボタン、到達不能な画面 |
| **Architecture問題** | ユーザーがタスクを完了できないInfinite Loop |
| **Typography failures** | 実在しないフォントキャラクターの生成 |
| **Token hallucination** | 存在しないトークン名の参照、hex値の捏造 |
| **Component duplication** | 既存コンポーネントを無視して新規に生成 |

**根本原因:** LLMは確率的に動作する。コンテキストが曖昧な場合、統計的に多く見たパターンで穴埋めする。デザインシステムの制約が明示されていない部分は常に「即興」のリスクがある。

**軽減策:**
- AIをジェネレーターとして検証ループ内で動作させる
- デザインシステムのガードレール(既存コンポーネントライブラリへの接続)
- Human-in-the-loop: 生成後ではなく **生成前** に制約を確立する
- 監査スクリプトによる自動チェック

### 8.2 ブランド逸脱・デザイン意図の喪失

- 「ユーザーのButtonを生成した。青い背景、白いテキスト、角丸。見た目は良い。しかしそれは彼らのButtonではなかった。間違った青。間違ったRadius。間違ったSpacing。」
- Figmaの「Make Designs」機能がAppleのiOS天気アプリに酷似したデザインを生成して一時停止になった事例(意図せずした模倣リスク)
- デザインの「なぜ」(意図・ブランド価値・ユーザーの文脈)はLLMが訓練データから推測できない

**対策:** DESIGN.md・AGENTS.md等に設計意図と「なぜそのルールがあるか」を明示的に記述する。

### 8.3 一貫性の崩れ

- LLMはクロスセッションのメモリを持たない。10回目のセッションが1回目と同じ視覚品質を生成する保証がない
- 対策: 監査スクリプト + CI/CDパイプラインへの統合

### 8.4 コンテキストウィンドウの限界

- 設計文書を全部詰め込むとコンテキストウィンドウを超え、詰め込みすぎると逆に品質が下がる
- 研究結果: モデルはコンテキストを均一に使用しない。長さが増すにつれパフォーマンスは急速に不安定になる
- 解決: Progressive Disclosure。必要なときに必要な部分だけ提供する

### 8.5 アクセシビリティの自動化限界

- 自動ツールはWCAG問題の30%しか検出できない
- EU Accessibility Act (2025年6月施行): 違反で制裁金対象。罰則はEU指令自体が金額を規定しておらず加盟国ごとに異なる(EU指令は「効果的・比例的・抑止的」な罰則を各国が定めることを義務づけるにとどまる)

### 8.6 規制・コンプライアンスの不確実性

- AIコード生成の著作権問題(LLMの訓練データに含まれるコードの扱い)
- ハルシネーションに対する説明責任の所在
- ISO/IEEE等での「ハルシネーション堅牢性」標準策定が進行中(2025年時点)

---

## 出典

1. [v0 by Vercel - Build Full-Stack Web Apps with AI](https://v0.app/)
2. [Announcing v0: Generative UI - Vercel](https://vercel.com/blog/announcing-v0-generative-ui)
3. [Vercel v0 and the future of AI-powered UI generation - LogRocket](https://blog.logrocket.com/vercel-v0-ai-powered-ui-generation/)
4. [AI Figma-to-Code in 2026: Builder.io vs Locofy vs Anima - sixtythirtyten](https://www.sixtythirtyten.co/blog/from-figma-to-code-ai-design-to-dev-workflows-in-2026)
5. [Figma to Code Tools Comparison 2025 - 0xminds](https://0xminds.com/blog/guides/figma-to-code-ai-tools-2025)
6. [From Tokens to Thinking Systems: Making AI-Native Design Systems Actually Work (Medium/Digitaltableteur)](https://medium.com/digitaltableteur/from-tokens-to-thinking-systems-making-ai-native-design-systems-actually-work-46a51931e8e0)
7. [Design tokens that AI can actually read - by Romina Kavcic](https://learn.thedesignsystem.guide/p/design-tokens-that-ai-can-actually)
8. [LLMs.txt - Nord Design System](https://nordhealth.design/ai/llms-txt/)
9. [Agent Skills - Nord Design System](https://nordhealth.design/ai/skills/)
10. [AI Integration - Nord Design System](https://nordhealth.design/ai/)
11. [Design Tokens specification reaches first stable version | W3C Design Tokens Community Group](https://www.w3.org/community/design-tokens/2025/10/28/design-tokens-specification-reaches-first-stable-version/)
12. [Design Systems And AI: Why MCP Servers Are The Unlock | Figma Blog](https://www.figma.com/blog/design-systems-ai-mcp/)
13. [Introducing our Dev Mode MCP server - Figma Blog](https://www.figma.com/blog/introducing-figma-mcp-server/)
14. [Guide to the Figma MCP server - Figma Help Center](https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-the-Figma-MCP-server)
15. [Figma Design Tokens: How to Sync Design and Code](https://www.figma.com/resource-library/design-tokens/)
16. [5 Shifts Redefining Design Systems in the AI Era | Figma Blog](https://www.figma.com/blog/5-shifts-redefining-design-systems-in-the-ai-era/)
17. [Schema 2025: Design Systems For A New Era | Figma Blog](https://www.figma.com/blog/schema-2025-design-systems-recap/)
18. [Polaris Goes Stable - Shopify](https://www.shopify.com/partners/blog/polaris-goes-stable-the-future-of-shopify-app-development-is-here)
19. [Atlassian Design System: Building the context engine for the AI era](https://www.atlassian.com/blog/ai-at-work/atlassian-design-system-building-the-context-engine-for-the-ai-era)
20. [Teaching AI to speak our design language - Inside Atlassian](https://www.atlassian.com/blog/ai-at-work/teaching-ai-to-speak-our-design-language)
21. [@atlaskit/ads-mcp - npm](https://www.npmjs.com/package/@atlaskit/ads-mcp)
22. [Redesigning a Homepage in 20 Minutes: Figma MCP + ADS MCP + Rovo Dev CLI](https://www.atlassian.com/blog/development/redesigning-homepage-20-minutes-with-rovo-dev)
23. [AI Design Hallucination: Examples, Causes, and Mitigation Strategies - Trinetix](https://www.trinetix.com/insights/ai-design-hallucination)
24. [Supercharge Your Design System with LLMs and Storybook MCP | Codrops](https://tympanus.net/codrops/2025/12/09/supercharge-your-design-system-with-llms-and-storybook-mcp/)
25. [Storybook MCP sneak peek](https://storybook.js.org/blog/storybook-mcp-sneak-peek/)
26. [Expose your design system to LLMs · Hardik Pandya](https://hvpandya.com/llm-design-systems)
27. [When design system documentation becomes AI-readable (Medium/Design Bootcamp)](https://medium.com/design-bootcamp/when-design-system-documentation-becomes-ai-readable-14f7a3180233)
28. [AI-Ready Design Systems: Preparing Your Design System for Machine-Powered Product Development | Supernova.io](https://www.supernova.io/blog/ai-ready-design-systems-preparing-your-design-system-for-machine-powered-product-development)
29. [Your Design System Isn't AI-Readable Yet (Medium)](https://medium.com/@mohitphogat/your-design-system-isnt-ai-readable-yet-168aca6d2e13)
30. [Figma to Code - How to use your design system with AI | Builder.io](https://www.builder.io/blog/figma-design-system-component-mapping)
31. [Builder Design System Intelligence | Builder.io Docs](https://www.builder.io/c/docs/fusion-design-system-intelligence)
32. [CLAUDE.md, AGENTS.md & Copilot Instructions: Configure Every AI Coding Assistant - DeployHQ](https://www.deployhq.com/blog/ai-coding-config-files-guide)
33. [DESIGN.md - Google Stitch Open-Source Announcement](https://blog.google/innovation-and-ai/models-and-research/google-labs/stitch-design-md/)
34. [google-labs-code/design.md - GitHub](https://github.com/google-labs-code/design.md)
35. [DESIGN.md Goes Open Source - AI Agents Get a Style Sheet | Awesome Agents](https://awesomeagents.ai/news/google-design-md-open-source-spec/)
36. [AGENTS.md, SKILL.md, DESIGN.md: How AI Instructions Split into Three Layers - DEV Community](https://dev.to/aws-builders/agentsmd-skillmd-designmd-how-ai-instructions-split-into-three-layers-d0g)
37. [What Is DESIGN.md and How to Use It | Banani](https://www.banani.co/blog/design-md-guide)
38. [Chromatic - Frontend UI Testing & Review Platform](https://www.chromatic.com/)
39. [AI in Design Systems: Consistency Made Simple | UXPin](https://www.uxpin.com/studio/blog/ai-design-systems-consistency-simple/)
40. [Context Engineering for AI Agents - Anthropic](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
41. [Bridging Design and Code: Figma MCP, Code Connect, and Figma Make (Medium)](https://michel-velis.medium.com/bridging-design-and-code-a-comprehensive-guide-to-figma-mcp-code-connect-and-figma-make-1d5c5afef8fe)
42. [How We Use AI to Turn Figma Designs into Production Code - monday engineering](https://engineering.monday.com/how-we-use-ai-to-turn-figma-designs-into-production-code/)
43. [Automatically Generating UI Code from Screenshot - arXiv](https://arxiv.org/html/2406.16386v1)
44. [screenshot-to-code/blog/evaluating-claude.md - GitHub (abi)](https://github.com/abi/screenshot-to-code/blob/main/blog/evaluating-claude.md)

---

## このリポジトリへの示唆

WhoOwnsDesignの核心課題「AIにデザインシステムを消費させてWebアプリを構築する際、何を・どの粒度で・どの形式で用意すべきか」への直接的な示唆を以下にまとめる。

### 示唆 1: 参照実装を作らない代わりに「実行可能な仕様書」を作る

本リポジトリは参照実装(コード)を持たない方針だが、AIが実際に使えるのは **実行可能な仕様書** だ。prose(散文)ではなくmachine-readable contract(機械可読な契約)を提供する必要がある。最低限必要な成果物:

- **デザイントークン仕様:** W3C Design Tokens形式に準拠したJSON。生の値だけでなく `description`・`type`・用途制約・ペア関係を含む
- **コンポーネント意味スキーマ:** 各コンポーネントの「何か」(identity, allowedContexts, states, accessibility)を定義するJSON/YAML
- **生成ルールスキーマ:** 「どのコンテキストでどのトークンを使うか」を条件分岐で定義

### 示唆 2: DESIGN.md / AGENTS.md を文書の主軸に据える

Google LabsのDESIGN.md(2026年4月オープンソース化)は、このリポジトリが定義しようとしているものの「業界標準候補」になりつつある。WhoOwnsDesignは:

- DESIGN.md形式でデザイントークンと設計意図を表現する例を提示すべき
- AGENTS.md/CLAUDE.mdでのデザインシステムルール記述のテンプレートを提供すべき
- 「ファイルのサイズと粒度」についてのガイドライン(500行以下、詳細は別ファイルへ委譲)を定めるべき

### 示唆 3: 「なぜ」の明示化が最重要

AI が推測できないのはコンテキストと意図だ。人間のデザイナーが暗黙に共有している「なぜこのルールがあるか」を全て言語化しなければならない。

> 悪い例: "Use primary button for primary actions."  
> 良い例: "Use primary button when the action is the single most important next step for the user. Limit to one per viewport. If two compete, one must be secondary. Reason: Multiple primary buttons dilute visual hierarchy and confuse action priority."

WhoOwnsDesignが定義するドキュメント標準は、全ての制約に「理由」のフィールドを必須にすることを検討すべき。

### 示唆 4: MCP対応は「オプション」ではなく「インフラ」

Figma MCP Server・Storybook MCP・Atlassian ADS MCP・Nord Agent Skillsの事例は、MCPサーバーがデザインシステムとAIエージェントをつなぐ標準インフラになることを示している。参照実装を持たないWhoOwnsDesignでも:

- どのツールにMCPサーバーが存在するかのカタログ
- MCPサーバーの有無に応じた「デザインシステムの渡し方」の分岐フローチャート
- Code Connectのセットアップガイドへの言及

を含めることが、実践的価値を高める。

### 示唆 5: llms.txtとllms-full.txtの二層構造を採用する

Nord Design Systemの実装から学べる構造:

```
/llms.txt        → 5K tokens以内。コンポーネント一覧・主要トークン・主要パターンのリンク集
/llms-full.txt   → 200K tokens+。全仕様。大きなコンテキストウィンドウのツール向け
/components/
  ├── button.md  → コンポーネント個別仕様 (オンデマンドで読み込む)
  └── ...
```

WhoOwnsDesignは「このリポジトリのllms.txt」を作成し、AIがこのリポジトリ自体を理解して活用できるようにすることを検討すべき。

### 示唆 6: 検証可能性を設計に組み込む

ドキュメントの制約はAIが「検証できる形式」で記述することが重要。「控えめに使う」は検証不能だが「1ビューポートに最大1つ」はlintルールにできる。WhoOwnsDesignが定義する文書テンプレートには:

- **機械的に検証可能な制約:** カウント・比率・token名前空間など数値化できるもの
- **人間がレビューする制約:** ブランドボイス・デザイン意図など

の明確な区分を設けることを推奨する。

### 示唆 7: コンテキストウィンドウを「設計パラメーター」として扱う

LLMのコンテキストウィンドウは設計上の制約だ。全ての文書を一度に詰め込もうとしてはいけない。WhoOwnsDesignは「AIへの情報の渡し方」として:

- **Session-start context:** 毎回必ず読ませるもの (AGENTS.md, 最重要トークン: 300行以内)
- **On-demand context:** 特定タスク時に追加で参照させるもの (コンポーネント仕様, パターン詳細)
- **Never-in-context:** AIには渡さずCI/ツールで検証するもの (全トークン一覧, 変更履歴)

の三層に分類した文書アーキテクチャを定義すべき。

### 示唆 8: 「AI対応度の評価基準」を定義する

現状、全デザインシステムの約77%はAIが活用できる構造になっていない。WhoOwnsDesignは、デザインシステムがAIに消費されやすいかどうかを評価する **チェックリスト** を提供できる:

- [ ] デザイントークンはW3C Design Tokens形式でJSONエクスポートできるか
- [ ] トークンはセマンティック命名か (color-primary ではなく color-brand-action-primary)
- [ ] 各トークンにdescription・用途例・WCAG情報が付いているか
- [ ] コンポーネントにallowedContexts・forbiddenContextsが文書化されているか
- [ ] do/don't・代替案が全制約に付いているか
- [ ] llms.txtまたはAGENTS.mdにデザインシステムへの入口が記述されているか
- [ ] Code Connect / Mapper Files でFigmaとコードが接続されているか
- [ ] デザイントークン準拠を検証する自動スクリプトがCI/CDに組み込まれているか
