---
title: "Webプラットフォーム固有のシステム制約"
topic: "アクセシビリティ・パフォーマンス・レスポンシブ・状態設計・CSS基盤"
date: 2026-06-05
status: research-log
---

# Webプラットフォーム固有のシステム制約

本ドキュメントは、AIやチームが「守るべき規範」としてWeb固有の制約をどこまで・どの粒度で定義すべきかを検討するための調査ログである。「AIが違反を自己検証できるほど具体的に落とせるか」という観点で整理している。

---

## 1. アクセシビリティ (Accessibility)

### 1.1 WCAG 概要と適合レベル

WCAG（Web Content Accessibility Guidelines）はW3C/WAIが策定するWebアクセシビリティの国際標準仕様である。

| バージョン | 公開年 | 主な追加内容 |
|-----------|-------|------------|
| WCAG 2.0  | 2008  | 基礎となる61の達成基準 |
| WCAG 2.1  | 2018  | 17の新基準（モバイル・認知・低視力対応） |
| WCAG 2.2  | 2023  | 9の新基準（フォーカス・タッチターゲット・認証） |
| WCAG 3.0  | 草稿中 | 2026年以降に完成予定。構造を全面改訂 |

**後方互換性:** WCAG 2.2を満たすコンテンツは2.1・2.0も満たす。

#### 適合レベル (Conformance Levels)

- **Level A（最低）:** 基本的な障壁を除去。必須要件の最小セット
- **Level AA（中間）:** 重大な障壁をほぼ除去。多くの組織・法令が求めるレベル
- **Level AAA（最高）:** 可能な限り高いアクセシビリティ。一部の基準は全ページへの適用が非現実的

#### POUR の4原則

| 原則 | 英語 | 概要 |
|-----|------|------|
| 知覚可能 | Perceivable | ユーザーが情報・UIを知覚できること |
| 操作可能 | Operable | UIコンポーネントとナビゲーションが操作できること |
| 理解可能 | Understandable | 情報とUIの操作が理解できること |
| 堅牢性 | Robust | 支援技術を含む様々なUAで確実に解釈できること |

---

### 1.2 WCAG 2.2 新追加の9基準（詳細）

WCAG 2.2（2023年10月公開）で追加された基準は以下の通り。AIが検証可能な数値基準を含む。

| 基準番号 | 名称 | Level | 具体的な閾値・要件 |
|---------|------|-------|-----------------|
| 2.4.11 | フォーカスの非遮蔽（最低限） | AA | キーボードフォーカスを受けたコンポーネントが、著者作成コンテンツによって「完全に」隠れていないこと（部分的な表示はOK） |
| 2.4.12 | フォーカスの非遮蔽（強化） | AAA | キーボードフォーカスを受けたコンポーネントのいかなる部分も著者コンテンツに隠れていないこと |
| 2.4.13 | フォーカスの外観 | AAA | フォーカスインジケーターが最低2CSSピクセルの太さ、かつフォーカス前後で3:1以上のコントラスト比を持つこと |
| 2.5.7 | ドラッグ動作 | AA | ドラッグが必要な機能に対して、シングルポインターによる代替手段を提供すること |
| 2.5.8 | ターゲットサイズ（最低限） | AA | ポインターターゲットが最低24×24CSSピクセルであること（例外あり） |
| 3.2.6 | 一貫したヘルプ | A | ヘルプ機能が複数ページにわたって存在する場合、同じ相対的な順序で配置すること |
| 3.3.7 | 冗長な入力 | A | 同一プロセス内で再度求められる情報は自動入力またはユーザーが選択可能にすること |
| 3.3.8 | アクセシブル認証（最低限） | AA | パスワード記憶・パズル解決などの認知機能テストを認証に使用しないこと（代替手段がある場合） |
| 3.3.9 | アクセシブル認証（強化） | AAA | 認知機能テストを一切使用しない認証代替手段を提供すること |

---

### 1.3 カラーコントラスト比の基準

| 対象 | Level | 最低コントラスト比 |
|------|-------|-----------------|
| 通常テキスト（18pt未満、bold 14pt未満） | AA | **4.5:1** |
| 大きいテキスト（18pt以上、bold 14pt以上） | AA | **3:1** |
| 通常テキスト | AAA | 7:1 |
| 大きいテキスト | AAA | 4.5:1 |
| UIコンポーネント・グラフィック要素（1.4.11） | AA | **3:1** |
| カスタムフォーカスインジケーター（2.4.13） | AAA | **3:1**（フォーカス前後の状態間で） |

**AIが検証可能な形式:** コントラスト比はWCAG公式算式（相対輝度の比）で算出可能。axe-core・Lighthouse・Storybook a11yアドオンで自動検出できる。

---

### 1.4 フォーカス可視性・キーボード操作

**フォーカス関連の必須要件:**

- **2.1.1（A）:** すべての機能がキーボードのみで操作可能であること
- **2.1.2（A）:** キーボードフォーカスがトラップされないこと
- **2.4.7（AA）:** キーボードフォーカスを受けた要素に視覚的なインジケーターがあること
- **2.4.11（AA）:** フォーカスを受けた要素が完全に隠れていないこと（WCAG 2.2新規）

**実装パターン:**
```css
/* スタイルリセットに注意：outline: none は設定しないこと */
:focus-visible {
  outline: 3px solid #005fcc;
  outline-offset: 2px;
}
```

**フォーカス管理の重要場面:**
- モーダルダイアログ開閉時（フォーカスをダイアログ内に閉じ込め、閉じたら戻す）
- ページ遷移後（メインコンテンツへのスキップリンク）
- 動的コンテンツ追加後

---

### 1.5 タッチターゲットサイズ

