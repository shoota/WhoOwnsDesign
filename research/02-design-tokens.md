---
title: "デザイントークン：階層・仕様・ツール・機械可読化"
topic: design-tokens
date: 2026-06-05
status: research-log
---

# デザイントークン：階層・仕様・ツール・機械可読化

> **目的:** WhoOwnsDesign リポジトリの文脈 — AIとチームが共同で消費できる「機械可読なデザインの真実の源（SSOT）」として、デザイントークンをどう定義・提供すべきかを明確にするための内部調査ログ。

---

## 1. デザイントークンの定義と歴史

### 1.1 定義

W3C Design Tokens Community Group (DTCG) の公式定義:

> "Design tokens are a methodology for expressing design decisions in a platform-agnostic way so that they can be shared across different disciplines, tools, and technologies."

重要なのは「**方法論（methodology）**」であるという点だ。Jina Anne はしばしば誤解される「変数（variables）」との違いを強調し、「変数と呼ぶのは、レスポンシブデザインをメディアクエリと呼ぶのと同じくらい不十分だ」と述べている。

### 1.2 歴史的経緯

| 年 | 出来事 |
|----|--------|
| **2014** | Salesforce でデザイナー **Jina Anne** と開発者 **Jon** が Lightning Design System 構築中に "design tokens" という用語を創出。プラットフォーム横断で設計決定を一箇所に保持する必要性から生まれた |
| **2016–2017** | Amazon の **Danny Banks** が **Style Dictionary** をオープンソース化。Salesforce 外のチームにも実用的な変換ツールを提供 |
| **2019** | W3C Design Tokens Community Group 設立。Adobe、Google、Microsoft、Meta、Figma、Salesforce、Shopify らが参加 |
| **2020** | **Nathan Curtis**（EightShapes）が "Naming Tokens in Design Systems" を発表（2020年10月）。コミュニティに体系的な語彙を提供（なお Curtis は 2016 年に "Tokens in Design Systems" という先行記事も執筆） |
| **2025年10月** | DTCG が初の安定仕様 **Design Tokens Format Module 2025.10** を公開。10以上のデザインツールが実装予定または対応済み |

### 1.3 Jina Anne の貢献

Salesforce の Lightning Design System にてトークンを実装し、"design tokens" という名称そのものを生み出した張本人。その後も W3C DTCG の共同編集者として仕様策定に貢献した。彼女の「methodology」という視点は、トークンを「単なる CSS 変数の別名」として矮小化しないための根幹をなす。

---

## 2. トークンの階層モデル

### 2.1 三層アーキテクチャ

デザイントークンは通常、以下の三層で管理される。

```
Primitive / Global tokens
         ↓ (参照)
Semantic / Alias tokens
         ↓ (参照)
Component tokens
```

#### 第一層: Primitive / Global トークン

- **役割:** 生の値（raw value）を保持するパレット。文脈を持たない。
- **命名:** 値を記述する（例: `blue-500`, `space-4`, `font-weight-bold`）
- **例:**
  ```json
  {
    "color": {
      "blue": {
        "500": { "$type": "color", "$value": "#3b82f6" }
      }
    },
    "space": {
      "4": { "$type": "dimension", "$value": { "value": 1, "unit": "rem" } }
    }
  }
  ```
- **原則:** まず primitive を安定させてから、意味（intent）をのせる。

#### 第二層: Semantic / Alias トークン

- **役割:** "なぜその値を使うか"（intent/purpose）を表現する。Primitive への参照（alias）として定義。
- **命名:** 用途を記述する（例: `color-text-primary`, `color-background-error`）
- **例:**
  ```json
  {
    "color-text-primary": {
      "$type": "color",
      "$value": "{color.blue.500}",
      "$description": "主要なテキストカラー。ブランドカラーに対応する"
    }
  }
  ```
- **原則:** 「何色か」ではなく「何のために使うか」を名前で表す。

#### 第三層: Component トークン

- **役割:** 特定コンポーネント固有の設計決定を保持。Semantic トークンへの参照。Primitive を直接参照してはならない（間接参照が柔軟性を保つ鍵）。
- **命名:** コンポーネント名を含む（例: `button-background-primary`, `input-border-focus`）
- **例:**
  ```json
  {
    "button": {
      "background": {
        "$type": "color",
        "$value": "{color-text-primary}",
        "$description": "プライマリボタンの背景色"
      }
    }
  }
  ```
- **原則:** 3つ以上のコンポーネントで共通の判断が生まれたら、Semantic 層へ昇格を検討する（"Start Within, Then Promote" - Nathan Curtis）。

### 2.2 なぜ分けるのか

| 理由 | 説明 |
|------|------|
| **変更の局所化** | Primitive の `blue-500` を変更すれば、それを参照するすべての Semantic・Component が自動更新される |
| **テーマ対応** | Semantic 層を差し替えるだけで Light/Dark や別ブランドに対応できる |
| **意図の保持** | 「なぜこの色か」という文脈が Semantic 層に記録される |
| **AIへの文脈提供** | 三層のJSON構造はLLMが設計の意図と値の実体を両方理解できる構造になっている |

---

## 3. トークンの種類

### 3.1 カラー（Color）

