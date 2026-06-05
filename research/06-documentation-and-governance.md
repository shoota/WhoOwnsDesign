---
title: "デザインシステムのドキュメンテーションとガバナンス(所有・運用・貢献)"
topic: design-system-documentation-governance
date: 2026-06-05
status: research-log
---

# デザインシステムのドキュメンテーションとガバナンス

## 概要

デザインシステムの成功は、コンポーネントの品質よりも**ドキュメンテーションとガバナンスの質**に左右されることが多い。2024〜2025年の調査(Zeroheight "Design Systems Report 2025"、294名調査)によると、デザインシステムの高採用を達成しているチームの共通点は、**高い信頼性(96%)、高い安定性(90%)、コミュニケーション満足度(75%)・ステークホルダーの賛同(75%)** であり、技術的な完成度よりも組織的な要素が際立っている。

---

## 1. デザインシステムドキュメントの構成・要素

### 1.1 全体構造のレイヤー

良いデザインシステムドキュメントは以下の階層で組織される:

| レイヤー | 内容 | 担当者 |
|---------|------|--------|
| ブランド・原則 | デザイン哲学、ビジョン、Voice&Tone | デザインリーダー |
| デザイントークン | カラー、スペーシング、タイポグラフィ、モーション等の変数定義 | デザイン+エンジニア |
| コンポーネント | 各UIパーツの仕様、用途、状態、APIリファレンス | デザイン+エンジニア |
| パターン | 複数コンポーネントの組み合わせによるソリューション | 全員 |
| コンテンツガイドライン | 文言、ラベリング、エラーメッセージのトーン | コンテンツストラテジスト |
| アクセシビリティガイドライン | WCAG基準、アノテーション、実装要件 | アクセシビリティ専門家 |
| Changelog・バージョン管理 | 変更履歴、deprecation、移行ガイド | 全員 |

### 1.2 コンポーネントドキュメントの必須ブロック

Design System Documentation Spec (DSDS、2025年6月時点でバージョン 0.2.1 Draft) は、コンポーネントドキュメントをブロック型で定義している。推奨される主要ブロックは以下:

- **Guidelines(ガイドライン)**: 使用目的・適切な文脈・誤用の防止
- **Anatomy(アナトミー)**: コンポーネントの構成要素の図解と名称
- **Variants(バリアント)**: サイズ、タイプ、色などのバリエーション
- **States(ステート)**: hover、focus、disabled、loading、error等
- **Do / Don't**: 正しい使い方と避けるべき使い方の例示
- **Accessibility(アクセシビリティ)**: WCAG適合、ARIAロール、キーボード操作、スクリーンリーダー対応
- **API Specs**: propsリスト、デフォルト値、型定義
- **Examples**: インタラクティブなデモ(静的画像より優先)
- **Content(コンテンツ)**: ラベル文言のガイドライン
- **Motion(モーション)**: アニメーション仕様
- **Interactions(インタラクション)**: フォーカス管理、複雑な操作フロー

#### Nathan Curtis の "4要素" (Storybook blog より)

コンポーネントドキュメントの最小セットとして Nathan Curtis は以下を定義している:
1. コンポーネントの説明と目的
2. インタラクティブなレンダリング例(静的画像より優先)
3. デザインリファレンスと使用ガイドライン
4. APIリファレンス(コード)

### 1.3 アクセシビリティの組み込み

GitHub の設計システムチームは「アクセシブルなコンポーネントを組み合わせても、自動的にアクセシブルなページにはならない」と指摘している。**インスタンス固有のアノテーション**が必要になる文脈例:

- コンポーネント内のheadingレベル(ページ構造によって変わる)
- アイコンボタンのアクセシブルラベル(文脈依存)
- フォームのバリデーションエラーメッセージ
- モーダルのフォーカス管理
- ARIA属性・セマンティックロール

GitHub のアプローチ: **「Preset annotations」** — コンポーネントドキュメントとStorybookデモに直接リンクされた事前設定済みのアクセシビリティアノテーションを提供し、設計ツールから離れることなく文脈固有のガイダンスを得られる仕組みを構築。

---

## 2. ドキュメントの情報設計・ナビゲーション・発見性

### 2.1 オーディエンス別のエントリポイント設計

