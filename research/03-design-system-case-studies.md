---
title: 主要デザインシステムの事例比較研究
topic: デザインシステム文書構造・設計原則・トークン公開方法の横断比較
date: 2026-06-05
status: research-log
---

# 主要デザインシステムの事例比較研究

本ドキュメントは、AIを活用したWebアプリケーション構築を支援するための文書設計の参照点を確立するため、世界の代表的なデザインシステムの公開ドキュメント構造・設計原則・基礎定義・トークン公開方法を調査した内部調査ログである。

---

## 1. Material Design 3（Google）

**公式サイト:** https://m3.material.io/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| Get Started | 概要・導入ガイド |
| Foundations | 用語集・アクセシビリティなど基礎概念 |
| Styles | Color / Shape / Typography |
| Components | ボタン・カード等のUIコンポーネント群 |
| Blog | 最新情報・事例 |

- **Styles** セクションが「色・形・タイポ」を担い、他システムの「Foundations」に相当する役割を果たす。
- Material Design 3（Material You）は2021年に登場。2025年には **Material 3 Expressive** としてさらに進化。

### デザイン原則（Design Principles）

Material Design 3 は以下の3つを核心原則として掲げる：

1. **Personal（個人的）** — ユーザーの壁紙からダイナミックカラーパレットを自動生成し、各個人に合わせた配色体験を提供する。
2. **Adaptive（適応的）** — デバイス・画面サイズ・ユーザー環境に応じてレイアウトやコンポーネントが柔軟に変化する。
3. **Expressive（表現的）** — デザインの合理性と個人の好みの緊張感を大切にし、感情に響く体験を志向する。

### Foundations / 基礎に含まれるもの

| カテゴリ | 概要 |
|---|---|
| Color | Dynamic Color / トーンパレット13階調 / セマンティックカラー |
| Typography | タイプスケール / 可読性・階層 |
| Shape | コーナーRadius スケール（正方形〜完全円形） |
| Motion | スプリングベースのアニメーション / Spatial Springs / Effects Springs |
| Interaction | フォーカス・ホバー・ドラッグなどのインタラクション状態 |
| Layout | 大画面対応レイアウトガイドライン |
| Elevation | Z軸方向のサーフェス距離 |

### コンポーネント以外に何を定義しているか

- **Adaptive Layout** パターン（レスポンシブ設計）
- **Accessibility**（色・タイポ・モーション全体にインクルーシブ設計を内包）
- **Design Tokens**（カラートークン・タイプトークン・シェイプトークン等）

### デザイントークンの公開方法

