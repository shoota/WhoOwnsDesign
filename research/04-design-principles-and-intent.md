---
title: デザイン原則と「意図」の言語化
topic: design-principles-intent-verbalization
date: 2026-06-05
status: research-log
---

# デザイン原則と「意図」の言語化

## 1. デザイン原則(Design Principles)とは何か

デザイン原則とは、チームがデザインに関する意思決定を一貫して行うための **価値観の表明(value statements)** である。Nielsen Norman Groupは「デザイン原則はトレードオフを扱う際に最も重要な目標を説明する価値の表明」と定義する。

原則はルール(rules)とは異なる。ルールは「何をすべきか」を規定するのに対し、原則は「なぜそうするのか」という判断の基軸を与える。Center Centre の記事が指摘するとおり、強い原則は「ノーと言う手助けをする」ものである。

---

## 2. 良い原則の条件

### 2.1 NN/Gが定義する要件
- **明確な立場をとる**: 曖昧さなく、どちらを優先するかが伝わること
- **ユーザーへの影響と接続されている**: なぜその価値が重要かが説明されている
- **簡潔で記憶できる**: エッセイではなく、一文〜数文で伝わること
- **内部矛盾がない**: 原則同士が相反しないこと

### 2.2 Clearleft(Katie Wishlade)の5基準
1. **Purposeful(目的的)**: 誰が・どう使うかを先に決める
2. **Authentic(自社固有)**: 競合のコピーではなく自分たちの文脈から生まれている
3. **Actionable(行動可能)**: 経験の「感触」を表現し、何を支持し何を否定するかが判断できる
4. **Iterated(反復的)**: 複数ラウンドのフィードバックを経て洗練されている
5. **Maintained(管理されている)**: オーナーが明確で、生きた文書として維持されている

### 2.3 Matthew Ström の4テスト
1. **記憶できる**: 「Good design is as little design as possible」(Dieter Rams)
2. **ノーを言う力がある**: 「One primary action per screen」(Joshua Porter)
3. **裏返しテスト(Reversibility Test)**: 反対が合理的に存在しないなら原則ではない。「Make users happy」はトリヴィアル。
4. **広く適用できる**: 「Use an 8 pixel grid」はUIにしか使えず弱い

### 2.4 Even Over フォーマット
強い原則の表記法として有効な形式:

| 優先すること | even over | 犠牲にするもの |
|---|---|---|
| Accessibility | even over | Aesthetics |
| Platform conventions | even over | Cross-platform consistency |
| User preference | even over | Business preference |

### 2.5 Center Centreの6つの逆説的テスト
1. 実際のユーザーリサーチから直接生まれているか
2. 以前の設計方向の約2/3を否定できるか
3. 競合と自分たちを区別できるか
4. 将来のリリースで逆転できるほど文脈依存か
5. このプロジェクトで再評価されているか
6. 意味を継続的に実際のデザインでテストしているか

---

## 3. 有名なデザイン原則の実例と比較分析

### 3.1 Dieter Ramsの10原則(1970年代)

工業デザイナーが「良いデザインとは何か」と問いかけ生まれた普遍的な基準。

| 番号 | 原則 | 要点 |
|---|---|---|
| 1 | 革新的である | 技術進歩と並走し、革新のための革新を避ける |
| 2 | 有用である | 機能・心理・審美すべての有用性を果たす |
| 3 | 美しい | 美しさは有用性の一部。使われ続けるものだけが美しい |
| 4 | わかりやすい | 構造を明示し、理想的には説明不要にする |
| 5 | 邪魔しない | ツールのように中立で控えめ。自己表現の余地を残す |
| 6 | 誠実である | 製品を実際以上に見せない。誇大な約束をしない |
| 7 | 長持ちする | 流行に流されず、長期的に価値を保つ |
| 8 | 細部まで徹底する | 偶然の要素を持ち込まない。ユーザーへの敬意 |
| 9 | 環境に優しい | 資源を節約し、廃棄物と汚染を最小化する |
| 10 | できる限り少なく | Less but better。不要な要素を排除し本質に集中 |