| 基準 | Level | サイズ | 対象 |
|------|-------|-------|------|
| 2.5.5 Target Size (Enhanced) | AAA | **44×44CSSピクセル** | すべてのポインターターゲット |
| 2.5.8 Target Size (Minimum) | AA | **24×24CSSピクセル** | 例外なしの最低保証 |

**プラットフォーム参考値:**
- Apple iOS HIG: 最低44×44pt（44px相当）
- Android Material Design: 最低48×48dp（物理的に約9mm）

**CSS実装例（2.5.8準拠）:**
```css
button {
  min-height: 24px;
  min-width: 24px;
  /* 実用上は44px以上を推奨 */
}
```

---

### 1.6 テキストリサイズと拡大表示

| 基準 | Level | 要件 |
|------|-------|------|
| 1.4.4（AA）| AA | 200%ズームでもコンテンツが読める・機能する |
| 1.4.10（AA）| AA | 320px幅（1280px×400%ズーム相当）で横スクロールしないこと |
| 1.4.12（AA）| AA | 行間1.5倍、文字間0.12em以上、単語間0.16em以上、段落後スペース2em以上でも機能すること |

---

### 1.7 WAI-ARIA と ARIA Authoring Practices Guide (APG)

#### ARIAの5つのルール（重要度順）

1. **ARIAを使わない:** ネイティブHTML（`<button>`、`<nav>`など）が常に優先
2. **ネイティブセマンティクスを上書きしない:** HTMLの意味を壊すrole付与を避ける
3. **キーボード操作は必須:** ARIAのインタラクティブ要素はすべてキーボード操作可能にする
4. **フォーカス可能要素を隠さない:** `aria-hidden="true"` と focusable要素の組み合わせは禁止
5. **アクセシブルな名前を提供する:** すべてのインタラクティブ要素に適切なラベルをつける

#### WAI-ARIA 1.2/1.3 の主要コンテンツ

**ロール（Roles）の分類:**
- **ウィジェットロール:** `button`, `checkbox`, `dialog`, `listbox`, `menu`, `slider`, `tab`, `tooltip`
- **ドキュメント構造ロール:** `article`, `definition`, `document`, `figure`, `heading`, `list`, `note`
- **ランドマークロール:** `banner`, `complementary`, `contentinfo`, `form`, `main`, `navigation`, `region`, `search`

**WAI-ARIA 1.3（2024年1月 Working Draft）の新要素:**
- 新ロール: `suggestion`, `comment`, `mark`
- 新属性: `aria-description`, `aria-braillelabel`, `aria-brailleroledescription`
- `aria-details` が複数IDRefをサポート

**ARIA APG（Authoring Practices Guide）の活用:**
- ダイアログ・モーダル: 開いたときのフォーカス移動、Escキーで閉じる、フォーカストラップ
- コンボボックス: 展開/折りたたみの `aria-expanded`, 選択肢の `aria-selected`
- タブパネル: `role="tablist"` + `role="tab"` + `aria-controls`
- アコーディオン: `aria-expanded`, 矢印キーナビゲーション
- カルーセル: `aria-live="polite"` または `aria-live="off"`（オートプレイ時）

**重要な属性:**
```html
<!-- チェックボックス状態 -->
<div role="checkbox" aria-checked="true">同意する</div>

<!-- 展開状態 -->
<button aria-expanded="false" aria-controls="menu">メニュー</button>

<!-- エラー説明 -->
<input aria-describedby="error-msg" aria-invalid="true">
<span id="error-msg">必須項目です</span>
```

---

### 1.8 アクセシビリティをデザインシステムに組み込む方法

**コンポーネントレベルの組み込み原則:**

1. **セマンティクスを仕様化する:** 各コンポーネントのARIAロール・状態・プロパティをドキュメント化
2. **キーボードパターンを定義する:** APGのパターンを参照し、必須キー操作（Tab, Enter, Space, 矢印）を明示
3. **フォーカス管理を実装仕様に含める:** 特にモーダル・トースト・ドロワーの開閉時
4. **カラーコントラストをトークンで保証する:** 背景/前景の組み合わせをトークンペアで管理
5. **状態の伝達手段を多重化する:** 色だけでなく、テキスト・アイコン・パターンでも状態を伝える

---

## 2. レスポンシブ/レイアウト

### 2.1 ブレークポイント設計とモバイルファースト

**モバイルファーストアプローチ:**
- 小さい画面から設計を開始し、`min-width` メディアクエリで大きい画面に拡張する
- デバイスサイズではなく、コンテンツのブレークポイントを優先する

**一般的なブレークポイント参考値（絶対値ではなくコンテンツ基準で設定すること）:**
```css
/* モバイルファースト */
/* base: ~320px (モバイル) */
@media (min-width: 640px) { /* sm: タブレット縦 */ }
@media (min-width: 768px) { /* md: タブレット横 */ }
@media (min-width: 1024px) { /* lg: デスクトップ */ }
@media (min-width: 1280px) { /* xl: 大画面 */ }
```

**現代的なベストプラクティス:**
- メディアクエリはひとつのツールに過ぎない。CSS Grid・Flexbox・Container Queriesと組み合わせる
- ブレークポイントを「デバイス」ではなく「レイアウトの臨界点」として定義する
- 絶対単位（px）ではなく相対単位（em, rem）でブレークポイントを記述することを推奨する見解もある

---

### 2.2 Fluid Typography / Fluid Spacing

`clamp()` 関数を使って、ブレークポイントなしで滑らかにスケールするタイポグラフィを実現できる。

**基本構文:**
```css
html {
  /* min: ユーザー設定に紐づくem値、preferred: ビューポート連動、max: 上限 */
  font-size: clamp(1em, 17px + 0.24vw, 1.125em);
}
```