デザインシステムのユーザーは異なる知識・目的を持つ。LogRocket によるベストプラクティス:

| ユーザー層 | 求めるもの | エントリポイント設計 |
|-----------|-----------|---------------------|
| デザイナー | パターン、バリアント、アセット、トークン | Figmaライブラリリンク、ビジュアルブラウザ |
| エンジニア | コードスニペット、APIリファレンス、アクセシビリティ要件 | npm/コードファーストナビゲーション |
| プロダクトマネージャー | デザイン根拠、実現可能性ノート、ビジネスロジック | 原則・パターンのユースケース |
| 新メンバー | クイックスタート、チュートリアル | Getting Started ページ |

### 2.2 標準的なナビゲーション構造

```
/ Getting Started(クイックスタート)
/ Foundations(基盤)
  / デザイン原則
  / デザイントークン(カラー、スペーシング、タイポグラフィ...)
  / アクセシビリティ
  / Voice & Tone
/ Components(コンポーネント)
  / [各コンポーネント]
/ Patterns(パターン)
/ Content Guidelines(コンテンツガイドライン)
/ Contributing(貢献ガイド)
/ Changelog
/ Resources & Support
```

### 2.3 検索性・発見性の確保

- 全文検索インデックス + タグ・カテゴリによるメタデータ管理
- コンポーネント名の一貫した命名(コードとデザインで同じ名前)
- パーマリンク(アンカーURL)でコンポーネント固有ページに直リンク可能にする
- サイドバーナビゲーションに加え、ページ内目次を設置

---

## 3. ドキュメンテーションのライティング原則・メンテナンス性

### 3.1 ライティング原則

- **シンプルかつ機能的**: 抽象的な理論より実用的な例を優先
- **一貫した構造**: 全コンポーネントで同じテンプレートを使用
- **コンテキスト提供**: なぜそうするか(rationale)を必ず書く
- **パターン名はコンテキスト非依存に**: 再利用性の最大化
- **全ディシプリンを包含**: エンジニアだけでなくデザイナー・PM向けの文章も含む

### 3.2 メンテナンス性のための仕組み

- **ドキュメント所有権の明確化**: 誰が最終責任を持つかを定義する
- **変更時に必ずドキュメントも更新**: リリースプロセスに組み込む
- **定期監査**: 定期的にドキュメントの正確性・最新性をチェック
- **新メンバーをテスターとして活用**: オンボーディングでドキュメントのギャップが露出する
- **フィードバックチャネルの設置**: Slackチャンネル、GitHub Issues等

---

## 4. Docs-as-Code とツール比較

### 4.1 Docs-as-Code の考え方

Docs-as-Code は、ドキュメントをコードと同じワークフロー(Git、PR、CI/CD)で管理するアプローチ。主なメリット:

- バージョン管理とトレーサビリティ
- コードと同時にドキュメントをリリース
- レビューワークフローに組み込める
- エンジニアが自然に貢献しやすい

### 4.2 ツール比較

| ツール | 強み | 弱み | 最適なチーム |
|-------|------|------|------------|
| **Storybook + MDX** | コードと密結合、インタラクティブデモ、バージョン管理 | 非エンジニアには使いにくい、コードリポジトリアクセスが必要 | エンジニア主導のチーム |
| **Zeroheight** | Figma/Storybookと統合、WYSIWYG編集、バージョン管理、AI生成リリースノート | コスト、カスタマイズ制限 | クロスファンクショナルチーム |
| **Backlight** | MDX/MDVue等広範な技術対応、Git統合、コンポーネントとドキュメントの同期 | 学習コスト | 開発者中心チーム |
| **カスタムサイト** | 完全な制御、Storybookをiframeで埋め込み可能(Shopify Polaris等) | 構築・維持コストが高い | 大規模専任チーム |
| **Notion/Confluence** | 非技術者が編集しやすい、Chromatic/Storybookを埋め込み可能 | バージョン管理弱い、コードとの分離 | 小規模・混成チーム |

**実績例**:
- Shopify Polaris: カスタムサイト + Storybook + Figma Libraries の三位一体
- Intuit・Instacart: Zeroheight を採用
- IBM Carbon・Workday Canvas: カスタムドキュメントサイト + Storybook iframes

### 4.3 Storybook の4つのドキュメント手法