**評価**: 非常に普遍的だが、「デジタルUI判断に直接使えるか」という観点ではやや抽象的。Jony Iveがこれを基にAppleの美学を形成したことで、デジタルへの変換が証明されている。

---

### 3.2 Jakob Nielsenの10ヒューリスティクス(1994)

「経験則(heuristics)」であり、任意のUIに評価軸として適用可能。

| 番号 | ヒューリスティクス | 説明 |
|---|---|---|
| 1 | システム状態の可視性 | ユーザーが今何が起きているか常に把握できる |
| 2 | 現実との一致 | システムが日常言語で動作する |
| 3 | ユーザーのコントロールと自由 | 間違いから即座に抜け出せる |
| 4 | 一貫性と標準 | 同じ言葉・状況・行動が同じ意味を持つ |
| 5 | エラーの予防 | 問題の発生そのものを防ぐ |
| 6 | 記憶より認識 | オプション・行動・情報を可視化してメモリ負荷を減らす |
| 7 | 柔軟性と効率 | 熟練者向けショートカットを備えつつ初心者にも対応 |
| 8 | 審美的でミニマルなデザイン | 関連性のない情報を排除 |
| 9 | エラーからの回復を助ける | 平易な言葉でエラーを説明し解決策を示す |
| 10 | ヘルプとドキュメント | 必要な時に文脈に沿ったヘルプが見つかる |

**評価**: 評価軸として優れているが、「どう作るか」ではなく「どう評価するか」の基準。原則というよりチェックリスト。

---

### 3.3 GOV.UK デザイン原則(2012)

GDS(Government Digital Service)が策定。2012年に公開、現在11原則。

| 原則 | 要点 |
|---|---|
| 1. ユーザーニーズから始める | 推測ではなく調査から。ニーズがわからなければ正しいものは作れない |
| 2. より少なくする | 必要なコアに集中。既存の解に連携する |
| 3. データで設計する | データが意思決定を導く。ヒラメキや推測ではない |
| 4. シンプルにするための苦労を惜しまない | 「Always been this way」を受け入れない |
| 5. 繰り返し、また繰り返す | 小さなリリースとテストでリスクを低減。失敗を学びに |
| 6. 全員のために | アクセシブルな設計が良い設計。「サービスを最も必要とする人が最も使いにくい」 |
| 7. 文脈を理解する | スマホ・図書館・不慣れな環境など多様な文脈を想定 |
| 8. ウェブサイトではなくサービスを作る | リアルなニーズへの統合されたソリューション |
| 9. 一貫性を、画一性ではなく | 言語・パターンの一貫性と、文脈への適応を両立 |
| 10. 公開する。その方が良くなる | コード・設計・アイデアの共有が品質を高める |
| 11. 環境負荷を最小化する | サービスのライフサイクル全体で環境への影響を考慮 |

**評価**: 政府サービスという特殊な文脈に根ざしつつ普遍性を持つ。「ユーザーニーズから始める」は最も引用される原則の一つ。

---

### 3.4 Apple Human Interface Guidelines(HIG)

3つのコア原則で全Appleプラットフォームを貫く。

| 原則 | 説明 |
|---|---|
| **Clarity(明確さ)** | テキストは読みやすく、アイコンは精確で明瞭、装飾は最小限。全ての要素がコミュニケーションに奉仕する |
| **Deference(敬意)** | UIはコンテンツの邪魔をしない。ユーザーの写真・メッセージ・作業が前面に立つ |
| **Depth(奥行き)** | 視覚的な層と現実感ある動きが階層とナビゲーションを伝える |

**評価**: 高度に凝縮されており記憶しやすい。しかし「なぜその判断か」の橋渡しには追加のガイドラインが必要。

---

### 3.5 Shopify Polaris 体験価値(Experience Values)

6つの価値で全Shopifyエクスペリエンスを統一。