**コンテナクエリ対応の流体タイポグラフィ:**
```css
/* vwの代わりにcqiを使用（コンテナ相対） */
font-size: clamp(1em, 16px + 0.25cqi, 1.25em);
```

**数学的タイプスケール（modular scale）:**
```css
html {
  --scale: 1.2; /* 完全4度スケール */
  --text-lg:   calc(1rem * pow(var(--scale), 1)); /* 1.2rem */
  --text-xl:   calc(1rem * pow(var(--scale), 2)); /* 1.44rem */
  --text-2xl:  calc(1rem * pow(var(--scale), 3)); /* 1.728rem */
}
```

**アクセシビリティとの関係:**
- `clamp()` の最大値を最小値の2.5倍以下に保つことで WCAG 1.4.4 に準拠
- ビューポートへの依存度が増すほど、ユーザーのフォントサイズ設定への応答性が下がる（トレードオフ）

---

### 2.3 Container Queries（コンテナクエリ）

ビューポートではなく親コンテナのサイズに基づいてスタイルを切り替える。

```css
.card-container {
  container-type: inline-size;
  container-name: card;
}

@container card (min-width: 400px) {
  .card { flex-direction: row; }
}

/* コンテナ相対単位 */
.heading {
  font-size: clamp(1rem, 5cqi, 2rem); /* cqi = コンテナのinline-size の1% */
}
```

**設計システムへの示唆:**
- コンポーネントが「どのコンテキストに置かれるか」に依存しないポータブルな設計が可能
- ページレイアウトとコンポーネントレイアウトの責任を明確に分離できる

---

### 2.4 CSS論理プロパティと国際化（RTL対応）

物理的な方向（left/right/top/bottom）ではなく、テキストの流れに基づく論理プロパティを使用する。

| 物理プロパティ | 論理プロパティ | 説明 |
|-------------|-------------|------|
| `margin-left` | `margin-inline-start` | LTRでは左、RTLでは右 |
| `margin-right` | `margin-inline-end` | LTRでは右、RTLでは左 |
| `padding-top` | `padding-block-start` | 書き込み方向の開始 |
| `padding-bottom` | `padding-block-end` | 書き込み方向の終端 |
| `left` | `inset-inline-start` | 位置指定の開始方向 |
| `text-align: left` | `text-align: start` | テキストの開始方向 |
| `border-left` | `border-inline-start` | インライン方向の開始辺 |

**実装方針:**
```css
/* 非推奨（物理プロパティ） */
label { margin-right: 0.5em; }

/* 推奨（論理プロパティ） */
label { margin-inline-end: 0.5em; }
```

`html` 要素に `dir="rtl"` を付与するだけで論理プロパティが自動反転する。

**多言語対応の設計システム上の考慮事項:**
- CJK言語はboldフォントが存在しないことが多く、`font-weight: 700` → `400` への上書き定義が必要
- Figmaのデフォルト行間（1.2倍）はWCAGの1.5倍基準を下回る
- 言語カテゴリ別（西欧/スラブ、CJKなど）のトークンセットを用意することが現実的

---

## 3. パフォーマンス

### 3.1 Core Web Vitals の閾値と測定

INPはFIDの代替として2024年3月12日に正式なCore Web Vitalとなった。

| 指標 | 説明 | 良好 | 改善が必要 | 不良 |
|-----|------|------|----------|------|
| **LCP** (Largest Contentful Paint) | 最大コンテンツ要素の描画完了時間 | ≤**2.5秒** | 2.5〜4秒 | >4秒 |
| **INP** (Interaction to Next Paint) | ユーザー操作への応答時間（最長値） | ≤**200ms** | 200〜500ms | >500ms |
| **CLS** (Cumulative Layout Shift) | 予期しないレイアウトシフトの累積値 | ≤**0.1** | 0.1〜0.25 | >0.25 |

**測定基準:** 75パーセンタイル（ページロードの75%がこの値以内）、モバイル・デスクトップを分けて計測。

**閾値の科学的根拠:**
- LCP 2.5秒: ユーザー注意持続（0.3〜3秒）の研究と達成可能性のバランス（42%のモバイルオリジンが達成）
- INP 200ms: 因果知覚研究（〜100ms）vs. 現実の達成率（100ms基準では12%のみ達成、200msで56%）
- CLS 0.1: 0.05（49%達成）は厳しすぎ、0.1が品質と現実性のバランス点

**測定ツール:**

| ツール | フィールド/ラボ | 特徴 |
|-------|-------------|------|
| Chrome UX Report (CrUX) | フィールド | 実ユーザーデータ |
| PageSpeed Insights | 両方 | CrUX + Lighthouseを統合 |
| Lighthouse | ラボ | LCP/CLS測定、INPはTBTで代替 |
| web-vitals ライブラリ | フィールド | JavaScriptで計測・送信 |
| Chrome DevTools | ラボ | INPのデバッグに対応 |

---

### 3.2 デザイン判断がパフォーマンスに与える影響

#### LCPへの影響要因

| デザイン要素 | 影響 | 対策 |
|------------|------|------|
| ヒーロー画像 | 最大コンテンツ要素になりやすい → LCP直撃 | `fetchpriority="high"`, `loading="eager"`, プリロード |
| Webフォントのテキスト | フォントブロック期間中は描画されない | `font-display: swap` または `optional` |
| 背景グラデーション画像 | CSSグラデーションに置換可能 | `background: linear-gradient(...)` でHTTP要求削減 |
| 遅延読み込みLCP要素 | `loading="lazy"` をLCPに使うと遅延 | LCP要素には `loading="lazy"` 禁止 |
| クライアントサイドレンダリング | HTMLにLCP要素が存在しない | SSR/SSG、または`<link rel=preload>` |

#### CLSへの影響要因