1. **Docs addon**: 既存storiesから自動生成、ArgsTable、MDXカスタマイズ
2. **カスタムdocsサイトへのStorybook埋め込み**: 完全制御、高メンテコスト
3. **Notion/Confluence + Chromatic**: 非技術者包含、簡易実装
4. **Design System Manager連携**(Zeroheight等): トークン管理、設計ツール統合

---

## 5. バージョニング・Changelog・Deprecation

### 5.1 セマンティックバージョニング (SemVer)

```
MAJOR.MINOR.PATCH
例: 2.1.3
```

| バージョン | 内容 | 対応 |
|----------|------|------|
| PATCH (1.0.x) | バグ修正のみ | 自動適用可 |
| MINOR (1.x.0) | 新コンポーネント追加、後方互換 | 任意アップグレード |
| MAJOR (x.0.0) | 破壊的変更 | 移行ガイド必須 |

### 5.2 Changelog のベストプラクティス

- 最上部に**2文以内の要約**を配置
- 変更の種類(追加・修正・非推奨・削除)を明示
- 影響を受けるコンポーネント名と旧APIを記載
- マイグレーションガイドへのリンクを必ず添付
- すべての変更に出典(PR、Issue番号)を含める

Zeroheight ではバージョン間のブラウズ機能と、AIによるリリースノート自動生成をサポート。

### 5.3 Deprecation の進め方

ProductRocket の推奨プロセス(最低3ヶ月のタイムライン):

1. **告知**: Changelog、Slack、ドキュメントで deprecated と明示。代替コンポーネントを示す
2. **コード上のマーク**: `@deprecated` JSDocアノテーション、コンソール警告
3. **積極的なアウトリーチ**: まだ使用しているチームに個別連絡し、ブロッカーを把握
4. **削除**: major バージョンでのみ実行。移行ガイドとセットで

---

## 6. ガバナンスモデルの類型

### 6.1 三つのモデル概要

Nathan Curtis (EightShapes) の "Team Models for Scaling a Design System" および Zeroheight の調査をもとに整理。

#### Centralized(中央集権型)

```
専任DS チーム
    ↓ 配布
製品チームA  製品チームB  製品チームC
```

| 利点 | 欠点 |
|------|------|
| 一貫性が高い | 製品チームのコンテキスト不足 |
| 意思決定が速い | 採用促進に主体性が必要 |
| 品質管理しやすい | ボトルネックになりやすい |
| 広い製品ポートフォリオに対応 | 製品チームの参加が受動的になりがち |

**適合条件**: 統一されたブランド体験が必要な多プロダクト組織、デザインシステムへの専任投資が可能な組織

#### Federated(連邦・分散型)

```
製品チームA  製品チームB  製品チームC
  (DSメンバー) (DSメンバー) (DSメンバー)
      ↓         ↓          ↓
       [共有意思決定・共同構築]
```

| 利点 | 欠点 |
|------|------|
| 複数プラットフォームの正当性 | 意思決定が複雑・遅い |
| 多様なコンテキストを反映 | 製品優先でDS作業が後回しになりやすい |
| 採用エバンジェリストが各チームに存在 | 決定の記録・追跡が困難 |
| 偏りの認識が少ない | 強い横断コミットメントが必要 |

**Curtis の警告**: 「中央集権的なコア担当者なしに連邦モデルだけでは、デザインシステムは死んだように見える」

**適合条件**: 製品間の文脈差が大きい、組織が地理的・事業的に分散している

#### Hybrid(ハイブリッド型)

```
コアDSチーム(少人数)
    ↕ 連携・調整
各製品チームの DSコントリビューター
```

| 利点 | 欠点 |
|------|------|
| 安定した基盤 + 現場知見の両立 | 役割定義が難しい |
| スケーラブル | コミュニケーションコスト |
| 採用しやすい | 優先順位の衝突 |

**Zeroheight 調査(2025)**: 高採用システムの **68% がハイブリッドモデルを採用**。

### 6.2 ガバナンスの成熟度

Curtis が提唱する4段階:

| ステージ | 体制 | 特徴 |
|---------|------|------|
| Stage 1 | Spare Timers | 個人がスペア時間でDS作業、スケールしない |
| Stage 2 | Allocated Individuals | 10〜25%の時間を割り当て、断片的なアウトプット |
| Stage 3 | System Team-as-Product Team | 正式な多職種チーム、製品チームにサービス提供 |
| Stage 4 | System Team of Teams | 企業規模の複数チーム体制(大企業のみ) |

---

## 7. 貢献モデル(Contribution Model)

### 7.1 貢献できる対象の分類

| カテゴリ | 例 |
|---------|---|
| 新コンポーネント | 新しいUIパーツの提案・実装 |
| バグ修正 | 既存コンポーネントの不具合修正 |
| Figma修正 | デザインファイルの更新 |
| ドキュメント更新 | 説明文・例の追加・修正 |
| トークン追加 | 新しいデザイン変数の提案 |

**組織別の制限例**:
- **Atlassian**: バグ修正・Figma修正・ドキュメント更新のみ受け付け
- **GOV.UK**: 「有用性・独自性の証明」が必要
- **Zalando/Nord Health**: 軽・中・重の三段階でカテゴリ分類

### 7.2 貢献プロセスの5ステップ

1. **初期評価**: 提案の種別確認、システムレベルか製品固有かの判断。既存トークン・コンポーネントとの整合性チェック
2. **計画**: 提出プロセスの概要、内部レビュー手順の説明
3. **プレゼンテーション**: ステークホルダーへのフィードバックセッション(週次または隔週)
4. **設計・協働**: デザイナーとエンジニアの分担、使用ツールの確認
5. **承認**: レビューサイクル数と意思決定者の明確化

### 7.3 貢献提案のテンプレート(4問)

貢献プロセスの書類負担を下げるため、シンプルな4問フォーマットが有効:

1. これはどんな問題を解決するか?
2. 何チームがこれを必要としているか?
3. 提案されるソリューションは何か?
4. 類似のものが既に存在するか?

### 7.4 レビュー基準

- トークンのみ使用(ハードコーデッドな値を含まない)
- WCAG 2.2 AA以上のアクセシビリティ適合
- 完全なドキュメント添付
- 命名規則への準拠

**レスポンスタイム目標**: 最大10営業日。即時確認 → 1週間以内に初期フィードバック → 2週間以内に最終決定

### 7.5 組織規模別の課題

Zeroheight 調査によると、貢献への満足度は組織規模が大きくなるほど低下:
- 500〜999名: 貢献に満足しているのは28%
- 1000名以上: 32%
- 500名超の組織では、全スタッフの約1%しかアクティブに貢献していない

---

## 8. 所有権(Ownership)とチーム構成

### 8.1 所有権の原則

「全員が所有する = 誰も所有しない」という失敗パターンを避けるため、**単一の System Owner** を置くことが重要。

System Ownerの責務:
- デザインシステムのロードマップ所有
- バックログの優先順位付け
- チーム内の意見対立時の最終決定
- 採用状況のモニタリング
- 外部への説明責任

### 8.2 理想的なコアチーム構成(Stage 3)

**最低限必要**:
- デザイナー1名(ビジュアルデザイン、インタラクション、情報設計)
- エンジニア1名(フロントエンド、HTML/CSS、ツール構築)

**あると良い**:
- プロダクトマネージャー(ロードマップ管理、バックログ整理、採用モニタリング、リリース管理)

**状況に応じて**:
- コンテンツストラテジスト、アクセシビリティ専門家

**平均チームサイズ(Zeroheight 2025)**:
- 100名以下の組織: 平均3名
- 100〜499名: 平均4名
- 500名以上: 平均9名

### 8.3 ハーフタイムモデル

Curtis は「全メンバーがDS作業に半分の時間を割き、3〜5の製品チームにも関わり続ける」ハーフタイムモデルを推奨。メリット:

- 単一製品への偏見を防ぐ
- 実際のニーズへの可視性を維持
- 製品チームとの信頼関係構築

### 8.4 専門ロールの責務分界

| ロール | 主な責務 |
|-------|---------|
| デザインシステムリード | 全体戦略、原則、ビジョン管理 |
| System Owner / PM | ロードマップ、バックログ、採用追跡 |
| DSデザイナー | コンポーネントデザイン、トークン定義、Figmaライブラリ管理 |
| DSエンジニア | コンポーネント実装、APIデザイン、コードレビュー |
| Contribution Lead | 貢献の受け入れ判断、提案の評価 |
| Adoption Champion | 各製品チームへの普及、ギャップのフィードバック |
| QAエンジニア | 品質基準への適合確認 |