| 価値 | 要点 |
|---|---|
| **Considerate(思いやり)** | 全スクリーン・言語・文化に配慮したアクセシブルな体験 |
| **Empowering(力を与える)** | ユーザーが自律的にタスクを完遂できる |
| **Crafted(職人的)** | 複雑な問題を明確かつ親しみやすく解くプロの仕事 |
| **Efficient(効率的)** | 最小限の摩擦で目標に到達。自動化と段階化 |
| **Trustworthy(信頼できる)** | 細部への注意と透明性で継続的な信頼を構築 |
| **Familiar(親しみやすい)** | 一貫したパターンと直感的な動作で安心感を提供 |

---

### 3.6 Medium 初期デザイン原則

トレードオフを明示した原則の好例。

| 原則 | 意味 |
|---|---|
| **Direction over Choice(選択より方向性)** | レイアウト・書体・色の選択肢を意図的に削り、書くことへの集中を優先した |
| **Appropriate over Consistent(一貫性より適切さ)** | OS・デバイス・文脈により一貫性を破ることも辞さない |
| **Evolving over Finalized(完成より進化)** | コンテンツは使用で成長し、「印刷物」ではなく「生き物」である |

**評価**: 「even over」形式の先駆け。決定の理由(書くことへの集中)が原則に直接埋め込まれている優れた例。

---

### 3.7 NHS デザイン原則(2018)

医療文脈の特殊性(信頼・包括性・生命への影響)を反映した10原則。主な特徴:
- 「Put people at the heart of everything」から始まる人間中心性
- 「Design for trust」という医療特有の原則
- GOV.UKと共通する「Make things open」

---

### 3.8 Indeed デザイン原則

4原則に絞り込み、150人超へのサーベイとワークショップで策定した優れた事例:
1. Users' biggest advocates
2. Thoughtful consistency
3. Radical collaboration for radical problems
4. Inclusive inside and out

**注目点**: 「以前の原則がUX過ぎて非デザイナーが使えなかった」という反省から、クロスファンクショナルな共同作成プロセスを採用。

---

## 4. 原則から具体的UI判断への橋渡し:階層構造

NN/Gは4層の階層を提示している。

```
Team Charter(チームの目的・役割の合意)
    ↓
Design Principles(価値の表明・戦略的指針)
    ↓
Usability Heuristics(経験則・質の評価軸)
    ↓
Design Patterns(再利用可能な具体的UIソリューション)
```

この階層は競合するものではなく相互補完的。成熟したUX組織は4層すべてを組み合わせる。

### 4.1 原則→ガイドライン→Do/Don'tの展開例

**原則(抽象)**: Efficiency — ユーザーが目標に素早く到達する

**ガイドライン(中間)**: 繰り返しタスクには自動化やショートカットを提供する

**Do**: 頻繁な操作にはキーボードショートカットを設ける  
**Don't**: 毎回同じフォームフィールドを空白で表示する

**Do/Don'tの効果的な書き方**:
- 視覚的なBefore/Afterで示す
- なぜそれが問題か/良いかの理由を1行添える
- 文脈(いつ使うか・使わないか)を示す

---

## 5. ボイス&トーン(Voice & Tone)とコンテンツデザイン

### 5.1 ボイスとトーンの違い

| 概念 | 定義 | 変化するか |
|---|---|---|
| **Voice(ボイス)** | 常に一貫している人格・性格 | 変わらない |
| **Tone(トーン)** | 文脈・相手・状況に応じて変わる感情的色調 | 状況で変わる |

### 5.2 Mailchimp Content Style Guide

**ボイスの4特性**:
1. **Plainspoken**: 装飾を排し、明確さを最優先。バイアスビジネス向けの直接的な言葉
2. **Genuine**: 顧客の苦労を理解し、温かく親しみやすく話しかける
3. **Translators**: マーケティング技術の難解な概念を分かりやすく翻訳する
4. **Dry humor**: ストレートフェイスで、控えめで、少し風変わりな笑い