| デザイン要素 | 影響 | 対策 |
|------------|------|------|
| サイズ未指定の画像 | 読み込み後にレイアウトがシフト | `width`/`height` 属性を必ず設定、または `aspect-ratio` |
| Webフォントのスワップ | フォールバックと本フォントで字幅が異なる | `font-display: optional`、または `size-adjust` / `ascent-override` |
| 動的コンテンツ挿入（広告・Cookie通知など） | 上部への挿入でページがずれる | `min-height` で空間予約、下部に配置 |
| レイアウトをトリガーするCSSアニメーション | `margin`, `top`, `left` のアニメーション | `transform` / `opacity` のみアニメーション |

**アニメーションのCLS無効化:**
`transform: translate()` は他の要素に影響しないためCLSにカウントされない。

#### INPへの影響要因

| 要因 | 影響 | 対策 |
|-----|------|------|
| 巨大なDOM | レイアウト再計算コストが増大 | DOM要素数を最小化、CSS containmentを活用 |
| 長いJavaScriptタスク（>50ms） | ユーザー操作への応答を遅延 | タスクを分割（`scheduler.yield()`）|
| 複雑なCSSセレクター | レイアウト計算の負荷が増す | セレクターの単純化 |

---

### 3.3 パフォーマンス予算 (Performance Budget)

パフォーマンス予算とは、サイトのパフォーマンスに関する定量的な制限値（目標値）である。

**予算の設定例:**
- LCP ≤ 2.5秒（75パーセンタイル）
- CLS ≤ 0.1（75パーセンタイル）
- INP ≤ 200ms（75パーセンタイル）
- 画像の最大ファイルサイズ: ヒーロー画像 200KB以下
- Webフォントの合計: 100KB以下（WOFF2）
- 初回ロード時のCSS合計: 50KB以下（gzip後）
- Total Blocking Time (TBT): 200ms以下

**デザインシステムへの組み込み:**
- コンポーネントごとにCSS/JSの目安サイズを定義
- アニメーションは `transform` / `opacity` に限定し、layout-triggering propertyを禁止
- 画像コンポーネントは `width`/`height` と `aspect-ratio` を必須プロパティとする

---

### 3.4 Webフォントの最適化

| 手法 | 効果 | 推奨度 |
|------|------|------|
| WOFF2のみ使用 | 圧縮率がWOFFより30%高い | 必須 |
| `font-display: optional` | CLS完全防止、フォント未表示リスクあり | LCP要素のフォントに推奨 |
| `font-display: swap` | テキストがすぐ表示されるが、スワップ時にCLS | 一般テキスト向け |
| `font-display: block` | 初期に非表示、スワップで見えるが遅い | 原則避ける |
| `unicode-range` によるサブセット | 未使用グリフを除き、ファイルサイズ削減 | 多言語サイト以外でも有効 |
| `size-adjust` / `ascent-override` | フォールバックとのサイズ差を縮小 → CLS軽減 | swapと組み合わせ |
| Variable fonts | 複数ウェイト/スタイルを1ファイルで提供 | 複数スタイル使用時に有効 |
| アイコンフォント → SVGへ移行 | フォールバック時の表示崩れ防止 | 強く推奨 |

---

## 4. モーション/インタラクション

### 4.1 prefers-reduced-motion

前庭障害（vestibular disorder）は、米国では40歳以上の成人の約35%（約6,900万人）が何らかの前庭機能障害を経験するとされており（NHANES 2001–2004調査）、過剰なアニメーションがめまい・吐き気・偏頭痛を引き起こす可能性がある。この数値は米国の調査データであり、世界全体の罹患者数ではない点に注意。

**WCAG 関連基準:**
- **2.3.1（A）:** 1秒間に3回を超えるフラッシュ禁止
- **2.2.2（A）:** 5秒超の自動動作コンテンツに一時停止機能を提供

**CSS実装パターン:**

```css
/* 推奨: デフォルトでアニメーションを提供し、設定でオフに */
@media (prefers-reduced-motion: no-preference) {
  .animated { animation: slide-in 0.3s ease-out; }
}

@media (prefers-reduced-motion: reduce) {
  .animated { animation: none; /* または duration を 0 に */ }
}
```

**アニメーションの安全性分類:**

| 分類 | 例 | reduced-motion時の扱い |
|-----|---|----------------------|
| 非モーションアニメーション | 色変化・opacity変化 | そのまま維持可 |
| 非本質的アニメーション | 装飾的なスライドイン | 完全に削除 |
| 本質的アニメーション | カートへの追加、プログレスバー | 必要最小限に縮小（回転速度削減・スケール効果除去・最大5秒） |

**問題のあるアニメーション（避けるべき):**
- 画面の1/3を超える大きな移動
- パララックス効果・多方向移動
- スケール・ズーム・ブラー・渦巻き効果
- 自動再生するコンテンツ
- 形状が変形するモーフィング

---

### 4.2 モーション原則の標準化

**duration（継続時間）の目安:**

| ユースケース | 推奨 duration |
|-----------|--------------|
| マイクロインタラクション（ボタン hover, フォーカス） | 100〜200ms |
| 要素の表示/非表示（フェード、スライド） | 200〜400ms |
| ページ遷移・モーダル開閉 | 300〜500ms |
| reduced-motion時 | 0ms または1000ms以上に延長 |
| essential animationの最大値（reduced-motion対応） | 5000ms |

**easing（イージング）の原則:**
- 加速して終わる: `ease-in`（要素が退場するとき）
- 減速して終わる: `ease-out`（要素が登場するとき）
- 加速後減速: `ease-in-out`（要素が移動するとき）
- 線形: `linear`（スクロール連動など） 