- **Primitive:** `color.blue.500 = #3b82f6`（色名＋スケール）
- **Semantic:** `color.text.primary`, `color.background.error`
- **設計観点:** DTCG 2025.10 は Display P3、Oklch、CSS Color Module 4 を正式サポート。sRGB だけでなくワイドガマットを考慮した設計が必要。
- **テーマ対応:** Light/Dark で Semantic 層の値を切り替える。

### 3.2 タイポグラフィ（Typography）

- **構成要素:** font-family、font-size、font-weight、line-height、letter-spacing
- **Composite トークン例（DTCG）:**
  ```json
  {
    "heading-1": {
      "$type": "typography",
      "$value": {
        "fontFamily": "{font.family.sans}",
        "fontSize": { "value": 2, "unit": "rem" },
        "fontWeight": 700,
        "lineHeight": 1.2
      }
    }
  }
  ```
- **設計観点:** 流体タイポグラフィ（`clamp()`）との組み合わせも検討。

### 3.3 スペーシング（Spacing）

- **Primitive:** `space.1 = 0.25rem`, `space.4 = 1rem`（4px ベースのスケール）
- **Semantic:** `spacing.inline.md`（水平方向）, `spacing.stack.lg`（垂直方向）
- **設計観点:** 8pt グリッドや 4px ベース乗算系が多い。「spacing と sizing を分けるか統合するか」はチーム方針による。

### 3.4 サイジング（Sizing）

- **用途:** 幅・高さ・最小/最大サイズなどの固定寸法
- **例:** `size.icon.sm = 1rem`, `size.avatar.md = 2.5rem`
- **設計観点:** スペーシングと分けることで「余白」と「要素サイズ」の意図が明確になる。

### 3.5 ボーダー・角丸（Border / Border-Radius）

- **Primitive:** `border-radius.sm = 4px`, `border-radius.full = 9999px`
- **Semantic:** `border-radius.button`, `border-radius.card`
- **Composite Border（DTCG）:**
  ```json
  {
    "border-default": {
      "$type": "border",
      "$value": {
        "color": "{color.border.default}",
        "width": { "value": 1, "unit": "px" },
        "style": "solid"
      }
    }
  }
  ```

### 3.6 エレベーション・シャドウ（Elevation / Shadow）

- **DTCG Shadow 型:**
  ```json
  {
    "shadow.md": {
      "$type": "shadow",
      "$value": {
        "color": "rgba(0,0,0,0.15)",
        "offsetX": { "value": 0, "unit": "px" },
        "offsetY": { "value": 4, "unit": "px" },
        "blur": { "value": 6, "unit": "px" },
        "spread": { "value": -1, "unit": "px" }
      }
    }
  }
  ```
- **設計観点:** Material Design 3 は tonal color（色付きの影）でエレベーションを表現するアプローチも採用。

### 3.7 Z-Index

- **Primitive:** `z-index.base = 0`, `z-index.overlay = 100`, `z-index.modal = 1000`, `z-index.toast = 2000`
- **設計観点:** 数値の衝突を防ぐため、スケールを事前に定義して階層を管理。

### 3.8 モーション・アニメーション（Motion / Duration / Easing）

- **Duration 例:** `duration.fast = 100ms`, `duration.normal = 200ms`, `duration.slow = 400ms`
- **Easing（Cubic Bézier）:**
  ```json
  {
    "easing.ease-out": {
      "$type": "cubicBezier",
      "$value": [0.0, 0.0, 0.2, 1.0]
    }
  }
  ```
- **Transition Composite:**
  ```json
  {
    "transition.default": {
      "$type": "transition",
      "$value": {
        "duration": "{duration.normal}",
        "delay": { "value": 0, "unit": "ms" },
        "timingFunction": "{easing.ease-out}"
      }
    }
  }
  ```
- **設計観点:** `prefers-reduced-motion` への配慮として、モーショントークンに `reduced` バリアントを用意する。

### 3.9 ブレークポイント（Breakpoints）

- **例:** `breakpoint.sm = 640px`, `breakpoint.md = 768px`, `breakpoint.lg = 1024px`
- **注意:** DTCG の型としては `dimension` で表現。CSS では媒体クエリ内でのみ使用されるため、CSS 変数として直接は利用できない（値を JS や SCSS の変数として展開）。

### 3.10 オパシティ（Opacity）

- **例:** `opacity.disabled = 0.38`, `opacity.hover = 0.08`, `opacity.loading = 0.6`
- **型:** DTCG では `number`（0〜1）

---

## 4. 命名規則（Naming Convention）

### 4.1 Nathan Curtis の命名フレームワーク

Nathan Curtis（EightShapes）は以下の命名レベルを提唱している。

**Base Levels（基幹）**

| レベル | 説明 | 例 |
|--------|------|-----|
| Category | 視覚プロパティ種別 | `color`, `font`, `space`, `elevation` |
| Property | 関連プロパティ | `text`, `background`, `border` |
| Concept | カテゴリ内の概念グループ | `feedback`, `action`, `heading` |

**Modifier Levels（修飾）**