**5つのライティングゴール**: Empower / Respect / Educate / Guide / Speak Truth

**4つのコンテンツ品質基準**: Clear / Useful / Friendly / Appropriate

**最重要原則**: 「It's always more important to be clear than entertaining」

### 5.3 Shopify Polaris コンテントガイドライン

「言葉はデザインの必須要素。すべての単語・すべてのピリオドが体験にノイズを加える。だからすべての言葉を吟味せよ」

コンテンツ設計を視覚デザインと同等に扱い、マーチャント向けのアクション可能なランゲージ、エラーメッセージの書き方、ヘルプドキュメントまで体系化している。

---

## 6. 用語集(Terminology)の役割

### 6.1 なぜ用語集が必要か

- チーム全体が同じ言葉を使うことで認知的一貫性を保つ
- UXライター・開発者・PMが「同じ概念を同じ言葉で指す」状態を作る
- 禁止語(forbidden alternatives)を明示することで誤用を防ぐ

### 6.2 用語集の構成要素

効果的な用語集には以下が含まれる:
- **推奨語**: 製品内で使う正式な表現
- **禁止語/代替語**: 使用しない類義語と理由
- **定義**: ユーザー目線の簡潔な説明
- **文脈**: どのシーン・コンポーネントで使うか
- **スタイルガイドとの連携**: ボイス&トーンガイドとの統合

---

## 7. デザイン判断の根拠(Rationale)を記録する価値

### 7.1 なぜ「意図」を記録するか

設計決定を記録することの価値:

1. **知識の保全**: 担当者が離れても、なぜその判断をしたかが残る
2. **意思決定の反復防止**: 過去に試して失敗したパターンを繰り返さない
3. **ステークホルダーへの説明**: 判断のロジックが証拠として機能する
4. **オンボーディング加速**: 新メンバーがコンテキストを素早く把握できる
5. **自己認識の向上**: 記録するプロセス自体が思考を整理する

### 7.2 ADR(Architecture Decision Record)的アプローチ

ソフトウェアアーキテクチャの世界で生まれたADR(Architecture Decision Record)の考え方はデザインにも適用できる。

**ADRの要素**:
- **タイトル**: 決定の内容
- **ステータス**: 提案・採択・非推奨・置き換え
- **コンテキスト**: 決定に至った事実・制約
- **決定**: 何を選んだか、なぜ選んだか
- **結果(consequences)**: この決定が生む影響・トレードオフ

**デザインへの応用例**:
```
# DDR: タブナビゲーションをボトムナビに変更

## ステータス: 採択

## コンテキスト
スマートフォン普及によりシングルハンド操作の需要が増加。
上部のタブはiOS SafariのURLバーと競合し誤タップが多発。

## 決定
ボトムナビゲーションバーに移行。

## 結果
+ 親指の届く範囲でナビゲーション可能
- デスクトップ画面での視覚的重心が下がる
```

### 7.3 Pencil & Paperの提言

「デザインラショナール(Design Rationale)は複数の視点を融合するべき」:
- デザイン美学
- ユーザーニーズ
- ビジネス戦略
- 開発実現性

どれか一つの正当化に過度に依存することを避け、複眼的に記録する。

---

## 8. ブランドとデザインシステムの関係

### 8.1 ブランド価値をシステムに落とす経路

```
ブランド価値・ミッション
    ↓(抽象的な信念)
デザイン原則
    ↓(行動可能な指針)
デザイントークン(色・タイポ・スペーシング)
    ↓(機械的な値)
コンポーネント・パターン
    ↓(具体的なUI)
個別の画面・インタラクション
```

### 8.2 デザイントークンの役割

「トークンはブランド価値をコードに翻訳する接着剤」であり、ブランド戦略と技術実装の間の「単一の真実の源泉(Single Source of Truth)」として機能する。

例: Shopifyの「Trustworthy(信頼できる)」という価値
→ 原則「細部への一貫した注意」
→ ガイドライン「ボーダー半径・シャドウを全コンポーネントで統一」
→ トークン `--p-border-radius-base: 0.4rem`