---

## 9. 採用(Adoption)の促進と計測

### 9.1 採用に影響する主要因子

Zeroheight 調査の「ガバナンスのパラドックス」: ガバナンスの欠如が採用されない強い理由になる一方、**採用が高いチームの4分の3はガバナンスについて言及しない**、という解釈が同レポートから引用されている。これは「ちょうど良いガバナンス」を見つけることが鍵であることを示唆するが、レポートの原文から同数値を直接確認する必要がある。

採用に最も相関する要素:
1. **コミュニケーション・教育**: ドキュメントだけでなく、研修・ワークショップ・オフィスアワー
2. **専任チームの存在**: 79%が専任チームを持ち、前年比7%増
3. **リーダーシップの支持**: エグゼクティブからの明示的なサポート
4. **抵抗最小化**: 使いやすいパッケージ構造、コピー可能なコードスニペット

### 9.2 採用メトリクス

| 指標 | 内容 | 測定方法 |
|-----|------|---------|
| **コンポーネントカバレッジ** | システムのコンポーネントを使って構築されたUIの割合 | Figma Analytics, Supernova |
| **detachment rate** | コンポーネントをデタッチして使っている割合 | Figma Analytics |
| **ライブラリ利用率** | ライブラリのコンポーネント・変数・スタイルの利用状況 | Figma Library Analytics(2025年更新) |
| **ドキュメントの使われ方** | ドキュメントページへのアクセス・検索クエリ | Analytics ツール |
| **サポート量の推移** | 「このコンポーネントの使い方は?」という質問数 | Slackチャンネル |
| **時間効率** | DS使用者と非使用者のタスク完了時間の比較 | 実験・調査 |

**ベンチマーク(Supernova 2024と記載されているが出典の詳細不明)**: コンポーネントカバレッジ70%超の組織は、50%以下の組織より40%速く新機能をリリース、という数値が流通しているが、一次出典の調査方法が確認できていないため参考値として扱われたい。

**Figma のデータ(Figma社内実験、2025)**: Figma自社のデータサイエンスチームが実施した実験では、デザインシステムを使用したデザイナーはそうでないデザイナーより34%速くタスクを完了。ただしFigma自身が「この数値はデザインシステムが当該タスクに直接適合していた理想的条件下での上限値であり、実際の現場環境での効果は小さくなる可能性がある」と明記している(ベンダー自社実験の数値として解釈が必要)。

### 9.3 採用を路線とする戦略

- 設定が少ないパッケージ構造
- 全コンポーネントにコピー可能なコードスニペット
- 専用サポートチャンネル(Slack等)
- リリースノートへのコントリビューター名掲載
- **Zeroheight 調査(2023年版、ProductRocket記事経由)**: 専用ドキュメントサイトを持つシステムは72%の採用率、Figmaファイルのみは31%(Zeroheight 2025レポートでの同数値の確認はできていない)

---

## 10. デザインと開発の連携(Figma ↔ コードの同期)

### 10.1 デザイントークンのSync戦略

W3C Design Tokens仕様(Design Tokens Community Group)が2025年10月に初の安定版(バージョン表記: 2025.10)に到達。W3Cの発表によると、Figma・Penpot・Sketch・Framer・Supernova・zeroheight等10以上のデザインツール・OSプロジェクトが「すでに対応しているか、標準を実装中(already support or are implementing the standard)」とされており、各ツールの対応程度の差異は同発表では明示されていない。ツール間のトークンファイルのポータビリティが向上しつつある。

主な同期パターン:

```
Figma Variables
    ↓ (Figma REST API または GitHub Action)
JSON トークンファイル (W3C Design Tokens形式)
    ↓ (Style Dictionary 等)
CSS Custom Properties / JS/TS 定数 / iOS/Android 変数
```

Figma 公式: デザイナーがVariablesを更新 → GitHub Actionが自動でPRを作成 → エンジニアがレビュー・マージ

最も発展している自動化: デザイン→コードへの同期 (CI/CDパイプライン)
最も未発達: 双方向同期 (コード→デザイン)

### 10.2 Figma Code Connect