**デザインシステムへの組み込み:**
```css
:root {
  --motion-duration-fast: 100ms;
  --motion-duration-base: 250ms;
  --motion-duration-slow: 400ms;
  --motion-easing-enter: cubic-bezier(0, 0, 0.2, 1);   /* ease-out */
  --motion-easing-exit:  cubic-bezier(0.4, 0, 1, 1);    /* ease-in */
  --motion-easing-move:  cubic-bezier(0.4, 0, 0.2, 1);  /* ease-in-out */
}

@media (prefers-reduced-motion: reduce) {
  :root {
    --motion-duration-fast: 0ms;
    --motion-duration-base: 0ms;
    --motion-duration-slow: 0ms;
  }
}
```

---

## 5. UI 状態設計

### 5.1 コンポーネントが持つべき状態の一覧

デザインシステムにおける「状態の網羅的定義」はUX品質の基盤である。

| 状態 | 英語 | 説明 | 実装上の注意 |
|-----|------|------|------------|
| デフォルト | Default | 通常表示の基本状態 | ベースとなる表示 |
| ホバー | Hover | ポインターが重なっている状態 | タッチデバイスでは発生しない |
| フォーカス | Focus | キーボード/タブでフォーカスされている状態 | `focus-visible` 疑似クラスを活用 |
| アクティブ | Active | クリック/タップ中の状態 | `:active` 擬似クラス |
| 選択済み | Selected | 選択されている状態 | `aria-selected="true"` |
| チェック済み | Checked | チェックが入っている状態 | `aria-checked="true"` |
| 無効 | Disabled | 操作できない状態 | `disabled` 属性 + `aria-disabled` |
| 読み込み中 | Loading | データ取得・処理中 | `aria-busy="true"` |
| 空 | Empty | データが存在しない状態 | ガイダンスとCTA（行動促進）を表示 |
| エラー | Error | 失敗・問題が発生した状態 | `aria-invalid`, エラーメッセージのリンク |
| 成功 | Success | 操作が正常に完了した状態 | `aria-live="polite"` で通知 |
| スケルトン | Skeleton | 実際のコンテンツが来る前のプレースホルダー | `aria-busy="true"` をコンテナに付与 |
| 展開/折りたたみ | Expanded/Collapsed | アコーディオン等の開閉状態 | `aria-expanded="true/false"` |

### 5.2 状態の設計原則

**ローディング状態:**
- スケルトンスクリーン: コンテンツのテンプレートが確定している場合（体感的な速さが向上）
- スピナー: コンテンツが空になる可能性がある場合
- `aria-busy="true"` をコンテナに付与し、スクリーンリーダーに「更新中」を伝える

**エラー状態:**
- エラーメッセージはフォームの上部にリスト表示（2.4.3 Focus Order準拠）
- 各フィールドのエラーは `aria-describedby` でフィールドと関連付ける
- `aria-invalid="true"` をエラー状態のフィールドに付与
- 色だけでなくテキスト・アイコンでもエラーを伝える（1.4.1準拠）

**空状態:**
- 「なぜ空なのか」を説明するコンテキストを提供
- 次のアクション（CTAボタン、ガイダンステキスト）を提示
- ユーザーを行き詰まらせない設計

**無効状態:**
- なぜ無効なのかを説明するツールチップや補助テキストを提供
- `disabled` 属性はフォーム要素で使用（フォームバリデーション除外の副作用に注意）
- カスタムコンポーネントでは `aria-disabled="true"` + 視覚的なスタイル変更

**状態間のトランジション:**
- 状態変化をアニメーションで示す場合は `prefers-reduced-motion` を考慮
- `aria-live` リージョンで状態変化をスクリーンリーダーに通知

---

## 6. CSS 基盤

### 6.1 CSS Custom Properties（カスタムプロパティ）とデザイントークン

CSS カスタムプロパティ（CSS変数）はデザインシステムのトークン管理の中核技術である。

**基本構文:**
```css
:root {
  /* カラートークン */
  --color-primary-500: #0066cc;
  --color-text-default: #1a1a1a;

  /* スペーシングトークン */
  --spacing-base: 8px;
  --spacing-md: calc(var(--spacing-base) * 2); /* 16px */

  /* タイポグラフィトークン */
  --font-size-base: 1rem;
  --line-height-base: 1.5;
}
```

**`@property` アットルール（型付きカスタムプロパティ）:**
```css
@property --logo-color {
  syntax: "<color>";      /* 型チェック: colorとして扱う */
  inherits: false;         /* 継承を無効化 */
  initial-value: #c0ffee; /* フォールバック値 */
}
```

`@property` を使うと、型違いの値が入った場合にフォールバック値が適用される。アニメーションのトランジションにも使用可能（グラデーションのアニメーションなど）。

**ブラウザサポート:** `@property` は Chrome 85+、Firefox 113+、Safari 16.4+

---

### 6.2 デザイントークンの実装パターン

**3層構造（推奨）:**
```css
/* 1. Primitive tokens（原始トークン） */
:root {
  --blue-500: #0066cc;
  --gray-100: #f5f5f5;
}

/* 2. Semantic tokens（意味トークン） */
:root {
  --color-action-primary: var(--blue-500);
  --color-background-subtle: var(--gray-100);
}

/* 3. Component tokens（コンポーネントトークン） */
.button {
  --button-bg: var(--color-action-primary);
  background-color: var(--button-bg);
}
```

**テーマ切り替え（ダークモード）:**
```css
:root { --color-bg: white; --color-text: black; }

@media (prefers-color-scheme: dark) {
  :root { --color-bg: #1a1a1a; --color-text: white; }
}
```

**CSSカスタムプロパティの制約:**
- メディアクエリ/コンテナクエリの値として直接使用不可
- プロパティ名・セレクターとして使用不可
- 部分的な値（例: `margin: var(--size)px`）として使用不可

---

### 6.3 Cascade Layers（カスケードレイヤー）