### 8.3 ブランドとデザインシステムの相互依存

- デザインシステムは「全インタラクションがブランド価値を強化」するための制度的インフラ
- ブランドは「なぜこの色か・なぜこのトーンか」の根拠を提供する
- 両者が一致しないと「見た目の一貫性」と「感情的一貫性」が乖離する

---

## 9. 原則のガバナンス:「誰が決めるか」

### 9.1 所有モデルの類型

| モデル | 構成 | 特徴 |
|---|---|---|
| **専任チーム** | デザイナー1+開発者1+PM1(最低) | 最高の品質・一貫性。専任チームで高い満足度(一部調査では2.5倍との報告があるが出典の検証が困難) |
| **ローテーション制** | 四半期ごとにスチュワードが交代。20-30%のキャパシティを割り当て | 専任チームが難しい場合の現実的選択 |
| **全員所有(避けるべき)** | 明確な責任者なし | 実質的に誰も所有しない。決定が滞り貢献が放置される |

### 9.2 原則策定のプロセス

**クロスファンクショナルな共同作成が不可欠**:
- デザイナーだけが決めた原則はエンジニア・PMに無視されやすい
- 全員が策定に参加することで全員が「所有感」を持つ
- Indeed社の事例: 150人超のサーベイ→小グループワークショップ→投票→全社発表

**策定プロセスの4段階**:
1. リサーチ(ユーザー調査、現状のデザインの分析)
2. 共同ワークショップ(価値観の抽出・候補の生成)
3. 絞り込み(トレードオフの特定・言語の洗練)
4. 定着化(ドキュメント・実例への埋め込み・継続的な参照)

### 9.3 Julie Zhuoのビュー

Facebook/Metaのデザインリーダーは「良い原則は物議を醸すべき」と指摘。他の会社が採用しない選択を説明するものでなければ、それは原則ではなくトリヴィアルな真実に過ぎない。

### 9.4 「誰がデザインを所有するか」の核心

設計原則のガバナンスは「WhoOwnsDesign」の核心問題と直結する:

| ステークホルダー | 関与の形 |
|---|---|
| デザイナー | 原則の草案作成・美学的判断の第一責任者 |
| エンジニア | 実装可能性の検証・技術的制約の提示 |
| PM/PO | ビジネス要件との整合・優先順位付け |
| ユーザーリサーチャー | 原則が実際のユーザーニーズに根ざしているか検証 |
| ブランドチーム | ブランド価値との一致を保証 |

「誰かが所有する」のではなく「全員が責任を分有し、明確な意思決定権を持つ担当者がいる」状態が理想。

---

## 10. 原則・意図を機械可読/AI可読にする工夫

### 10.1 AI可読デザインシステムの必要性

Atlassian社の研究が示すように、構造化されていない設計文書はAIが理解できない:
- **AI + 構造化されたADSドキュメント**: 52%の精度向上、34%速度向上、16%トークン削減(Atlassian社の計測値)
- 「システムの構造が一貫しておらず機械可読でなければ、CursorなどのAIツールはそれを理解できない」

### 10.2 DESIGN.md フォーマット(Google Labs)

AIコーディングエージェント向けのデザインシステム記述仕様。

**構造**:
```yaml
---
# YAML frontmatter: 機械可読なデザイントークン
colors:
  primary: "#1a1a2e"
  background: "#ffffff"
typography:
  body:
    fontFamily: "Inter"
    fontSize: "16px"
rounded:
  sm: "4px"
  md: "8px"
---

## Design Philosophy
[Markdown: 人間可読なデザインの意図・理由・コンテキスト]

## Components
[コンポーネントの意図・使用文脈・アンチパターン]
```

**3層のAI命令ファイル体系**:

| ファイル | 役割 | 形式 |
|---|---|---|
| `AGENTS.md` / `CLAUDE.md` | 振る舞い・制約・文脈 | 人間可読なMarkdown |
| `SKILL.md` | 再利用可能なタスク手順 | YAML + Markdown |
| `DESIGN.md` | 視覚的アイデンティティ | YAML(トークン) + Markdown(意図) |