Code Connect により、Figmaコンポーネントをプロダクションコードの対応物にリンク。開発者がDev Modeで見るのは「AIが推測したコード」ではなく「実際のプロダクションコードスニペット」になる。

「Code Connectなしでは、AIモデルはコンポーネントの使い方を推測するだけ」— LogRocket

### 10.3 Figma Dev Mode とハンドオフ

- **Figma Dev Mode**: エンジニア向けに設計された読み取りビュー。コンポーネントのプロパティ、変数値、コードスニペットをそのまま確認可能
- **Check Designs**: デザイナー向けリンタ。変数の適切な使用を自動チェックし、ハンドオフ前の品質保証を実現(Figma 2025)
- **Extended Collections**: 多ブランドシステムで親システムの更新を継承しながらカスタムオーバーライドが可能

---

## 11. 意思決定の記録 — ADR のデザインシステムへの応用

### 11.1 ADR(Architecture Decision Records)とは

ソフトウェアエンジニアリングで確立した「アーキテクチャ決定の記録」手法。デザインシステムに応用することで「なぜこのコンポーネントはこの形になったのか」「なぜこのトークンを選んだのか」を追跡可能になる。

ADRの構造:

```markdown
# ADR-001: Buttonコンポーネントのバリアント設計

## 状態: 承認済み
## 日付: 2026-06-05

## 背景
なぜこの決定が必要だったか

## 決定
何を決めたか

## 根拠
なぜこの選択肢を選んだか

## 結果
この決定がもたらすトレードオフ

## 代替案
検討した他の選択肢と棄却理由
```

### 11.2 デザインADRのユースケース

- コンポーネントの追加・削除の判断根拠
- トークンアーキテクチャの設計選択
- ガバナンスプロセスの変更
- サードパーティツールの採用判断
- アクセシビリティ基準のレベル選択

**保存場所**: コードリポジトリの `docs/decisions/` または `design-decisions/`。コードと同じバージョン管理下に置く。

**重要**: ADRは一度承認したら変更しない(変更時は新しいADRを作成)。これにより意思決定の歴史が保全される。

---

## 12. AI時代のドキュメント設計

### 12.1 AI エージェントが文書を参照する前提での留意点

2025〜2026年のトレンドとして、Figma MCP・Cursor・Claude Code等のAIコーディングエージェントがデザインシステムのドキュメントを直接参照して実装する場面が増えている。

#### 機械可読性の確保

| 要素 | 人向け | AI向け |
|-----|-------|-------|
| コンポーネント情報 | 散文的な説明文 | 構造化JSON/YAMLメタデータ |
| トークン | Figmaでの視覚的確認 | W3C形式JSONエクスポート |
| コンポーネント使用例 | スクリーンショット | Code Connect でリンクされたコードスニペット |
| 命名規則 | ページで説明 | システマティックで一貫した命名 |

#### AIがコンテキストを消化できる設計原則

1. **コンテキスト分割**: AIエージェントに一度に与えるコンテキストが多すぎると「コンテキスト汚染」が発生。コンポーネントごとの独立したドキュメント単位が重要
2. **明確なエントリポイント**: AIエージェントが「どこから読み始めるか」を理解できる構造化インデックス(CLAUDE.md や .cursorrules等)
3. **曖昧さの排除**: 「適切な場合に使用」等の曖昧な表現を避け、具体的なユースケースを列挙
4. **Code Connect の徹底**: AIが推測でなく正確なコンポーネント実装を生成できるよう、Figmaコンポーネントとコードを紐付ける
5. **機械可読メタデータ**: コンポーネントのprops、バリアント、状態を構造化データで定義

#### Figma MCP のインパクト

Figma MCPサーバー(2025年GA)により、AIコーディングエージェントはFigmaの変数・デザイントークン・コンポーネント・バリアント・AutoLayout規則を直接読み取って実装に使用できる。

Figma Make(Config 2025発表): Anthropic のClaudeを使い、デザインからReactコード(Shadcn UI)を生成。デザインシステムの「Make kits」と「npmパッケージのインポート」でOSS設計の利用も可能。

#### LLM を活用したドキュメント自動生成の報告事例