- [Material Theme Builder](https://m3.material.io/theme-builder) でカラートークンをGUI生成・エクスポート可能
- CSS / Android / Flutter / Jetpack Compose 向けに変換出力
- GitHub: `material-components` 以下に各プラットフォーム実装

---

## 2. Human Interface Guidelines（Apple）

**公式サイト:** https://developer.apple.com/design/human-interface-guidelines/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| Platforms | iOS / iPadOS / macOS / watchOS / tvOS / visionOS 別ガイドライン |
| Foundations | アクセシビリティ・色・ダークモード・アイコン・レイアウト・モーション・SF Symbols・タイポグラフィ等 |
| Patterns | ナビゲーション・検索・オンボーディング・ドラッグ&ドロップ等 |
| Components | ボタン・タブバー・スライダー・アラート等のUIコンポーネント |
| Inputs | タッチ・キーボード・マウス・Apple Pencil・視線追跡等の入力方式 |
| Technologies | Widget / Live Activities / SharePlay / ARKit等のプラットフォーム機能 |

### デザイン原則（Design Principles）

Appleは3つの核心原則を長年一貫して掲げる：

1. **Clarity（明瞭性）** — テキストは読みやすく、アイコンは明確で、装飾は機能を補佐する。
2. **Deference（敬意）** — UIはコンテンツの邪魔をせず、ユーザーが見るべきものを際立たせる。
3. **Depth（奥行き）** — レイヤー・シャドウ・モーションが視覚的ヒエラルキーを伝える。

2025年には **Liquid Glass** と呼ばれる大幅なビジュアルリデザインが導入され、半透明・奥行き・流動的応答性が全プラットフォームで強調されるようになった。

### Foundations / 基礎に含まれるもの

Accessibility / App Icons / Branding / Color / Dark Mode / Icons / Images / Layout / Materials / Motion / Right-to-Left / SF Symbols / Typography

### コンポーネント以外に何を定義しているか

- **Patterns**（ナビゲーション・モーダル・フィードバック等の共通パターン）
- **Technologies**（プラットフォーム固有機能の設計ガイド）
- **Inputs**（多様な入力デバイスへの対応）
- アクセシビリティ（プラットフォーム横断で徹底）

### デザイントークンの公開方法

- **Figmaリソース**（Apple Design Resources）としてUIキットを提供
- システムカラー・SF Symbols は API / SDK 経由でアクセス
- コードレベルのトークン公開はなく、UIKit / SwiftUI のセマンティックカラー API が事実上のトークン相当

---

## 3. Polaris（Shopify）

**公式サイト:** https://polaris.shopify.com/ → https://polaris-react.shopify.com/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| Getting Started | 導入ガイド |
| Foundations | 管理画面体験の根幹となる設計ガイダンス |
| Design | Pro Design Language / Color / Depth / Icons / Layout / Motion / Typography / Data Viz / Illustrations / Interaction States / Sounds |
| Content | Fundamentals / Grammar / Error Messages / Naming / Alt Text / Inclusive Language |
| Patterns | 繰り返し登場するUXパターン |
| Components | 90件前後のReactコンポーネント（廃止予定含む。前回承認者検証時点で92件、2025年時点） |
| Tokens | カラー・スペーシング・タイポグラフィのトークン |
| Icons | 400+ 商取引テーマのアイコン |
| Contributing | 貢献ガイドライン |
| Tools | 設計・開発支援ツール |

### デザイン原則（Design Principles）

Polaris の公式ドキュメントは「Experience Values（体験の価値観）」として以下の6つを掲げる（出典: polaris-react.shopify.com/foundations/experience-values）：

1. **Considerate（思いやり）** — あらゆるプラットフォーム・状況のユーザーの仕事をより良くする配慮。
2. **Empowering（力を与える）** — ユーザーが自律的にタスクを達成できるよう支援する。
3. **Crafted（丁寧に作られた）** — プロ用ツールのパワーと消費者向け製品のシンプルさを両立した体験。
4. **Efficient（効率的）** — 合理化されたワークフローと自動化により、少ない努力で目標を達成できる。
5. **Trustworthy（信頼できる）** — 透明性と細部への注意によって信頼を構築する。
6. **Familiar（親しみやすい）** — 一貫したパターンでエコシステム全体の直感性を高める。

> 注: 一部の外部解説サイトでは「Usability / Consistency / Accessibility / Scalability」の4原則として紹介されることがあるが、これらは公式ドキュメントの用語ではない。

### Foundations / 基礎に含まれるもの

Design セクション内に Color / Depth / Layout / Typography / Motion / Icons / Data Visualizations / Illustrations / Interaction States / Sounds を収録。**Content** セクションが独立しライティングガイドを担う。

### コンポーネント以外に何を定義しているか

- **Content**（ボイス&トーン / 文法 / エラーメッセージ / 命名 / 代替テキスト / インクルーシブランゲージ）
- **Patterns**（UXパターン）
- **Tokens**（CSS Custom Properties / JSON 形式で npm 公開）

### デザイントークンの公開方法

- npm: `@shopify/polaris-tokens`
- CSS: `import '@shopify/polaris-tokens/css/styles.css'`
- JSON: トークングループ別にエクスポート可能
- GitHub: https://github.com/Shopify/polaris-tokens

---

## 4. Carbon Design System（IBM）

**公式サイト:** https://carbondesignsystem.com/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| All About Carbon | Carbonの概要・エコシステム・ケーススタディ |
| Designing | デザイナー向け入門・Figmaリソース |
| Developing | 開発者向け入門・フレームワーク別 |
| Guidelines | コンテンツガイドライン・アクセシビリティ |
| Elements | Color / Typography / Spacing / Icons / 2×Grid |
| Components | 70+ コンポーネント（使用方法・スタイル・コード） |
| Patterns | ベストプラクティスパターン（ハーベスト＆承認制） |
| Data Visualization | チャート・グラフ等のデータ可視化 |
| Community Assets | コミュニティ制作資産 |

### デザイン原則（Design Principles）

5つの原則（IBM Design Language の「人間中心設計」が根底にある）：

1. **Open（オープン）** — オープンソースの精神に基づき、ユーザーが貢献者でもある分散型開発を推進。
2. **Inclusive（インクルーシブ）** — 能力・状況に関わらず全ての人が利用できるよう設計・構築。
3. **Modular & Flexible（モジュラー&柔軟）** — コンポーネントはどのような組み合わせでも美しく連携する。
4. **User-First（ユーザーファースト）** — 厳密なユーザーリサーチに基づき、実際の人間のニーズを最優先。
5. **Consistency-Building（一貫性の構築）** — IBM Design Language を基盤に全要素が調和して動作することを保証。

### Foundations / 基礎に含まれるもの

Elements セクション: **Color Tokens / Type Tokens / Spacing Tokens / Icon / 2×Gridシステム**

### コンポーネント以外に何を定義しているか

- **Content Guidelines**（ライティング / ボイス&トーン）
- **Data Visualization**（独立セクション、チャート・メトリクス等）
- **Patterns**（ベストプラクティス、ガバナンス審査済み）
- **Accessibility**（WCAG 2.1 AA 準拠、コンポーネント内にも明示）

### デザイントークンの公開方法

- npm: `@carbon/colors`, `@carbon/type`, `@carbon/layout`, `@carbon/themes`
- JSON / CSS Custom Properties / Sass variables の多形式対応
- Style Dictionary ベースのトークン変換パイプライン

---

## 5. Primer（GitHub）

**公式サイト:** https://primer.style/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| Product UI | GitHub UIコンポーネント群 |
| Brand UI | マーケティング・ブランド向けデザインコンポーネント |
| Octicons | GitHub製SVGアイコンライブラリ |
| Accessibility | アクセシブルインターフェース構築リソース |
| Brand Toolkit | ブランド資産（外部向け） |
| Primitives | カラー・スペーシング・タイポグラフィのデザイントークン |

### デザイン原則（Design Principles）

Primer は「GitHub のための設計システム」として以下を重視：

- **Accessibility First** — カラーシステムは機能的アクセシビリティを核心に構築（ライト / ダーク / ハイコントラストモード対応）
- **Open-source collaboration** — GitHubのオープンソース文化を体現した分散型貢献モデル
- **Developer-centric** — CSS・React・ViewComponents（Ruby on Rails）等の開発者向け実装を重視

### Foundations / 基礎に含まれるもの

Primitives として: **Color Tokens / Spacing / Typography** をnpm経由で公開。アクセシビリティが横断的基礎として機能。

### コンポーネント以外に何を定義しているか

- **Accessibility**（専用セクション、インクルーシブ設計ガイド）
- **Octicons**（アイコンライブラリ、独自エコシステム）
- **Brand UI**（Product UIとは分離された独立系統）

### デザイントークンの公開方法

- npm: `@primer/primitives`（バージョン11.9.0、25+ プロジェクトが依存）
- JSON形式でカラー・スペーシング・タイポグラフィトークンを配布
- 機能的カラーシステム（セマンティックトークン）でカラーモードを管理

---

## 6. Fluent / Fluent 2（Microsoft）

**公式サイト:** https://fluent2.microsoft.design/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| Design | FigmaUIキット（Web / iOS / Android / Windows別） |
| Develop | インストールガイド・フレームワーク別実装 |
| Components | Web / iOS / Android / Windows の各コンポーネント |
| Design Tokens | グローバルトークン / エイリアストークン |
| Typography | タイポグラフィシステム |
| Color | カラーシステム（中立 / 共有 / ブランドパレット） |
| Accessibility | WCAG 2.1 AA準拠ガイドライン |
| Resources | アクセシビリティツール・Figmaプラグイン |

### デザイン原則（Design Principles）

Fluent 2 は明示的な原則リストよりも「アクセシビリティ・クロスプラットフォーム一貫性・クリエイティビティ」を設計の軸として強調する。Microsoft全製品（Teams・Outlook等）への展開を念頭においた統一感を重視。

### Foundations / 基礎に含まれるもの

- **Color**: 中立 / 共有 / ブランドパレット、セマンティックカラー（赤＝危険、黄＝注意、緑＝正常）
- **Typography**: Segoe UIを基本としつつネイティブフォントへのフォールバック、セマンティックタイプトークン
- **Design Tokens**: 2層構造（グローバルトークン＋エイリアストークン）
- **Accessibility**: WCAG 2.1 AA 準拠（通常テキスト4.5:1、大テキスト3:1以上）

### コンポーネント以外に何を定義しているか

- **Content Guidelines**（ボイス・トーン・用語推奨）
- **Accessibility**（フォーカス順序・カラーコントラストFigmaプラグイン）
- クロスプラットフォーム実装ガイド

### デザイントークンの公開方法

- **2層トークン構造**: Global Tokens（生値: hex等） + Alias Tokens（セマンティック意味付け）
- テーマ: ライト / ダーク / ハイコントラスト / ブランドバリアント
- npm: `@fluentui/tokens`
- Fluent UI Web Components向けデザイントークン API をMicrosoft Learn で公開

---

## 7. Spectrum（Adobe）

**公式サイト:** https://spectrum.adobe.com/ / Spectrum 2: https://s2.spectrum.adobe.com/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| Principles | デザイン原則 |
| Inclusive Design | インクルーシブ設計ガイドライン |
| Color System | カラーシステム・セマンティックカラー |
| Color Fundamentals | 色の基礎 |
| Typography | タイポグラフィ |
| Iconography | アイコン |
| Motion | モーションガイドライン |
| Design Tokens | トークンの命名・構造・使い方 |
| Components | UIコンポーネント群 |
| Content / Copywriting | ライティング・ボイス&トーン・文法 |
| Application Frame | アプリフレーム構成ガイドライン |

### デザイン原則（Design Principles）

Spectrum（特にSpectrum 2）は以下を核心に据える：

- **Inclusive（インクルーシブ）** — すべての人が使えるアクセシブルな設計（視覚的知覚・色覚・認知多様性への対応）
- **Approachable（親しみやすい）** — 親しみやすく楽しい体験、感情的な共感を重視
- **Coherent（一貫している）** — Adobe 100+製品間でコンテキスト・コヒーレント・パフォーマンス的な体験を統一
- **Perceivable（知覚可能）** — 視覚的知覚・インクルーシブデザイン・アクセシビリティを基盤に置く

### Foundations / 基礎に含まれるもの

Color System / Color Fundamentals / Typography / Iconography / Motion / Inclusive Design / Design Tokens

### コンポーネント以外に何を定義しているか

- **Inclusive Design**（色覚・認知・運動能力の多様性への対応を専用セクションで解説）
- **Content / Copywriting**（ボイス&トーン / 文法 / インクルーシビティ / エラーメッセージパターン）
- **Application Frame**（アプリ全体の構成ガイドライン）
- **Design Tokens**（専用ページで命名規則・構造を解説）

### デザイントークンの公開方法

- すべての設計属性（色・タイポ・レイアウト・アニメーション）をデザイントークンとして提供
- `@adobe/spectrum-tokens` (npm)
- GitHub: `adobe/spectrum-design-data`（JSON形式のデザインデータ・スキーマ・ツーリング）
- Spectrum CSS / React Spectrum / Spectrum Web Components のオープンソース実装と連動

---

## 8. Atlassian Design System

**公式サイト:** https://atlassian.design/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| Foundations | Design Tokens / ガイドライン（アクセシビリティ・コンテンツ） / スタイル（色・スペーシング・タイポグラフィ等） |
| Components | 再利用可能なUIコンポーネント |
| Patterns | 特定インタラクション向けガイダンス |
| Tools | 実装支援ツール |

Foundation 内のスタイルカテゴリ: **Spacing / Grid / Color / Typography / Iconography / Illustrations / Logos / Elevation / Border / Radius**

### デザイン原則（Design Principles）

3つの原則（内部的な実践原則として公開）：

1. **Trusted Fundamentals Before Comprehensive Patterns** — まず基礎的な問題を解決し、複雑で独自な体験を構成できる意見のある基礎部品を提供。
2. **Meet System Needs Before Delivering Individual Features** — ドキュメント・サポート・ツーリング・メンテナンスをフィーチャー提供と並行し、多様な視点と継続的フィードバックを取り込む。
3. **Bring People on the Journey Before Helping for the Moment** — チームをデザインプロセスに巻き込み、セルフサービスで自信を持ってシステムを使えるよう最適化。

### Foundations / 基礎に含まれるもの

**Design Tokens**（単一信頼源として色・スペーシング・タイポグラフィ等を管理） / Accessibility / Content Guidelines / Color / Typography（Atlassian Sans / Atlassian Mono / rem単位） / Spacing / Grid / Iconography / Illustrations / Logos / Elevation / Border / Radius

### コンポーネント以外に何を定義しているか

- **Accessibility**（Foundation内に組み込み）
- **Content Guidelines**（明確・簡潔・会話的なライティング原則）
- **AI Design Patterns**（Rovo / AI体験向け専用パターンセクション）
- **Illustrations / Logos**（ビジュアルアイデンティティ資産も含む）

### デザイントークンの公開方法

- npm: `@atlaskit/tokens`（バージョン12.0.0、280+ プロジェクトが依存）
- CSS Custom Properties としてアクセス（`token()` メソッド推奨）
- カラー / エレベーション / スペーシング / その他スタイルを標準化中

---

## 9. Lightning Design System（Salesforce）

**公式サイト:** https://www.lightningdesignsystem.com/

### トップレベル文書構造（SLDS 1 + SLDS 2）

| セクション | 内容 |
|---|---|
| Design Tokens | アトミックな設計変数（色・フォント等） |
| Utilities | CSSユーティリティクラス |
| Component Blueprints | フレームワーク非依存のHTML/CSSコンポーネント |
| Guidelines | 設計ガイドライン群（下記参照） |
| Patterns | ユーザーパターン（SLDS 2では専用ページ） |

**Guidelines** サブセクション: Builder / Color / Conversation Design / Data (Entry / Visualization) / Layout & Navigation / Messaging UI / Notifications / Search / User Engagement

### デザイン原則（Design Principles）

4つの核心原則：

1. **Clarity（明瞭性）** — 曖昧さを排除し、ユーザーが確信を持って見・理解し・行動できるようにする。
2. **Efficiency（効率性）** — ワークフローを合理化・最適化し、より賢くより速い仕事を知的に支援する。
3. **Consistency（一貫性）** — 同じ問題に同じ解決策を適用することで親しみやすさを生み直感を強化。
4. **Beauty（美しさ）** — 思慮深くエレガントなクラフツマンシップで人々の時間と注意への敬意を示す。

### Foundations / 基礎に含まれるもの

Design Tokens（アトミック要素として色・テキスト属性を変数化） / Guidelines（インタラクションパターン・マークアップ・スタイリング・ボイス&トーン・アクセシビリティ）

### コンポーネント以外に何を定義しているか

- **Conversation Design**（AIボット設計・言語スタイル・インクルーシブ実践）
- **Data Visualization**（チャート・メトリクス専用ガイドライン）
- **User Engagement**（オンボーディング・機能発見・ヘルプリソース）
- **Notifications**（アプリ内通知・モバイルプッシュ設計）
- SLDS 2 では AI対応設計基盤・ダークモード・スタイリングフックを強化

### デザイントークンの公開方法

- Lightning Aura Components および Lightning Web Components のデザイントークン API
- CSS カスタムプロパティ（スタイリングフック）で動的カスタマイズ
- SLDS Linter / SLDS Validator でトークン利用を静的解析

---

## 10. U.S. Web Design System（USWDS）

**公式サイト:** https://designsystem.digital.gov/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| How to Use USWDS | 入門・ドキュメント・アクセシビリティ・パフォーマンス・移行ガイド |
| Design Principles | 5つのデザイン原則 |
| Components | 40+ UIコンポーネント |
| Patterns | 複雑ワークフロー向けUXガイダンス |
| Design Tokens | 色・タイポグラフィ・スペーシング等の基礎 |
| Utilities | レイアウト・スペーシング・スタイリング用CSSユーティリティ |
| Templates | 404 / 認証 / フォーム / ドキュメント等の既成ページレイアウト |

### デザイン原則（Design Principles）

5つの原則（政府サービス設計に特化）：

1. **Start with real user needs（本当のユーザーニーズから始める）** — 実際のユーザーを最初からプロジェクトに参加させ、仮定を検証し続ける。
2. **Earn trust（信頼を獲得する）** — 政府サイトは信頼性・一貫性・誠実さを体現し、安全な接続・正確なコンテンツ・迅速な問題対応が必要。
3. **Embrace accessibility（アクセシビリティを受け入れる）** — Section 508・WCAG 2.1を法的基準として遵守し、多様な能力のユーザーでテストする。
4. **Promote continuity（継続性を促進する）** — 政府サービス全体でデバイスを超えた一貫体験を提供し、ユーザーの混乱を最小化する。
5. **Listen（耳を傾ける）** — 分析・ユーザーリサーチ・調査・直接観察による継続的改善を行う。

### Foundations / 基礎に含まれるもの

**Design Tokens**（Color / Typesetting / Flex / Opacity / Shadow / Spacing Units / Z-index） / Accessibility / Community

### コンポーネント以外に何を定義しているか

- **Design Principles**（独立セクションとして5原則を詳述）
- **Patterns**（複雑なユーザーワークフロー向けUXガイダンス）
- **Utilities**（CSSユーティリティクラス群）
- **Templates**（既成ページレイアウト）
- 公式サイト記載では「dozens of agencies and nearly 200 sites」をサポート（連邦政府全体の.govサイトの一部）

### デザイントークンの公開方法

- npm: `@uswds/uswds`（v3.13.0）
- Sass変数・CSS Custom Properties 形式
- `dist/` ディレクトリにコンパイル済みアセット、`packages/` にコンポーネントソース
- [Design Tokens ページ](https://designsystem.digital.gov/design-tokens/)で全トークンをドキュメント化

---

## 11. Base（Uber）

**公式サイト:** https://base.uber.com/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| Welcome to Base | システム概要 |
| Foundations | システムの構成・ビジュアル言語の基礎 |
| Components | スペック・ガイドライン・振る舞い・使用方法 |
| Patterns | 広範なデザインパターン・モジュールライブラリ |
| Principles | デザイン原則 |
| Content Design | コンテンツデザインシステム |
| FAQs | よくある質問 |

### デザイン原則（Design Principles）

Base はUberのエコシステム全体で一貫したブランドアイデンティティとユーザー体験を維持しながら、革新を可能にする柔軟なガイドラインを重視。具体的な原則名称は公開サイトに詳述。

### 特徴

- Uber の多様なプロダクト（配車・フードデリバリー・貨物等）全体に適用される単一フレームワーク
- **Content Design** を独立したセクションとして設けるのが特徴的

---

## 12. Pajamas（GitLab）

**公式サイト:** https://design.gitlab.com/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| Get Started | 導入・ナビゲーション構造の説明 |
| Foundations | 色・タイポグラフィ・アイコノグラフィ等のコアビジュアル属性 |
| Components | アバター・ボタン・コンボボックス等の単機能UIコンポーネント |
| Directives | Vue.jsの再利用可能なカスタム動作（クリック検出・コンテンツサニタイズ等） |
| Patterns | 繰り返し登場するインタラクションフロー |
| Objects | ジョブ・マージリクエスト等の概念的ビルディングブロック |
| Data Visualization | データセットからインサイトを抽出するツール |
| Content | トーン・ボイス・文法ガイドライン |

### デザイン原則（Design Principles）

10の核心設計原則：

1. **Sophisticated Simplicity（洗練されたシンプルさ）** — 複雑なワークフローを合理化する思慮深い選択
2. **Design for Natural Developer Flow（自然な開発者フローの設計）** — コンテキストスイッチなしでシームレスな作業を実現
3. **Start Simple, Reveal Complexity Gradually（シンプルに始め複雑さを段階的に露出）** — 必要な情報のみを最初に表示
4. **Make Everything Feel Like One Product（すべてを1つの製品に感じさせる）** — 知識が機能間で転用できる一貫パターン
5. **Make Every Interaction Feel Instant（すべてのインタラクションを瞬時に感じさせる）** — 速度を最初から優先
6. **Show What's Happening and Why（何が起きているか・なぜかを示す）** — システム状態の常時可視化
7. **Build for Teams, Not Just Individuals（個人だけでなくチームのために構築）** — 共有・議論・協働を促進
8. **Always Provide a Clear Next Step（常に明確な次のステップを提示）** — 問題や数値だけでなく解決策も必ず提示
9. **Enable Customization Within Guardrails（ガードレール内でカスタマイズを許可）** — 柔軟性と一貫性のバランス
10. **Design for Scale and Compliance Always（常にスケールとコンプライアンスを念頭に設計）** — ガバナンス・セキュリティ・監査を初期段階から組み込む

### 特徴

- **Directives** / **Objects** という独自カテゴリを持つ（Vue.js 中心の技術スタックを反映）
- **Data Visualization** を独立セクションとして重視

---

## 13. Garden（Zendesk）

**公式サイト:** https://garden.zendesk.com/

### トップレベル文書構造

| セクション | 内容 |
|---|---|
| Design | Foundations（Color / Palette / Icons）|
| Content | ライティング原則（現在は内部Zendeskスペースに移管） |
| Components | ユーザーインターフェースコンポーネント |

### デザイン原則（Design Principles）

- 装飾ではなく機能: すべての色選択・スペーシング・タイポグラフィスケールは特定の機能を果たすための存在
- カラーは状態を伝え、ヒエラルキーが注意を誘導する
- ユーザーが完了すべきタスクを中心にコンポーネントを構築
- WCAG 2.1 対応（カラーコントラスト・キーボードナビゲーション・フォーカスインジケーター・セマンティックマークアップを必須とみなす）

### 特徴

- ベース4システム（4pxの倍数）を採用
- デフォルトフォント: San Francisco（SF）、14px / 20px line-height
- コンテンツガイドラインは内部化されており公開範囲が限定的

---

## 横断比較表

| デザインシステム | 組織 | トップ構造（主要セクション） | 原則の数 | コンテンツ/ライティングガイド | トークン公開形式 | 特徴・特記事項 |
|---|---|---|---|---|---|---|
| Material Design 3 | Google | Foundations / Styles / Components | 3（Personal / Adaptive / Expressive） | なし（公開） | CSS / JSON / 各プラットフォーム | Dynamic Color / Material You / Material Expressive (2025) |
| Human Interface Guidelines | Apple | Platforms / Foundations / Patterns / Components / Inputs / Technologies | 3（Clarity / Deference / Depth） | なし（公開） | Figma UIキット / SDK API | プラットフォーム別に詳細化 / Liquid Glass (2025) |
| Polaris | Shopify | Foundations / Design / Content / Patterns / Components / Tokens / Icons | 6 Experience Values（Considerate / Empowering / Crafted / Efficient / Trustworthy / Familiar） | あり（Voice & Tone / Grammar / Error Messages / Alt Text / Inclusive Language） | npm `@shopify/polaris-tokens` / CSS / JSON | EC管理者向け特化 / 音（Sounds）セクションが独特 |
| Carbon Design System | IBM | All About Carbon / Guidelines / Elements / Components / Patterns / Data Visualization | 5（Open / Inclusive / Modular / User-First / Consistency） | あり（Content Guidelines） | npm `@carbon/colors` 等多数 / CSS / Sass | Data Visualization 独立 / ガバナンス審査済みパターン |
| Primer | GitHub | Product UI / Brand UI / Octicons / Accessibility / Primitives | 非公式（Accessibility First / Developer-centric） | なし（公開） | npm `@primer/primitives` / JSON | Product UI / Brand UI の2系統分離 / Octicons統合 |
| Fluent 2 | Microsoft | Design / Develop / Components / Design Tokens / Typography / Color / Accessibility | 非明示（Accessibility / Cross-platform Consistency / Creativity） | あり（Content Guidelines） | npm `@fluentui/tokens` / 2層トークン | グローバル+エイリアスの2層トークン / Teams・Outlook展開 |
| Spectrum | Adobe | Principles / Inclusive Design / Color / Typography / Motion / Design Tokens / Components / Content | 4（Inclusive / Approachable / Coherent / Perceivable） | あり（Copywriting / Voice & Tone / Grammar） | npm `@adobe/spectrum-tokens` / JSON | 100+製品統一 / Inclusive Design専用セクション / Spectrum 2 (2023〜) |
| Atlassian Design System | Atlassian | Foundations / Components / Patterns / Tools | 3（実践的原則） | あり（Content Guidelines） | npm `@atlaskit/tokens` / CSS Custom Properties | AI Design Patterns専用セクション / Illustrations・Logos等ビジュアルアイデンティティを含む |
| Lightning Design System | Salesforce | Design Tokens / Utilities / Component Blueprints / Guidelines / Patterns | 4（Clarity / Efficiency / Consistency / Beauty） | あり（Conversation Design / Voice & Tone） | CSS Custom Properties / Lightning API | SLDS 2でAI対応 / Conversation Design専用セクション |
| USWDS | 米国連邦政府 | How to Use / Design Principles / Components / Patterns / Design Tokens / Utilities / Templates | 5（User Needs / Trust / Accessibility / Continuity / Listen） | なし（公開） | npm `@uswds/uswds` / Sass / CSS | 政府向け特化 / Utilities・Templates が独自強み / Section 508対応 |
| Base | Uber | Foundations / Components / Patterns / Principles / Content Design | 非明示 | あり（Content Design） | 非公開 | 多様製品ライン統一フレームワーク / Content Design独立 |
| Pajamas | GitLab | Foundations / Components / Directives / Patterns / Objects / Data Visualization / Content | 10（GitLab独自） | あり（Content） | 非公開 | Directives・Objects という独自カテゴリ / Vue.js特化 / 最多原則数 |
| Garden | Zendesk | Design（Foundations） / Content / Components | 非明示（機能優先・アクセシビリティ必須） | 内部化（公開限定） | 非公開 | シンプルな3構造 / コンテンツガイドは内部化 |

---

## 出典

以下は本調査で参照した公式URL一覧（調査日: 2026-06-05）。

1. [Material Design 3 - Google's latest open source design system](https://m3.material.io/)
2. [Material Design 3 - Styles](https://m3.material.io/styles)
3. [Material Design 3 - Get Started](https://m3.material.io/get-started)
4. [Unveiling Material You - Material Design Blog](https://m3.material.io/blog/announcing-material-you)
5. [Human Interface Guidelines | Apple Developer Documentation](https://developer.apple.com/design/human-interface-guidelines/)
6. [Designing for iOS | Apple Developer Documentation](https://developer.apple.com/design/human-interface-guidelines/designing-for-ios)
7. [WWDC24: What's new in the Human Interface Guidelines](https://www.createwithswift.com/wwdc24-whats-new-in-the-human-interface-guidelines/)
8. [Shopify Polaris React](https://polaris-react.shopify.com/)
9. [Polaris - Design Section](https://polaris-react.shopify.com/design)
10. [Polaris - Content Section](https://polaris-react.shopify.com/content)
11. [Polaris - Fundamentals](https://polaris-react.shopify.com/content/fundamentals)
12. [Polaris - Grammar & Mechanics](https://polaris-react.shopify.com/content/grammar-and-mechanics)
13. [Polaris—unified and for the web (2025)](https://www.shopify.com/partners/blog/polaris-unified-and-for-the-web)
14. [Carbon Design System](https://carbondesignsystem.com/)
15. [Carbon Design System - What is Carbon?](https://v10.carbondesignsystem.com/all-about-carbon/what-is-carbon/)
16. [Carbon Design System - Content Guidelines](https://carbondesignsystem.com/guidelines/content/overview/)
17. [Primer.style](https://primer.style/)
18. [Primer Foundations](https://primer.github.io/design/foundations/)
19. [Primer Primitives - npm](https://www.npmjs.com/package/@primer/primitives)
20. [GitHub - primer/primitives](https://github.com/primer/primitives)
21. [Fluent 2 Design System](https://fluent2.microsoft.design/)
22. [Fluent 2 - Design Tokens](https://fluent2.microsoft.design/design-tokens)
23. [Fluent 2 - Typography](https://fluent2.microsoft.design/typography)
24. [Fluent 2 - Accessibility](https://fluent2.microsoft.design/accessibility)
25. [Fluent 2 - Color](https://fluent2.microsoft.design/color)
26. [Fluent UI Web Components design tokens | Microsoft Learn](https://learn.microsoft.com/en-us/fluent-ui/web-components/design-system/design-tokens)
27. [Spectrum, Adobe's design system](https://spectrum.adobe.com/)
28. [Spectrum 2](https://s2.spectrum.adobe.com/)
29. [Spectrum - Principles](https://spectrum.adobe.com/page/principles/)
30. [Spectrum - Design Tokens](https://spectrum.adobe.com/page/design-tokens/)
31. [Spectrum - Inclusive Design](https://spectrum.adobe.com/page/inclusive-design/)
32. [Spectrum - Color System](https://spectrum.adobe.com/page/color-system/)
33. [Spectrum - Motion](https://spectrum.adobe.com/page/motion/)
34. [GitHub - adobe/spectrum-design-data](https://github.com/adobe/spectrum-design-data)
35. [Introducing Spectrum 2 - Adobe Design](https://adobe.design/stories/design-for-scale/introducing-spectrum-2)
36. [Atlassian Design System](https://atlassian.design/)
37. [Atlassian Design - Foundations](https://atlassian.design/foundations)
38. [Atlassian Design - Design Tokens Overview](https://atlassian.design/foundations/tokens/design-tokens/)
39. [Atlassian Design - About](https://atlassian.design/get-started/about-atlassian-design-system)
40. [Atlassian Design - Typography](https://atlassian.design/foundations/typography/)
41. [@atlaskit/tokens - npm](https://www.npmjs.com/package/@atlaskit/tokens)
42. [Lightning Design System](https://www.lightningdesignsystem.com/)
43. [Lightning Design System - Guidelines Overview](https://spring-20.lightningdesignsystem.com/guidelines/overview/)
44. [Lightning Design System 2 - Patterns](https://www.lightningdesignsystem.com/2e1ef8501/p/355656-patterns)
45. [Salesforce SLDS - Standard Design Tokens](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/tokens_standard.htm)
46. [What is Salesforce Lightning Design System 2 (SLDS 2 Beta)?](https://www.salesforce.com/blog/what-is-slds-2/)
47. [U.S. Web Design System (USWDS)](https://designsystem.digital.gov/)
48. [USWDS - Design Principles](https://designsystem.digital.gov/design-principles/)
49. [USWDS - Design Tokens](https://designsystem.digital.gov/design-tokens/)
50. [@uswds/uswds - npm](https://www.npmjs.com/package/@uswds/uswds)
51. [Base design system - Uber](https://base.uber.com/)
52. [Base - Principles](https://base.uber.com/6d2425e9f/p/434f39-principles)
53. [Base - Patterns](https://base.uber.com/6d2425e9f/p/966802-patterns)
54. [Base - Content Design](https://base.uber.com/6d2425e9f/p/4245c4-content-design)
55. [Pajamas Design System - GitLab](https://design.gitlab.com/)
56. [Pajamas - Navigating Structure](https://design.gitlab.com/get-started/structure/)
57. [Pajamas - Principles](https://design.gitlab.com/get-started/principles/)
58. [Zendesk Garden](https://garden.zendesk.com/)
59. [Garden - Design Overview](https://garden.zendesk.com/design/)
60. [Garden - Content Overview](https://garden.zendesk.com/content/)
61. [Garden - Content Principles](https://garden.zendesk.com/content/principles/)
62. [Content Standards in Design Systems - NN/G](https://www.nngroup.com/articles/content-design-systems/)
63. [Best design system examples - Backlight.dev](https://backlight.dev/mastery/best-design-system-examples)
64. [Polaris tokens - GitHub](https://github.com/Shopify/polaris-tokens)

---

## このリポジトリへの示唆

本調査から、WhoOwnsDesign（AIを活用したWebアプリケーション構築における文書設計の定義）に向けて、以下の示唆が得られる。

### 1. 文書のトップ構造は「Foundations / Components / Patterns / Content / Resources」の5層が国際標準に近い

Material Design・Carbon・Polaris・Atlassian・USWDSは概ねこの構造に収束している。AIへの指示文書も「基礎定義層 → コンポーネント層 → パターン層 → コンテンツ/ライティング層」の順に整理すると伝わりやすい。

### 2. Foundations（基礎）には最低でも「色・タイポグラフィ・スペーシング・アイコン・アクセシビリティ」の5項目が必要

どのシステムも上記5項目を Foundations に含める。さらに上位システム（Material / Atlassian / Spectrum）は Motion / Elevation / Illustration / Data Visualization まで拡張している。AIに渡す基礎定義文書でもこの粒度が最低ラインと考えられる。

### 3. Content（ライティング）ガイドラインの有無がシステムの成熟度を示す

Polaris・Carbon・Spectrum・Atlassian・Salesforce・GitLab・Uberなど成熟したシステムは Content / Voice & Tone を独立セクションとして持つ。AIへの指示においてもトーン・エラーメッセージの書き方・インクルーシブ言語の規則を明示することが、アウトプット品質の一貫性に直結する。

### 4. デザイントークンはnpm + CSS Custom Properties + JSON の3形式が事実上の標準

主要10システム中7システムがnpmパッケージを公開し、CSS Custom Properties と JSON の2形式を並行提供している。AIへの文書でトークンを渡す場合も JSON または CSS 変数形式が最も汎用的。

### 5. 「原則の粒度」は3〜5個が大半で、10個（GitLab）は稀な例外

大半のシステムは3〜5つの原則に絞る（Material 3個、Apple 3個、Carbon 5個、USWDS 5個）。GitLabの10原則は開発者中心製品の文化を反映した特殊例。AIへの基本指示に含める原則も3〜5個程度に絞るとコンパクトで機能しやすい。

### 6. Accessibility はコンポーネントに内包するか独立セクションかで姿勢が分かれる

Carbon・Primer・USWDSは Accessibility を独立・横断的な原則として最重視し、Material・Atlassian・Spectrum はFoundations内に組み込む。AIへの文書ではコンポーネント定義とは別に「横断的アクセシビリティ要件」を独立したセクションで記述する方が見落とし防止になる。

### 7. パターン（Patterns）は「コンポーネントの組み合わせが生む体験」を定義し、AIの文脈では「ページテンプレートや画面フロー定義」に相当する

Polaris の Patterns、Carbon の Patterns（ガバナンス審査済み）、USWDS の Patterns（複雑ワークフロー向け）など、上位概念としてのパターン定義が存在する。AIへの指示文書でも「コンポーネント仕様」と「画面フロー・ページ構成パターン」を分離記述することが推奨される。