`@layer` は特異性（specificity）を超えてスタイルの優先順位を制御する。デザインシステムのCSS管理において、特異性競合問題を根本的に解決する。

**基本的な使用方法:**
```css
/* レイヤーの宣言順序を先に定義 */
@layer reset, base, tokens, components, utilities;

@layer reset {
  * { box-sizing: border-box; margin: 0; }
}

@layer tokens {
  :root { --color-primary: #0066cc; }
}

@layer components {
  @layer elements {
    .button { background: var(--color-primary); }
  }

  @layer modifiers {
    .button.success { --color-primary: green; }
  }

  @layer states {
    .button:hover { background: color-mix(in srgb, var(--color-primary), white 10%); }
  }
}

@layer utilities {
  .sr-only { position: absolute; clip: rect(0 0 0 0); }
}
```

**カスケードレイヤーの優先規則:**
- 後に宣言されたレイヤーが優先（`utilities` > `components` > `tokens`）
- レイヤー内の特異性は通常通り適用される
- **レイヤーの優先度は特異性より強い**（`!important` の反転に注意）

---

### 6.4 最新のCSS機能がデザインシステムに与える影響

#### `:has()` 擬似クラス

親または先行要素の状態に基づいてスタイルを適用できる「親セレクター」。

```css
/* inputがfocusされているとき、ラベルも強調 */
.form-group:has(input:focus) label {
  color: var(--color-primary);
  font-weight: bold;
}

/* チェックボックスがチェックされているとき、関連コンテンツを表示 */
.card:has(.checkbox:checked) .card-detail {
  display: block;
}
```

#### `color-mix()` 関数

2つの色を指定した比率で混合する。ダークモード・ホバー状態・透明度表現に活用できる。

```css
.button:hover {
  background: color-mix(in srgb, var(--color-primary), white 15%);
}

.button:active {
  background: color-mix(in srgb, var(--color-primary), black 10%);
}
```

#### `oklch()` / `oklch` 色空間

知覚的に均一な色空間（Oklch）で、明度・彩度・色相を独立して操作できる。

```css
:root {
  --color-primary-h: 250;  /* 色相: 青紫 */
  --color-primary-c: 0.2;  /* 彩度 */
  --color-primary-l: 0.5;  /* 明度 */

  --color-primary: oklch(var(--color-primary-l) var(--color-primary-c) var(--color-primary-h));
  --color-primary-light: oklch(0.8 var(--color-primary-c) var(--color-primary-h));
  --color-primary-dark: oklch(0.3 var(--color-primary-c) var(--color-primary-h));
}
```

#### `@scope` アットルール

コンポーネントのスタイルを特定のDOMサブツリーに限定できる（カプセル化）。

```css
@scope (.card) {
  :scope { padding: var(--spacing-md); }
  .title { font-size: var(--text-lg); }
}
```

---

## 7. 制約の機械可読・検証可能な定義

### 7.1 自動検査ツールの能力と限界

**重要な前提:** 自動テストが検出できるアクセシビリティの問題の割合については諸説ある。WCAG達成基準の数で換算すると自動化可能な基準は約30%程度にとどまるが、Dequeの調査（13,000ページ超を分析）では実際の問題件数ベースで約57%を自動検出できると報告している。いずれにせよ、残りはmanualテストが必要である。

| ツール | 種別 | 検出できること | 限界 |
|------|------|-------------|------|
| **eslint-plugin-jsx-a11y** | 静的解析（JSX） | alt属性欠落、role不正、label未設定、aria属性の構文エラー | 動的レンダリング結果は検査不可 |
| **axe-core** | DOMベース実行時 | コントラスト比、ARIA論理矛盾、ランドマーク欠如、フォームラベル | ユーザーフロー依存の問題は難しい |
| **Lighthouse** | ラボ計測 | axe-coreのサブセット + CWV計測 | axe-coreより網羅性が低い |
| **jest-axe / cypress-axe** | CI統合テスト | 各コンポーネントのaxeルール違反 | インタラクション後の状態変化は要設定 |
| **Storybook a11y addon** | コンポーネント開発時 | コンポーネント単位のaxeチェック | 実ページのコンテキストは反映されない |
| **Deque axe DevTools** | ブラウザ拡張 | Guided tests（自動+手動誘導のハイブリッド） | 組織ライセンスが必要 |

### 7.2 自動検証可能な制約の例

以下の制約はルールとして記述し、CIで自動チェック可能である。

```javascript
// eslint-plugin-jsx-a11y（.eslintrc の設定例）
{
  "plugins": ["jsx-a11y"],
  "rules": {
    "jsx-a11y/alt-text": "error",           // img のalt必須
    "jsx-a11y/aria-roles": "error",         // 有効なroleのみ
    "jsx-a11y/label-has-associated-control": "error", // label必須
    "jsx-a11y/no-autofocus": "warn",        // autofocus禁止
    "jsx-a11y/click-events-have-key-events": "error", // clickにkeydown/keyup必須
    "jsx-a11y/interactive-supports-focus": "error"    // interactive要素はfocus可能
  }
}
```

```javascript
// jest-axe（ユニットテスト）
import { axe, toHaveNoViolations } from 'jest-axe';
expect.extend(toHaveNoViolations);

test('Button should have no accessibility violations', async () => {
  const { container } = render(<Button>Click me</Button>);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### 7.3 パフォーマンス制約の自動検証

```json
// Lighthouse CI の設定例（lighthouserc.json）
{
  "ci": {
    "assert": {
      "assertions": {
        "categories:performance": ["error", {"minScore": 0.9}],
        "categories:accessibility": ["error", {"minScore": 0.9}],
        "largest-contentful-paint": ["error", {"maxNumericValue": 2500}],
        "cumulative-layout-shift": ["error", {"maxNumericValue": 0.1}],
        "total-blocking-time": ["error", {"maxNumericValue": 200}]
      }
    }
  }
}
```

### 7.4 デザイントークンの型検証

`@property` と StyleDictionary・Figma Tokens Studioを組み合わせることで、トークンの型・値域を機械的に検証できる。

```css
@property --spacing-base {
  syntax: "<length>";        /* px, rem, em のみ許容 */
  inherits: true;
  initial-value: 8px;
}