- 構造化入力(サイズ、ステート、カラートークン等を分割)→ LLM処理 → 人間レビューのワークフローで、ドキュメント速度85%向上、命名一貫性95%、開発者の質問70%削減と報告されている(Yuti Vora, 2025, Medium個人記事。単一実践者の自己報告であり再現性は未検証)
- **重要**: LLMはブランドコンテキストや暗黙の慣習を理解できない。人間レビューは必須

---

## 13. 実際のデザインシステムの事例

### Shopify Polaris

- **ドキュメント構造**: 原則 → トークン → コンポーネント(使用例、コードスニペット、a11y) → パターン
- **ガバナンス**: 専任チームによる集権的管理 + セマンティックバージョニング + 移行ガイド必須
- **設計-コード同期**: Figmaライブラリ + Storybook的コンポーネントエクスプローラー + CI自動テスト(アクセシビリティ、ビジュアルリグレッション)
- **2025**: Web Componentsベースに統一し、フレームワーク非依存を実現

### Atlassian Design System

- **貢献モデル**: バグ修正・Figma修正・ドキュメント更新のみ受け付け(非常に厳格)
- **ガバナンス**: 中央集権型。専任チームが最終判断

### GOV.UK Design System

- **貢献基準**: 「有用性」と「独自性」の証明が必要。誰でも提案可能
- **ガバナンス**: オープン + 専任チームによる審査

---

## 出典

| タイトル | URL |
|---------|-----|
| Team Models for Scaling a Design System — Nathan Curtis / EightShapes | https://medium.com/eightshapes-llc/team-models-for-scaling-a-design-system-2cf9d03be6a0 |
| Designing a Systems Team — Nathan Curtis / EightShapes | https://medium.com/eightshapes-llc/designing-a-systems-team-d22f27a2d81d |
| Maintaining Design Systems — Brad Frost, Atomic Design | https://atomicdesign.bradfrost.com/chapter-5/ |
| Design System Documentation Spec (DSDS 0.2.1 Draft) | https://designsystemdocspec.org/ |
| How to govern a design system that is continuously evolving — designsystems.com | https://www.designsystems.com/how-to-govern-a-design-system/ |
| Design Systems Report 2025 (How We Document) — Zeroheight | https://zeroheight.com/how-we-document/ |
| Design System Governance: who owns what — ProductRocket | https://productrocket.ro/articles/design-system-governance/ |
| Design System Contribution Model — UXPin | https://www.uxpin.com/studio/blog/design-system-contribution-model/ |
| Design System Governance — UXPin | https://www.uxpin.com/studio/blog/design-system-governance/ |
| Making Metrics Matter (Design Systems 104) — Figma Blog | https://www.figma.com/blog/design-systems-104-making-metrics-matter/ |
| Schema 2025: Design Systems For A New Era — Figma Blog | https://www.figma.com/blog/schema-2025-design-systems-recap/ |
| Design System Annotations, Part 1 — GitHub Blog | https://github.blog/engineering/user-experience/design-system-annotations-part-1-how-accessibility-gets-left-out-of-components/ |
| 4 Ways to Document Your Design System with Storybook | https://storybook.js.org/blog/4-ways-to-document-your-design-system-with-storybook/ |
| MDX in Storybook — Storybook Docs | https://storybook.js.org/docs/writing-docs/mdx |
| Best Tools for Design System Documentation — Backlight/Design System Mastery | https://backlight.dev/mastery/best-tools-for-design-system-documentation |
| Should you document in Storybook? — Zeroheight | https://help.zeroheight.com/hc/en-us/articles/36473744204955-Should-you-document-your-design-system-in-Storybook |
| Automating Design Systems with LLMs — Medium / Design Bootcamp | https://medium.com/design-bootcamp/automating-design-systems-with-llms-how-ai-helped-me-scale-component-documentation-across-df4951a7ddfc |
| Your Design System Documentation Should Be Written for Both AI Agents and Humans — Design Systems Collective | https://www.designsystemscollective.com/your-design-system-documentation-should-be-written-for-both-ai-agents-and-humans-3519fb712c52 |
| Dear LLM, here's how my design system works — UX Collective | https://uxdesign.cc/dear-llm-heres-how-my-design-system-works-b59fb9a342b7 |
| How to structure Figma files for MCP and AI-powered code generation — LogRocket | https://blog.logrocket.com/ux-design/design-to-code-with-figma-mcp/ |
| Design Tokens: How to Sync Design and Code in Figma | https://www.figma.com/resource-library/design-tokens/ |
| The Ultimate Guide to Syncing Design Tokens: Figma to GitHub — Replay Blog | https://www.replay.build/blog/the-ultimate-guide-to-syncing-design-tokens-figma-to-github-automation |
| Shopify Polaris Design System Analysis — Upuply | https://www.upuply.com/blog/shopify-design-system |
| Tips for design system documentation you'll actually use — LogRocket | https://blog.logrocket.com/ux-design/design-system-documentation/ |
| Measuring Design System Adoption in Figma — Medium | https://medium.com/@davidveraco/measuring-design-system-adoption-in-figma-tools-comparisons-and-best-practices-c1783fc2c06b |
| Design System Adoption Plugin — Figma Community | https://www.figma.com/community/plugin/1403120879057576319/design-system-adoption |
| Architecture Decision Records — adr.github.io | https://adr.github.io/ |
| Architecture Decision Record — Martin Fowler's bliki | https://martinfowler.com/bliki/ArchitectureDecisionRecord.html |
| Information Architecture checks for Design Systems — Medium | https://medium.com/design-bootcamp/information-architecture-checks-for-design-systems-a3d30082f05a |