| レベル | 説明 | 例 |
|--------|------|-----|
| Variant | 代替ユースケース | `primary`, `secondary`, `error` |
| State | インタラクション状態 | `hover`, `active`, `focus`, `disabled` |
| Scale | 定量的段階 | `1`, `2`, `3`, `sm`, `md`, `lg` |
| Mode | 表面コンテキスト | `on-light`, `on-dark`, `on-brand` |

**典型的な順序:**
```
[namespace]-[category]-[concept]-[property]-[variant]-[scale]-[state]-[mode]
```

例:
```
$esds-color-feedback-error = #B90000
$esds-color-action-text-primary-hover
$esds-input-left-icon-size
```

### 4.2 各社の流儀比較

| 企業/DS | 命名パターン | 例 |
|---------|------------|-----|
| **Salesforce (Lightning)** | `$color-text-action-active` | Semantic 優先 |
| **Adobe (Spectrum)** | `--spectrum-global-color-blue-500` | System prefix + Global/Alias 明示 |
| **Atlassian** | `--ds-text-selected` | `ds` prefix + 意図 |
| **Material Design 3** | `md.sys.color.primary` | System / Reference / Component の階層 |
| **GitHub (Primer)** | `--fgColor-default` | property + intent のフラット命名 |

### 4.3 ベストプラクティスまとめ

- **値ではなく用途を名前にする:** `blue-button` ではなく `button-primary-background`
- **kebab-case を使う:** CSS 変数との親和性が高い
- **ドット記法（JSON）とハイフン記法（CSS）を統一的に扱う:** Style Dictionary などのツールが変換を担う
- **Homonyms を避ける:** `type`（型 or タイポグラフィ？）や `text`（テキストカラー or テキスト要素？）などの曖昧語は禁忌
- **スケールの定義を先に固める:** `sm/md/lg` か `1/2/3/4/5` か数値系か、ブレぬ基準を先決
- **チーム合意が最優先:** 「正しい命名」より「全員が一貫して使える命名」が本質

---

## 5. テーマ・モード対応

### 5.1 多次元化の概念

デザイントークンは複数の「次元」でバリアントを持てる。

| 次元 | 例 |
|------|-----|
| カラースキーム | `light` / `dark` |
| ブランド | `brand-a` / `brand-b` |
| アクセシビリティ | `high-contrast` / `increased-contrast` |
| 密度 | `compact` / `comfortable` / `spacious` |

### 5.2 CSS での実装パターン

```css
/* Layer 1: Global Primitives (変わらない) */
:root {
  --color-blue-600: #0052CC;
  --color-grey-900: #1a1a1a;
}

/* Layer 2: Semantic - Light (デフォルト) */
:root {
  --color-text-primary: var(--color-grey-900);
  --color-background-page: #ffffff;
}

/* Layer 2: Semantic - Dark (data属性で切替) */
[data-theme="dark"] {
  --color-text-primary: #f5f5f5;
  --color-background-page: #1a1a1a;
}

/* Layer 2: システム設定を自動反映 */
@media (prefers-color-scheme: dark) {
  :root {
    --color-text-primary: #f5f5f5;
    --color-background-page: #1a1a1a;
  }
}

/* Layer 3: Component (テーマ切替の影響を自動受け取る) */
.button-primary {
  background: var(--color-text-primary);
}
```

### 5.3 DTCG 2025.10 のテーマサポート

DTCG 2025.10 仕様はテーマ管理を正式にサポート。ファイル複製なしで Light/Dark・アクセシビリティ変種・マルチブランドテーマを管理できる仕組みを提供。Tokens Studio の「Token Sets」機能はこれを no-code で実現する先行実装。

---

## 6. W3C Design Tokens Community Group (DTCG) 仕様

### 6.1 概要

- **正式名称:** Design Tokens Format Module
- **最新バージョン:** 2025.10（2025年10月28日 - 初の安定版）
- **URL:** https://www.designtokens.org/tr/drafts/format/
- **メディアタイプ:** `application/design-tokens+json`
- **推奨拡張子:** `.tokens` または `.tokens.json`
- **参加企業:** Adobe、Amazon、Google、Microsoft、Meta、Figma、Salesforce、Shopify、Sketch、Penpot、Baidu、Sony、Disney など20社以上

### 6.2 JSON 基本構造

```json
{
  "color": {
    "$type": "color",
    "brand": {
      "primary": {
        "$value": "#0052CC",
        "$type": "color",
        "$description": "ブランドプライマリカラー。CTAボタンやリンクに使用",
        "$deprecated": false
      }
    }
  }
}
```

**必須プロパティ:**

| プロパティ | 役割 |
|-----------|------|
| `$value` | トークンの実際の値（必須） |
| `$type` | トークンの型（グループから継承も可） |

**オプションプロパティ:**

| プロパティ | 役割 |
|-----------|------|
| `$description` | 用途説明（平文テキスト） |
| `$extensions` | ベンダー固有メタデータ（逆ドメイン記法） |
| `$deprecated` | 廃止フラグ（boolean または廃止理由の文字列） |

### 6.3 サポートする型（Token Types）

**Atomic Types（原子型）:**