### 10.3 コンポーネントメタデータの構造

効果的なAI向けコンポーネント文書の5要素:
1. **Usage**: いつ使うか・使ってはいけない場合
2. **AI Hints**: 選択基準の明示(例: 「主要アクションにはprimary、副次アクションにはsecondary」)
3. **Variants**: 各バリアントの目的
4. **Composition**: 何と組み合わせるか
5. **Behavior**: 状態・インタラクション・レスポンシブ挙動

### 10.4 Atlassianの知見

「LLMを助けるルールを特定するプロセスは、そのままそれを人間に説明するルールを特定するプロセスでもある」— AIのための明確さは人間のための明確さでもある。

### 10.5 設計意図の構造化記述例

**抽象的(人間のみ読める)**:
「プライマリボタンは1画面に1つ」

**構造化(AI可読)**:
```json
{
  "component": "Button",
  "variant": "primary",
  "constraint": "max_per_context: 1",
  "rationale": "Multiple primary buttons create visual hierarchy confusion",
  "anti_pattern": "Multiple primary buttons in the same section",
  "correct_pattern": "Use one primary + secondary/ghost for other actions"
}
```

---

## 11. 弱い原則の典型パターン

| パターン | 例 | 問題点 |
|---|---|---|
| **トリヴィアル原則** | 「ユーザーを幸せにする」 | 反対を誰も主張しないため原則にならない |
| **過度に具体的** | 「8pxグリッドを使う」 | UIにしか適用できず、原則の射程が狭すぎる |
| **すべてを肯定する** | 「シンプルかつリッチに」 | トレードオフを示さず、判断の助けにならない |
| **一般的すぎる** | 「使いやすくする」 | 誰もが賛同するが、具体的判断に使えない |
| **「何」の記述** | 「色をブランドカラーで統一する」 | 「なぜ」がなく、例外判断が難しい |

---

## 出典