---

## このリポジトリへの示唆

WhoOwnsDesign リポジトリの「AIを使ってデザインシステムによるWebアプリケーションを構築するときに、AIやチームに何を・どの粒度で・どの形式で用意すべきか」というテーマに対して、本調査から以下の示唆が得られる。

### 1. ドキュメントは「人間+AI」の二層構造で設計する

2025〜2026年時点で、デザインシステムのドキュメントは**人間可読性**と**機械可読性**の両方を備える必要がある。人間向けには説明的な文章・Do/Don't・背景、AI向けには構造化JSON/YAML・コンポーネントメタデータ・一貫した命名規則。Figma Code Connect とW3C Design Tokens形式の徹底採用がその実装手段となる。

### 2. コンポーネントドキュメントの最小必須セットを決める

DSDS 0.2.1 Draft とNathan Curtis の4要素を参考に、このリポジトリでも「コンポーネント1件に必ず含めるべき項目」のテンプレートを定義する。最低限: 目的・アナトミー・バリアント・状態・Do/Don't・アクセシビリティ・APIリファレンス・インタラクティブ例。

### 3. ガバナンスモデルを明示的に選択・宣言する

"誰がデザインを所有するか"というリポジトリのコアテーマに直結して、Centralized/Federated/Hybridのどれを採用するかを明示的に決定し、ADR形式で記録する。特に「誰がコンポーネントの追加・変更・廃止を最終承認するか」のエスカレーションパスを文書化することが重要。

### 4. 貢献プロセスを「使いやすい入口」として設計する

最も重要な採用促進要因は「貢献がシステムを迂回するより簡単であること」。提案テンプレートの4問化・10営業日以内の応答目標・軽/中/重の分類など、WhoOwnsDesign でも貢献フローのフリクション最小化を設計原則として採用すべき。

### 5. ADR(設計決定の記録)をドキュメント構造に組み込む

なぜそのトークン値を選んだか、なぜそのコンポーネントを廃止したか、なぜそのガバナンスモデルを採用したかを追跡可能にするため、`design-decisions/` ディレクトリにADR相当の記録を蓄積する構造を検討。これはAIエージェントが「過去の意思決定の文脈」を理解するためにも有効。

### 6. Figmaファイル構造をAI消費可能な形に整える

Figma MCPが一般化した今、Figmaのレイヤー命名・コンポーネント構造・変数定義がそのままAIエージェントへのコンテキストになる。WhoOwnsDesign の定義する「用意すべきもの」の中に、Figmaファイル構造のガイドライン(命名規則・AutoLayout徹底・Code Connect設定)を含める必要がある。

### 7. 採用状況のメトリクス設計を最初から行う

「使われているかどうか分からないデザインシステム」は維持モチベーションを失いやすい。コンポーネントカバレッジ・detachment rate・ドキュメントアクセス数・サポート量の推移を最初から計測できる仕組みを設計に含める。Figma Library Analytics(2025年更新)を活用できる。