| 型 | 説明 | 値の例 |
|----|------|--------|
| `color` | RGB/HSL カラー | `"#3b82f6"` |
| `dimension` | 距離・サイズ | `{ "value": 1, "unit": "rem" }` |
| `fontFamily` | フォントファミリー | `"Inter"` または `["Inter", "sans-serif"]` |
| `fontWeight` | フォントウェイト | `700` または `"bold"` |
| `duration` | 時間 | `{ "value": 200, "unit": "ms" }` |
| `cubicBezier` | アニメーション曲線 | `[0.4, 0.0, 0.2, 1.0]` |
| `number` | 無単位数値 | `1.5` |

**Composite Types（複合型）:**

| 型 | 構成要素 |
|----|----------|
| `border` | `color` + `width`(dimension) + `style` |
| `shadow` | `color` + `offsetX/Y` + `blur` + `spread` |
| `strokeStyle` | `solid`, `dashed` などまたはカスタムパターン |
| `transition` | `duration` + `delay` + `timingFunction` |
| `gradient` | カラーストップとポジション |
| `typography` | `fontFamily` + `fontSize` + `fontWeight` + `lineHeight` |

### 6.4 参照（Alias）記法

```json
{
  "button-background": {
    "$type": "color",
    "$value": "{color.brand.primary}"
  }
}
```

- **波括弧 `{group.token}`** でトークン間の参照を表現
- 参照はツールが解決時に `$value` を自動展開
- 循環参照は仕様で明示的にエラー扱い

### 6.5 グループとタイプ継承

```json
{
  "color": {
    "$type": "color",
    "blue": {
      "400": { "$value": "#60a5fa" },
      "500": { "$value": "#3b82f6" },
      "600": { "$value": "#2563eb" }
    }
  }
}
```

グループに `$type` を指定すると、子トークンに型が継承される（個別指定が優先）。

### 6.6 `$extends` によるグループ拡張

```json
{
  "button-primary": {
    "$extends": "{button}",
    "background": { "$value": "#cc0066" }
  }
}
```

ディープマージセマンティクスで、ローカルプロパティが継承元を上書き。

---

## 7. Style Dictionary（Amazon）

### 7.1 概要

- **作者:** Danny Banks（Amazon）
- **初公開:** 2017年
- **ライセンス:** Apache 2.0
- **GitHub:** https://github.com/amzn/style-dictionary
- **公式ドキュメント:** https://styledictionary.com/

単一の JSON トークン定義から、CSS・SCSS・JavaScript・iOS（Swift/ObjC）・Android（XML）など複数プラットフォーム向けのファイルを生成するビルドシステム。

### 7.2 アーキテクチャ

```
[入力: JSON/YAML トークンファイル]
         ↓
    [Parsers]        ← カスタムパーサーも登録可
         ↓
  [Preprocessors]   ← データ変換前処理
         ↓
   [Transforms]     ← 値の変換（hex→rgba, px→pt など）
         ↓
    [Formats]       ← 出力形式の定義
         ↓
[出力: CSS変数, SCSS, JS, iOS, Android ...]
```

### 7.3 設定ファイル（config.json）の例

```json
{
  "source": ["tokens/**/*.json"],
  "platforms": {
    "css": {
      "transformGroup": "css",
      "prefix": "sd",
      "buildPath": "build/css/",
      "files": [
        {
          "destination": "_variables.css",
          "format": "css/variables",
          "options": {
            "outputReferences": true
          }
        }
      ]
    },
    "javascript": {
      "transformGroup": "js",
      "buildPath": "build/js/",
      "files": [
        {
          "destination": "tokens.js",
          "format": "javascript/es6"
        }
      ]
    },
    "ios": {
      "transformGroup": "ios",
      "buildPath": "build/ios/",
      "files": [
        {
          "destination": "StyleDictionaryColor.h",
          "format": "ios/colors.h"
        }
      ]
    }
  }
}
```

### 7.4 主要な組み込み Transforms

| Transform | 変換内容 |
|-----------|----------|
| `attribute/cti` | カテゴリ/タイプ/アイテム階層をattributeに付与 |
| `name/snake` | スネークケースに変換 |
| `color/hex` | カラーを HEX に変換 |
| `size/remToSp` | rem を Android sp に変換 |
| `name/camel` | キャメルケースに変換 |

### 7.5 主要な組み込み Formats

| Format | 出力形式 |
|--------|----------|
| `css/variables` | CSS カスタムプロパティ（`:root { --token: value; }`） |
| `scss/variables` | SCSS 変数（`$token: value;`） |
| `javascript/es6` | ES6 named exports |
| `ios/colors.h` | iOS Objective-C カラー定義 |
| `android/colors` | Android XML リソース |

### 7.6 DTCG サポート状況

- **v4:** DTCG フォーマットのファーストクラスサポートを追加
- **v5（リリース済み）:** DTCG 2025.10 を基本フォーマットとして採用し、安定版としてリリース済み（npm 最新版 5.4.x）
- **移行ツール:** v3（旧形式）から DTCG 形式への変換ツールを提供。`value` → `$value`、`type` → `$type`、`description` → `$description` への自動変換。

### 7.7 利点

- **プラットフォーム非依存:** 一度定義すれば CSS・iOS・Android・JS に同時出力
- **拡張性:** カスタム Transform・Format・Action の追加が容易
- **バッチビルド:** CI/CD パイプラインへの組み込みが容易
- **DTCG 準拠:** 業界標準フォーマットとの互換性

---