@property --color-primary {
  syntax: "<color>";         /* 有効な色値のみ許容 */
  inherits: true;
  initial-value: #0066cc;
}
```

---

## 8. アクセシビリティチェックリスト（AIが参照可能な形式）

以下は、AIが「違反を自己検証する」ために使用できる機械的なチェックリストである。

### カラー・コントラスト
- [ ] 通常テキストのコントラスト比 ≥ 4.5:1
- [ ] 大きいテキスト（18pt以上）のコントラスト比 ≥ 3:1
- [ ] UIコンポーネント・グラフィック要素のコントラスト比 ≥ 3:1
- [ ] 色だけで情報を伝えていない（テキスト・アイコン・パターンでも表現）

### フォーカス・キーボード
- [ ] すべてのインタラクティブ要素がキーボードでアクセス可能
- [ ] フォーカスインジケーターが視覚的に見える（`outline: none` の単独使用禁止）
- [ ] フォーカス順序が視覚的な表示順と一致している
- [ ] フォーカスがトラップされる場所がない（モーダル以外）
- [ ] モーダル開閉時にフォーカスが適切に移動する

### タッチターゲット
- [ ] すべてのインタラクティブ要素のサイズ ≥ 24×24px（AA: 2.5.8）
- [ ] 重要なインタラクティブ要素のサイズ ≥ 44×44px（AAA: 2.5.5, 推奨）

### テキスト・拡大
- [ ] 200%ズームでコンテンツが読めて機能する
- [ ] 320px幅で横スクロールが発生しない

### 画像・代替テキスト
- [ ] 意味のある画像に `alt` 属性あり（空でない）
- [ ] 装飾画像に `alt=""` を指定

### フォーム
- [ ] すべての入力フィールドに関連付けられた `<label>` がある
- [ ] エラーメッセージが `aria-describedby` でフィールドと関連付けられている
- [ ] エラー状態のフィールドに `aria-invalid="true"` がある

### 動的コンテンツ・ARIA
- [ ] ライブリージョンに `aria-live` が設定されている
- [ ] 展開可能な要素に `aria-expanded` がある
- [ ] ローディング中のコンテナに `aria-busy="true"` がある
- [ ] カスタムウィジェットに適切な ARIA ロールがある

### モーション
- [ ] `prefers-reduced-motion: reduce` 時にアニメーションが無効化または縮小される
- [ ] 1秒間に3回以上フラッシュするコンテンツがない
- [ ] 5秒超の自動動作コンテンツに一時停止機能がある

### パフォーマンス
- [ ] LCP 要素に `fetchpriority="high"`、`loading="lazy"` なし
- [ ] 画像に `width`/`height` 属性または `aspect-ratio` が設定されている
- [ ] CSS アニメーションが `transform`/`opacity` のみ使用している
- [ ] Webフォントに `font-display` が設定されている

---

## 出典

| # | タイトル | URL |
|---|---------|-----|
| 1 | Web Content Accessibility Guidelines (WCAG) 2.2 | https://www.w3.org/TR/WCAG22/ |
| 2 | WCAG 2 Overview - WAI - W3C | https://www.w3.org/WAI/standards-guidelines/wcag/ |
| 3 | What's New in WCAG 2.2 - WAI - W3C | https://www.w3.org/WAI/standards-guidelines/wcag/new-in-22/ |
| 4 | WCAG 3 Introduction - WAI - W3C | https://www.w3.org/WAI/standards-guidelines/wcag/wcag3-intro/ |
| 5 | WebAIM: WCAG 2 Checklist | https://webaim.org/standards/wcag/checklist |
| 6 | ARIA Authoring Practices Guide (APG) - W3C | https://www.w3.org/WAI/ARIA/apg/ |
| 7 | WAI-ARIA 1.2 - W3C | https://www.w3.org/TR/wai-aria-1.2/ |
| 8 | WAI-ARIA 1.3 First Public Working Draft - W3C | https://www.w3.org/TR/2024/WD-wai-aria-1.3-20240123/ |
| 9 | WAI-ARIA basics - MDN | https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics |
| 10 | Web Vitals - web.dev | https://web.dev/articles/vitals |
| 11 | How Core Web Vitals Thresholds Were Defined - web.dev | https://web.dev/articles/defining-core-web-vitals-thresholds |
| 12 | Largest Contentful Paint (LCP) - web.dev | https://web.dev/articles/lcp |
| 13 | Optimize LCP - web.dev | https://web.dev/articles/optimize-lcp |
| 14 | Optimize Cumulative Layout Shift - web.dev | https://web.dev/articles/optimize-cls |
| 15 | Optimize INP - web.dev | https://web.dev/articles/optimize-inp |
| 16 | Top CWV improvements - web.dev | https://web.dev/articles/top-cwv |
| 17 | CSS for Web Vitals - web.dev | https://web.dev/css-web-vitals/ |
| 18 | Best Practices for Fonts - web.dev | https://web.dev/articles/font-best-practices |
| 19 | Responsive and Fluid Typography with Baseline CSS - web.dev | https://web.dev/articles/baseline-in-action-fluid-type |
| 20 | Container queries and units in action - web.dev | https://web.dev/articles/baseline-in-action-container-queries |
| 21 | Internationalization - web.dev | https://web.dev/learn/design/internationalization |
| 22 | prefers-reduced-motion: Sometimes less movement is more - web.dev | https://web.dev/articles/prefers-reduced-motion |
| 23 | Animation and motion - web.dev Learn Accessibility | https://web.dev/learn/accessibility/motion |
| 24 | Using CSS custom properties - MDN | https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties |
| 25 | Responsive web design - MDN | https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design |
| 26 | prefers-reduced-motion CSS media feature - MDN | https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion |
| 27 | A Primer On CSS Container Queries - Smashing Magazine | https://www.smashingmagazine.com/2021/05/complete-guide-css-container-queries/ |
| 28 | Beyond CSS Media Queries - Smashing Magazine | https://www.smashingmagazine.com/2024/05/beyond-css-media-queries/ |
| 29 | Creating Accessible UI Animations - Smashing Magazine | https://www.smashingmagazine.com/2023/11/creating-accessible-ui-animations/ |
| 30 | Integrating Localization Into Design Systems - Smashing Magazine | https://www.smashingmagazine.com/2025/05/integrating-localization-into-design-systems/ |
| 31 | WCAG 3.0's Proposed Scoring Model - Smashing Magazine | https://www.smashingmagazine.com/2025/05/wcag-3-proposed-scoring-model-shift-accessibility-evaluation/ |
| 32 | Making Sense Of WAI-ARIA - Smashing Magazine | https://www.smashingmagazine.com/2022/09/wai-aria-guide/ |
| 33 | A Complete Guide To Accessibility Tooling - Smashing Magazine | https://www.smashingmagazine.com/2021/06/complete-guide-accessibility-tooling/ |
| 34 | Organizing Design System Component Patterns With CSS Cascade Layers - CSS-Tricks | https://css-tricks.com/organizing-design-system-component-patterns-with-css-cascade-layers/ |
| 35 | Cascade Layers Guide - CSS-Tricks | https://css-tricks.com/css-cascade-layers/ |
| 36 | Fluid Typography - CSS-Tricks | https://css-tricks.com/snippets/css/fluid-typography/ |
| 37 | Accessibility Checklist - The A11Y Project | https://www.a11yproject.com/checklist/ |
| 38 | eslint-plugin-jsx-a11y - GitHub | https://github.com/jsx-eslint/eslint-plugin-jsx-a11y |
| 39 | Accessibility audit with react-axe and eslint-plugin-jsx-a11y - web.dev | https://web.dev/articles/accessibility-auditing-react |
| 40 | Understanding Target Size (Minimum) 2.5.8 - W3C | https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html |

---

## このリポジトリへの示唆

このWhoOwnsDesignプロジェクトが「AIやチームに何を・どの粒度で・どの形式で用意すべきか」を定義するにあたり、本調査から以下の含意が導かれる。

### 1. 数値基準のある制約は「そのまま規範化」できる

カラーコントラスト比（4.5:1/3:1）、タッチターゲットサイズ（24px/44px）、Core Web Vitals閾値（LCP 2.5秒、INP 200ms、CLS 0.1）などは数値が明確であり、AIが自己検証できる形で規範化可能である。これらはデザインシステムの制約定義の「第一層」として、具体的な数値付きで記述すべきである。

### 2. 自動検証可能な制約と手動検証が必要な制約を分類して定義する

自動ツール（axe、Lighthouse、eslint-plugin-jsx-a11y）が検出できるアクセシビリティ問題の割合は、測定方法によって異なる。WCAG達成基準の網羅率で見ると約30%、実際の問題件数ベースでは約57%（Deque調査）という報告がある。いずれにせよ、自動検出だけでは全問題を把握できないため、規範文書では「自動検証可能」「手動検証が必要」を明示することで、AIとヒューマンの役割分担を明確にできる。

### 3. CSS変数とトークンの命名規則を規範に含める

CSS Custom PropertiesとDeign Tokenの3層構造（Primitive/Semantic/Component）は、AIが制約違反を検出しやすい形式である。`--color-action-primary` のような意味的命名規則と、`@property` による型制約を規範として明示すると、AIが「ハードコードされた色値がある」「semantic tokenを使っていない」などの違反を検出できる。

### 4. コンポーネントの状態一覧を規範として定義し、漏れを防ぐ

UI状態（Default/Loading/Error/Empty/Disabled/Success/Skeleton）の網羅的な定義は、AIがコンポーネント実装をレビューする際の「チェックリスト」として機能できる。「このコンポーネントにError状態の定義があるか」「aria-busyは設定されているか」などの検証が可能になる。

### 5. レスポンシブ制約は「物理ブレークポイント値」より「設計原則」として規範化する

具体的なpx値（640px, 768px等）はコンテキスト依存度が高く、プロジェクトによって異なる。代わりに「モバイルファーストでmin-widthを使う」「コンテナクエリでコンポーネントの自律的なレスポンシブ性を確保する」「CSS論理プロパティで物理方向の記述を禁止する」といった原則レベルの制約を規範化する方が、AIが汎用的に検証しやすい。

### 6. Webフォントとアニメーションのデザイン制約はパフォーマンスと直結する

「font-display を設定しないこと」「layoutをトリガーするCSSプロパティをアニメーションすること」「LCP要素にloading=lazyを設定すること」は、デザインシステムの使用規則として禁止事項として明記すべきである。これらはLighthouseやaxeで自動検出できる。

### 7. WCAG 3.0の動向は「監視対象」として位置づける

WCAG 3.0はまだ草稿段階（完成まで数年）であり、現時点での規範化は時期尚早である。Bronze/Silver/Goldの新適合モデルの方向性は把握しつつ、現行規範はWCAG 2.2（AA）を基準にするのが現実的である。ただし、WhoOwnsDesignのドキュメントには「WCAG 3.0の動向」として注記し、将来の更新を容易にする構造を持たせることが望ましい。