- [Design Principles to Support Better Decision Making - NN/G](https://www.nngroup.com/articles/design-principles/)
- [Design Guidance: Principles, Patterns, Heuristics, and Team Charters - NN/G](https://www.nngroup.com/articles/design-guidance/)
- [10 Usability Heuristics for User Interface Design - NN/G](https://www.nngroup.com/articles/ten-usability-heuristics/)
- [Government Design Principles - GOV.UK](https://www.gov.uk/guidance/government-design-principles)
- [Design Principles | A Free Library of Real-World Design Principles - principles.design](https://principles.design/)
- [Principles for Design Principles - Clearleft](https://clearleft.com/thinking/principles-for-design-principles)
- [Dieter Rams: 10 Timeless Commandments for Good Design - IxDF](https://ixdf.org/literature/article/dieter-rams-10-timeless-commandments-for-good-design)
- [Voice and Tone | Mailchimp Content Style Guide](https://styleguide.mailchimp.com/voice-and-tone/)
- [Writing Goals and Principles | Mailchimp Content Style Guide](https://styleguide.mailchimp.com/writing-principles/)
- [Shopify Experience Values — Polaris React](https://polaris-react.shopify.com/foundations/experience-values)
- [Shopify Polaris Content Fundamentals](https://polaris-react.shopify.com/content/fundamentals)
- [Design Principles - NHS Digital Service Manual](https://service-manual.nhs.uk/design-system/design-principles)
- [What Makes a Good Design Principle? - Matthew Ström / Medium](https://medium.com/@ilikescience/what-makes-a-good-design-principle-f5647629405e)
- [Creating Great Design Principles: 6 Counter-intuitive Tests - Center Centre](https://articles.centercentre.com/creating-design-principles/)
- [Early Design Principles at Medium - principles.design](https://principles.design/examples/medium-s-design-principles)
- [Indeed's Design Principles and How We Made Them](https://indeed.design/article/indeeds-design-principles-and-how-we-made-them/)
- [Design Rationale Documentation - Pencil & Paper](https://www.pencilandpaper.io/articles/design-rationale-documentation)
- [Design System Governance: Who Owns What - Product Rocket](https://productrocket.ro/articles/design-system-governance/)
- [Design System Documentation as Structured Metadata - Giorris.dev](https://www.giorris.dev/thoughts/design-system-documentation-as-structured-metadata)
- [AI-Ready Design Systems - Supernova.io](https://www.supernova.io/blog/ai-ready-design-systems-preparing-your-design-system-for-machine-powered-product-development)
- [DESIGN.md - Google Labs Code](https://github.com/google-labs-code/design.md)
- [AGENTS.md, SKILL.md, DESIGN.md: How AI Instructions Split into Three Layers - DEV Community](https://dev.to/aws-builders/agentsmd-skillmd-designmd-how-ai-instructions-split-into-three-layers-d0g)
- [Atlassian Design System: Building the Context Engine for the AI Era](https://www.atlassian.com/blog/ai-at-work/atlassian-design-system-building-the-context-engine-for-the-ai-era)
- [Teaching AI to Speak Our Design Language - Atlassian](https://www.atlassian.com/blog/ai-at-work/teaching-ai-to-speak-our-design-language)
- [A Practical Guide to Design Principles - Smashing Magazine](https://www.smashingmagazine.com/2026/04/practical-guide-design-principles/)
- [Monzo Product Principles - principles.design](https://principles.design/examples/monzo-product-principles)
- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- [Architectural Decision Records - adr.github.io](https://adr.github.io/)
- [Documenting Architecture Decisions - Cognitect](https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [Design system governance - Ramotion](https://www.ramotion.com/blog/design-system-guide-chapter-2-principles-and-governance/)

---

## このリポジトリへの示唆

1. **原則は「even over」形式で書く**: 「シンプルさ even over 機能の豊富さ」のようにトレードオフを明示することで、AIも人間も具体的なUI判断に使えるようになる。抽象的な美辞麗句を避け、実際に何かを諦めることを宣言する形式が最も効く。

2. **4層の階層を明確に分離して記述する**: 原則(Principles)→ガイドライン(Guidelines)→パターン(Patterns)→Do/Don'tという層を分けて文書化する。AI向けにはこの階層を構造化データとして提供し、参照できるようにすると精度が上がる。

3. **意図(Why)を常に付随させる**: 各原則・トークン・コンポーネントの判断に「なぜそうするか」を1〜2文で添える。Atlassianの計測では、意図を含む構造化文書は意図なしと比べてAIの精度が52%向上した(同社発表値)。

4. **DESIGN.md / AGENTS.md 的な二層構造を採用する**: 機械可読なYAML(値・トークン・制約)と人間可読なMarkdown(意図・文脈・理由)を同一ファイルに共存させる形式が、AIと人間の双方に有効な現時点の有望なアプローチである(DESIGN.md仕様自体はalphaステータスであり、仕様・スキーマは変更される可能性がある)。

5. **ボイス&トーンと用語集をコンテンツ原則として独立させる**: デザイン原則の中にコンテンツ原則を混ぜるのではなく、「言葉のデザイン」として別章を設ける。Mailchimp方式(ボイス4特性+コンテンツ品質4基準)は参照可能なテンプレートとなる。

6. **「誰が決めるか」を原則文書に明記する**: 原則に対するオーナーシップと意思決定プロセスを文書化する。これこそが「WhoOwnsDesign」の核心。原則のガバナンスモデル(専任チーム/ローテーション)、変更プロセス、例外申請の方法をセットで記述する。

7. **デザイン決定記録(DDR: Design Decision Record)の導入を検討する**: ADR的な発想で「なぜこのコンポーネントをこう設計したか」を記録する仕組みを作ることで、将来のAIや新メンバーへのコンテキスト移譲が劇的に容易になる。原則レベルの記述と、個別判断レベルの記述を分けて管理するとよい。