## 8. Tokens Studio（旧: Figma Tokens）

### 8.1 概要

- **公式:** https://tokens.studio/
- **Figma プラグイン:** https://www.figma.com/community/plugin/843461159747178978/
- **特徴:** デザイナーが Figma 上でノーコードでトークンを管理し、コードリポジトリと同期させるためのツール

### 8.2 主な機能

**Token Sets（JSON ファイルの no-code 版）**
- Figma の Collection/Mode を JSON のトークンセットとしてマッピング
- 階層・グループ・型を GUI で操作

**サポートするトークン型**
- Color（グラデーション・Modified Color含む）
- Typography（Composite）
- Dimension（spacing, sizing, border-radius, border-width）
- Box Shadow、Border（Composite）
- Asset、Boolean、Number、Opacity、Text

**リモート同期（Sync Providers）**

| プロバイダー | 特徴 |
|-------------|------|
| GitHub | ブランチ管理・PR連携（Pro機能） |
| GitLab | 同上 |
| Bitbucket | 同上 |
| Azure DevOps | エンタープライズ向け |
| JSONBin | シンプルなクラウドストレージ |
| Supernova | 包括的なDS管理ツールとの統合 |

### 8.3 DTCG 対応

W3C DTCG フォーマット（`$value`/`$type`/`$description`）に準拠したエクスポートをサポート。Style Dictionary との連携で JSON → 各プラットフォーム変換のパイプラインを形成。

### 8.4 ワークフロー

```
Figma で変数・スタイル定義
         ↓
Tokens Studio Plugin でトークン化・管理
         ↓
GitHub/GitLab に Push（JSON ファイル）
         ↓
GitHub Actions で Style Dictionary ビルド
         ↓
CSS変数・JS・iOS・Android へ展開
```

---

## 9. CSS Custom Properties（CSS 変数）との関係

### 9.1 トークンと CSS 変数の違い

| 概念 | 範囲 | 形式 |
|------|------|------|
| **デザイントークン** | プラットフォーム非依存 | JSON（DTCG 形式） |
| **CSS Custom Properties** | Web ブラウザのみ | `--variable-name: value` |

CSS 変数はデザイントークンの **Web プラットフォームにおける実装形態**。Style Dictionary などのツールが JSON → CSS 変換を担う。

### 9.2 三層を CSS で表現する完全な例

```css
/* ===== Layer 1: Primitive Tokens ===== */
:root {
  /* Color primitives */
  --color-blue-500: #3b82f6;
  --color-blue-600: #2563eb;
  --color-grey-50:  #f9fafb;
  --color-grey-900: #111827;

  /* Spacing primitives */
  --space-1: 0.25rem;   /* 4px */
  --space-2: 0.5rem;    /* 8px */
  --space-4: 1rem;      /* 16px */
  --space-8: 2rem;      /* 32px */

  /* Border-radius primitives */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-full: 9999px;
}

/* ===== Layer 2: Semantic Tokens (Light) ===== */
:root {
  --color-text-primary:       var(--color-grey-900);
  --color-text-interactive:   var(--color-blue-600);
  --color-background-page:    var(--color-grey-50);
  --color-background-action:  var(--color-blue-500);
}

/* ===== Layer 2: Semantic Tokens (Dark) ===== */
[data-theme="dark"],
@media (prefers-color-scheme: dark) {
  :root {
    --color-text-primary:      #f5f5f5;
    --color-text-interactive:  var(--color-blue-500);
    --color-background-page:   #1a1a1a;
    --color-background-action: var(--color-blue-600);
  }
}

/* ===== Layer 3: Component Tokens ===== */
.button-primary {
  --button-bg:     var(--color-background-action);
  --button-text:   #ffffff;
  --button-radius: var(--radius-md);
  --button-px:     var(--space-4);
  --button-py:     var(--space-2);

  background-color: var(--button-bg);
  color:            var(--button-text);
  border-radius:    var(--button-radius);
  padding:          var(--button-py) var(--button-px);
}
```

### 9.3 CSS 変数のランタイムの強み

- **JavaScript からのアクセス:** `getComputedStyle(el).getPropertyValue('--color-primary')`
- **動的な切り替え:** `el.style.setProperty('--color-primary', '#ff0000')`
- **コンポーネントスコープ:** `:root` ではなくコンポーネント要素にスコープ可能
- **カスケードの活用:** `inherit` の伝播を利用した親→子への設定引き継ぎ

---

## 10. SSOT 戦略：デザイン（Figma）と実装の同期

### 10.1 SSOT をどこに置くか

**Option A: コードを SSOT とする（推奨アプローチ）**

```
JSON（Git管理）→ Figma に変数として読み込む
                → Style Dictionary で CSS/iOS/Android へ
```

- 変更は必ず Git コミットを起点とし、PR でレビュー
- デザイナーはブランチで探索し、確定したら Git にマージ
- 将来的なツール変更に強い（Figma に依存しない）

**Option B: Figma を SSOT とする**

```
Figma Variables → Tokens Studio Plugin → Git Push → Style Dictionary
```

- デザイナー主導のワークフロー
- 変更が Figma → PR → マージ → 同期という形でトレーサブル
- Tokens Studio の GitHub Sync + Actions で自動化可能

### 10.2 推奨パイプライン（Option A + B ハイブリッド）

```
[Figma Branch] ──(探索・実験)──→ 確定後
      ↓
[Tokens Studio Push → GitHub PR]
      ↓
[PR Review] ←── デザイナー + エンジニア合同レビュー
      ↓
[Merge to main → GitHub Actions]
      ↓
[Style Dictionary Build]
      ↓
[CSS / JS / iOS / Android 生成]
      ↓
[Figma main file に自動 pull-back（任意）]
```

### 10.3 二重管理の回避戦略

| 問題 | 解決策 |
|------|--------|
| Figma と Git の値がずれる | Tokens Studio の自動同期 + CI での差分チェック |
| リネームによる暗黙の破壊 | コンポーネント名変更を cross-env 操作として扱う |
| ツールごとの形式の違い | DTCG 統一フォーマットを採用 |
| デザイナーが Git を使えない | Tokens Studio の GUI が Git を抽象化 |

---

## 11. バージョニング・破壊的変更の扱い

### 11.1 セマンティックバージョニング（SemVer）の適用

デザイントークンにも SemVer（MAJOR.MINOR.PATCH）が適用できる。

| バージョン種別 | 該当するトークン変更 |
|--------------|-------------------|
| **MAJOR（破壊的）** | トークン名の変更・削除、型の変更 |
| **MINOR（後方互換）** | 新規トークンの追加、`$deprecated` マークの追加 |
| **PATCH（バグ修正）** | 値の誤り修正（コントラスト比修正など）、`$description` 更新 |

### 11.2 破壊的変更の定義

以下はトークンにとっての破壊的変更：
- トークン名の変更（`color-primary` → `color-brand-primary`）
- トークンの削除
- 型の変更（`dimension` → `number`）
- 値の大幅な変更（ブランドカラーの刷新など）

### 11.3 廃止（Deprecation）ワークフロー

```json
{
  "color-primary": {
    "$value": "{color.brand.blue.600}",
    "$type": "color",
    "$deprecated": "代わりに color-brand-primary を使用してください。v3.0.0 で削除予定"
  }
}
```

1. `$deprecated` フラグでトークンを廃止予告（最低30〜90日前）
2. 新旧両方のトークンを並行運用
3. コンシューマーへの通知（changelog、Slack、リリースノート）
4. 移行ガイドの提供（codemods など）
5. メジャーバージョンアップで旧トークン削除

### 11.4 自動化ツール

- **semantic-release / Changesets:** conventional commits からバージョンを自動決定
- **codemod:** トークン名変更の機械的な一括置換
- **CI での lint:** 廃止済みトークンの使用を CI で検出・警告

---

## 12. AIとLLMがトークンを消費するうえでの機械可読フォーマットの優位性

### 12.1 なぜ JSON/DTCG 形式が AI に有利か

| 特性 | 理由 |
|------|------|
| **構造化データ** | LLM はフラットなテキストより階層的な key-value 構造を精度高く解釈できる |
| **意味の明示** | `$type: "color"` や `$description` により値の文脈が明示される |
| **参照の追跡** | alias 構造（`{color.brand.primary}`）により依存関係がグラフとして表現され、LLM が設計の意図を理解できる |
| **スキーマ検証** | JSON Schema で型を強制でき、LLM の出力バリデーションにも利用できる |
| **量の削減** | 3,000 色のハードコード値より 30 のトークン参照の方がコンテキスト消費量が少ない |

### 12.2 LLM によるデザイントークンの活用パターン

**パターン 1: トークン駆動コード生成**
```
入力: tokens.json + コンポーネント仕様
出力: React コンポーネント（CSS 変数を正しく使用）
```
トークンを JSON として LLM に与えると、`var(--color-text-primary)` のようなハードコードではなくトークン参照を含むコードを生成できる。

**パターン 2: ドリフト検出（Audit）**
```
入力: tokens.json + 既存コードベース
出力: トークンを使用すべき箇所のレポート、修正案
```
Hardik Pandya（Atlassian）の事例では、Atlaskit（Atlassian のパブリック DS）を使ったテストプロジェクトで 64 spec ファイルと 230+ トークンを整備し、418 件のハードコード値をゼロにした結果を報告している（個人実験の報告であり、Atlassian 公式の定量的精度データではない）。

**パターン 3: Figma MCP 連携**
Figma の MCP サーバーがデザイン変数・トークン情報を IDE の AI エージェントに直接提供。LLM はデザイン情報を参照しながらデザイン準拠のコードを生成（Code Connect）。

**パターン 4: 設計決定の文書化**
`$description` フィールドに記述された意図がそのまま LLM のコンテキストとなる。「なぜ `blue-600` ではなく `blue-700` か」という設計根拠を AI が引用できる。

### 12.3 AI ネイティブなデザインシステムの三本柱（2025年現在）

1. **Closed Token Layer（閉じたトークン層）**: コンポーネントは生の値ではなくトークン変数のみを参照。LLM が新しいコンポーネントを生成するとき、適切なトークンを選択せざるを得ない環境を作る。

2. **Spec Files（仕様ファイル）**: トークンの一覧を含む構造化されたドキュメントをリポジトリ内に配置（外部ドキュメントではなくコードと同居）。LLM のシステムプロンプトやコンテキストとして利用。

3. **Audit Scripts（監査スクリプト）**: CI で動作し、ハードコード値の混入を検出するスクリプト。LLM が生成したコードの品質を自動検証。

### 12.4 AI 消費を前提としたトークン設計の留意点

- **`$description` を充実させる:** AI が「なぜこのトークンを使うべきか」を判断できる説明を必ず記述
- **alias 構造を保つ:** フラット化せず三層構造を維持することで、AI が設計の文脈を追跡できる
- **型を明示する:** `$type` は AI の誤用を防ぐガードレールになる
- **廃止情報を残す:** `$deprecated` により AI が古いトークンを誤って推薦することを防ぐ
- **命名を意味的にする:** `blue-500` より `color-brand-primary` の方が AI の推論精度が高い

---

## 13. 主要ツール・エコシステムのまとめ

| ツール | 役割 | 特徴 |
|--------|------|------|
| **Tokens Studio** | Figma ↔ JSON 同期 | DTCG 対応、Git 連携、No-code |
| **Style Dictionary** | JSON → 各プラットフォーム変換 | Amazon OSS、高拡張性 |
| **Supernova** | DS 管理・ドキュメント | Figma連携、企業向け |
| **Specify** | トークン配信・管理 | APIファースト |
| **Cobalt UI** | DTCG 対応のモダンな変換ツール | 軽量・高速 |
| **Terrazzo** | DTCG リファレンス実装 | 仕様準拠優先 |
| **Penpot** | OSS デザインツール | ネイティブ DTCG サポート |
| **zeroheight** | トークン文書化 | Storybook連携 |

---

## 出典

1. [Design Tokens Community Group – 公式サイト](https://www.designtokens.org/)
2. [Design Tokens Format Module 2025.10 – W3C DTCG 仕様](https://www.designtokens.org/tr/drafts/format/)
3. [Design Tokens Specification Reaches First Stable Version – W3C DTCG ブログ](https://www.w3.org/community/design-tokens/2025/10/28/design-tokens-specification-reaches-first-stable-version/)
4. [The incomplete history of Design Tokens – David Lewis (Design Systems Collective)](https://www.designsystemscollective.com/the-incomplete-history-of-design-tokens-61581c573e5d)
5. [WTF are Design Tokens? – Amy Lee (Medium/Jina Anne 解説)](https://medium.com/@amster/wtf-are-design-tokens-9706d5c99379)
6. [Smashing Podcast Episode 3 With Jina Anne: What Are Design Tokens?](https://www.smashingmagazine.com/2019/11/smashing-podcast-episode-3/)
7. [Naming Tokens in Design Systems – Nathan Curtis (EightShapes/Medium)](https://medium.com/eightshapes-llc/naming-tokens-in-design-systems-9e86c7444676)
8. [Design Token Naming Conventions: A Practical Guide – Always Twisted](https://www.alwaystwisted.com/articles/design-token-naming-conventions)
9. [How To Name Design Tokens in Design Systems – Smart Interface Design Patterns](https://smart-interface-design-patterns.com/articles/naming-design-tokens/)
10. [Crafting consistency: a thoughtful approach for naming design tokens – Specify](https://specifyapp.com/blog/crafting-consistency-a-thoughtful-approach-for-naming-design-tokens)
11. [Design Tokens Community Group – Style Dictionary 解説](https://styledictionary.com/info/dtcg/)
12. [Style Dictionary 設定リファレンス – Configuration](https://styledictionary.com/reference/config/)
13. [Style Dictionary GitHub Repository (amzn/style-dictionary)](https://github.com/amzn/style-dictionary)
14. [Style Dictionary: An Open Source Tool for Building Customer Trust – AWS Open Source Blog](https://aws.amazon.com/blogs/opensource/style-dictionary-trust-design-consistency/)
15. [Intro to Design Tokens – Tokens Studio for Figma 公式ドキュメント](https://docs.tokens.studio/fundamentals/design-tokens/)
16. [Token Format - W3C DTCG vs Legacy – Tokens Studio](https://docs.tokens.studio/manage-settings/token-format)
17. [Tokens Studio for Figma – Figma Community プラグインページ](https://www.figma.com/community/plugin/843461159747178978/tokens-studio-for-figma)
18. [The developer's guide to design tokens and CSS variables – Penpot Blog](https://penpot.app/blog/the-developers-guide-to-design-tokens-and-css-variables/)
19. [Dark Mode Design Systems: A Complete Guide – Muzli Blog](https://muz.li/blog/dark-mode-design-systems-a-complete-guide-to-patterns-tokens-and-hierarchy/)
20. [Design Token SSOT: Figma or Code? The Governance Question and AI's Role (2026)](https://www.designsystemscollective.com/design-token-ssot-figma-or-code-the-governance-question-and-ais-role-6dc6dfda533c)
21. [How to Maintain a Single Source of Truth Between Figma and Code – Replay Blog](https://www.replay.build/blog/how-to-maintain-a-single-source-of-truth-between-figma-and-code)
22. [Design tokens architecture – Juan David Posada (Medium)](https://medium.com/@jdposada/design-tokens-architecture-7544c9a8f33a)
23. [Design tokens beyond colors, typography, and spacing – Cristiano Rastelli (Bumble Tech)](https://medium.com/bumble-tech/design-tokens-beyond-colors-typography-and-spacing-ad7c98f4f228)
24. [Design Tokens – Atlassian Design](https://atlassian.design/foundations/tokens/design-tokens/)
25. [Design Tokens – Adobe Spectrum](https://spectrum.adobe.com/page/design-tokens/)
26. [Design tokens – Fluent 2 Design System (Microsoft)](https://fluent2.microsoft.design/design-tokens)
27. [Design tokens – Material Design 3 (Google)](https://m3.material.io/foundations/design-tokens)
28. [Versioning Design Tokens – Francesco Improta (Substack)](https://designtokens.substack.com/p/versioning-design-tokens)
29. [Versioning and Breaking Changes – Morningstar Design System](https://designsystem.morningstar.com/getting-started/versioning-and-breaking-changes/)
30. [Design Systems And AI: Why MCP Servers Are The Unlock – Figma Blog](https://www.figma.com/blog/design-systems-ai-mcp/)
31. [Expose your design system to LLMs – Hardik Pandya](https://hvpandya.com/llm-design-systems)
32. [Design Systems + AI: Generating Token-Driven Code Automatically – Medium](https://medium.com/@Rythmuxdesigner/design-systems-ai-generating-token-driven-code-automatically-68f353754c0e)
33. [The Evolution of Design System Tokens: A 2025 Deep Dive – Design Systems Collective](https://www.designsystemscollective.com/the-evolution-of-design-system-tokens-a-2025-deep-dive-into-next-generation-figma-structures-969be68adfbe)
34. [From Tokens to Thinking Systems: Making AI-Native Design Systems Actually Work – Medium](https://medium.com/digitaltableteur/from-tokens-to-thinking-systems-making-ai-native-design-systems-actually-work-46a51931e8e0)
35. [Design Tokens: How to Sync Design and Code in Figma – Figma Resource Library](https://www.figma.com/resource-library/design-tokens/)
36. [design-tokens/community-group – GitHub リポジトリ](https://github.com/design-tokens/community-group)

---

## このリポジトリへの示唆

WhoOwnsDesign がデザイントークンを定義・提供するうえで、以下の点を強く推奨する。

### 1. DTCG 2025.10 を唯一の定義フォーマットとして採用する
W3C DTCG 2025.10 は初の安定仕様であり、Figma・Tokens Studio・Style Dictionary・Penpot が一斉に対応を進めている。今後の標準として`.tokens.json`（`$value`/`$type`/`$description` 形式）を WhoOwnsDesign の公式フォーマットとして定義することで、ツール変更に強く、AI が解釈しやすい構造を担保できる。

### 2. 三層構造（Primitive → Semantic → Component）を必須の設計原則として定義する
「すべてのコンポーネントは Primitive 値を直接参照してはならない」という制約を明文化する。この間接参照がテーマ対応・AI の文脈理解・変更の局所化の三つを同時に実現する根幹となる。具体的な命名規則（カテゴリ・コンセプト・プロパティ・バリアント・ステート）のリファレンスもリポジトリ内に配置すること。

### 3. AI 消費を前提に `$description` を必須フィールドとして扱う
`$description` には「なぜこのトークンが存在するか」「どんな状況で使うか」「何を参照しているか」を必ず記述する規約を設ける。LLM はこの説明を使って適切なトークンを選択する。空の `$description` は lint エラーとするくらいの厳しさが推奨。

### 4. Git を SSOT とするパイプラインを公式ワークフローとして定義する
Figma の探索フェーズは自由に行ってよいが、「正式なトークン変更は必ず Git コミット/PR を起点とする」というルールを定める。Tokens Studio + GitHub Actions + Style Dictionary の組み合わせを推奨パイプラインとして文書化し、CI でトークンのビルドと lint を自動実行する構成を示す。

### 5. セマンティックバージョニングと廃止フローを定義する
トークン名の変更・削除は MAJOR バージョンアップ、新規追加は MINOR、値修正は PATCH として扱うルールを明記する。`$deprecated` フィールドの使い方・廃止予告期間（最低30日）・移行ガイドの提供を義務として定義することで、AIが古いトークンを誤って使い続けることも防止できる。

### 6. AI 向け「Closed Token Layer + Spec Files」を成果物に含める
WhoOwnsDesign の成果物として、トークン定義 JSON に加えて「AIが読むための spec ファイル」の形式を定義する。具体的には: (a) トークン一覧を含む構造化 Markdown、(b) コンポーネントごとの使用ガイドライン、(c) CI で動作するハードコード値検出スクリプト。これによりAIがコードを生成するたびにトークンを一貫して参照できる環境が構築される。

### 7. トークンの種類と型の完全リストをリポジトリ内に定義する
Color・Typography・Spacing・Sizing・Border-Radius・Elevation/Shadow・Z-Index・Motion（Duration/Easing）・Breakpoints・Opacity の全カテゴリについて、DTCG 型との対応・命名パターン・CSS 変数への展開例を含むリファレンスを用意する。これが「AIやエンジニアが何を・どの粒度で・どの形式で提供すべきか」の実装ガイドとなる。
