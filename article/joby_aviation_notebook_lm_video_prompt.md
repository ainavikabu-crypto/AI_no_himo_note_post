# Joby Aviation YouTube動画生成 - Notebook LM用プロンプト

## 動画生成の前提条件

### アップロードするソース
1. **台本ファイル**: `joby_aviation_notebook_lm_slides.md`（このファイル）
2. **キャラクター画像**: `aihimo_chara.png`（アイヒモのキャラクター）

---

## Notebook LMへのプロンプト（日本語版）

```
【動画生成指示】

以下の台本を元に、5分間のYouTube動画を作成してください。

## 動画の基本設定

・動画タイトル：「トヨタが250億円投資！空飛ぶタクシーJoby Aviationは買いか？【米国株分析】」
・動画時間：約5分
・スライド総数：10枚
・ナレーション：女性の声（明るく親しみやすいトーン）
・BGM：テクノロジー系、未来的で前向きな曲（音量控えめ）

## キャラクター設定

・アップロードした「aihimo_chara.png」のキャラクター（アイヒモ）を使用
・キャラクターは各スライドに登場
・表情を台本の内容に合わせて変化させる：
  - オープニング（スライド1）：明るく笑顔、歓迎の表情
  - eVTOL説明（スライド2-4）：興味津々、好奇心旺盛な表情
  -財務説明（スライド5）：真剣な表情、少し眉をひそめる
  - FAA認証（スライド6）：期待と緊張が混ざった表情
  - リスク説明（スライド7）：警戒、注意深い表情
  - 評価（スライド8）：分析的、思慮深い表情
  - 投資判断（スライド9）：自信を持った表情
  - まとめ（スライド10）：笑顔、前向きな表情

・キャラクターの配置：各スライドの右下または右側に配置
・キャラクターのサイズ：スライドの20-25%程度

## スライドのデザイン方針

【重要】スライド内の文字は最小限にしてください。

・文字量：各スライドに表示する箇条書きは3-5個まで
・文字サイズ：大きく読みやすく
・フォント：ゴシック体、太字
・カラースキーム：
  - 背景：白またはライトグレー
  - メインカラー：テクノロジーブルー（#0066CC）
  - アクセントカラー：グリーン（#2ECC71）、イエロー（#FFC107）
  - 警告色：レッド（#FF0000）

・ビジュアル重視：
  - 文字よりも画像、グラフ、アイコンを多用
  - 数字は大きく強調
  - 重要ポイントは✅や⚠️のアイコンで表現

## 各スライドの内容

### スライド1：オープニング（0:00-0:30）

【表示する文字】（最小限）
・タイトル：「トヨタが250億円投資！」
・サブタイトル：「空飛ぶタクシー Joby Aviation」

【ビジュアル】
・Joby Aviation eVTOLの飛行イメージ（背景全体）
・トヨタロゴ
・「¥250億」を大きく表示
・株価チャート（+29%）

【キャラクター：アイヒモ】
・右下に配置
・明るく笑顔、歓迎の表情

【ナレーション】
台本の「スライド1」の内容をそのまま読み上げ

---

### スライド2：Joby Aviationとは（0:30-1:15）

【表示する文字】（最小限）
・「eVTOL = 空飛ぶタクシー」
・スペック：
  - 🧑‍🤝‍🧑 最大4名
  - ⚡ 時速320km
  - 🛫 約160km
  - 🌱 電動・静か

【ビジュアル】
・Joby Aviation eVTOLの詳細画像（メイン）
・スペックをアイコンで表現
・ヘリコプターとの比較図（小さく）

【キャラクター：アイヒモ】
・右側に配置
・興味津々、好奇心旺盛な表情

【ナレーション】
台本の「スライド2」の内容をそのまま読み上げ

---

### スライド3：トヨタ投資（1:15-2:00）

【表示する文字】（最小限）
・「トヨタ $250M 投資」
・「Blade買収 40,000人」
・「株価 +29%」

【ビジュアル】
・トヨタロゴ + Joby Aviationロゴ（パートナーシップ）
・$250Mを大きく表示
・株価上昇チャート（+29%急騰）
・Bladeロゴ + 顧客40,000人のアイコン

【キャラクター：アイヒモ】
・右下に配置
・驚きと感心の表情

【ナレーション】
台本の「スライド3」の内容をそのまま読み上げ

---

### スライド4：空飛ぶタクシーの実力（2:00-2:45）

【表示する文字】（最小限）
・「市場成長率 35.3%」
・「2027年 売上200億円」
・「EXPO 2025: 600回フライト」

【ビジュアル】
・都市部の空を飛ぶeVTOL（メインビジュアル）
・市場成長グラフ（右肩上がり、35.3%を強調）
・売上予測グラフ（2024年ゼロ → 2027年200億円）
・EXPO 2025のロゴ

【キャラクター：アイヒモ】
・右側に配置
・未来を見つめる、希望に満ちた表情

【ナレーション】
台本の「スライド4」の内容をそのまま読み上げ

---

### スライド5：財務状況（2:45-3:15）

【表示する文字】（最小限）
・「売上 23億円」
・「損失 継続中」
・「現金 980億円」

【ビジュアル】
・Q3決算データをシンプルな表で表示
・赤字のグラフ（赤色で強調）
・現金残高のグラフ（緑色で安心感）

【キャラクター：アイヒモ】
・右下に配置
・真剣な表情、少し眉をひそめる（心配そう）

【ナレーション】
台本の「スライド5」の内容をそのまま読み上げ

---

### スライド6：FAA認証（3:15-3:45）

【表示する文字】（最小限）
・「FAA認証 = 全て」
・「2026年 テスト開始」
・「最終段階」

【ビジュアル】
・FAA（連邦航空局）ロゴ
・認証プロセスの進捗バー（80-90%完了を示す）
・2026年のタイムライン図

【キャラクター：アイヒモ】
・右側に配置
・期待と緊張が混ざった表情

【ナレーション】
台本の「スライド6」の内容をそのまま読み上げ

---

### スライド7：投資リスク（3:45-4:15）

【表示する文字】（最小限）
・「⚠️ 5つのリスク」
・リスク1-5をアイコンで表現（文字は最小限）

【ビジュアル】
・5つのリスクアイコン（大きく表示）
  - ⚠️ 商業化
  - 💸 赤字
  - ❓ 市場
  - 🏁 競争
  - 📜 規制
・警告マーク（⚠️）を目立たせる

【キャラクター：アイヒモ】
・右下に配置
・警戒、注意深い表情（少し心配そう）

【ナレーション】
台本の「スライド7」の内容をそのまま読み上げ

---

### スライド8：5段階評価（4:15-4:45）

【表示する文字】（最小限）
・「5段階評価」
・各項目の星（★）のみ表示

【ビジュアル】
・5軸のレーダーチャート（大きく表示）
・各項目の星評価を視覚的に（文字は最小限）
  - 革新性 ★★★★★
  - 市場 ★★★★★
  - 財務 ★★☆☆☆
  - 実現 ★★★☆☆
  - 評価 ★★☆☆☆

【キャラクター：アイヒモ】
・右側に配置
・分析的、思慮深い表情（考え込む様子）

【ナレーション】
台本の「スライド8」の内容をそのまま読み上げ

---

### スライド9：投資判断（4:45-5:15）

【表示する文字】（最小限）
・「投資するなら」
・5つのアイコン：
  - 📊 5-10%
  - ⏰ 5-10年
  - 🔀 分散
  - 📰 情報
  - 💪 若さ

【ビジュアル】
・5つの投資方針をアイコンで表現（大きく）
・リスク・リターンの天秤（バランスを示す）

【キャラクター：アイヒモ】
・右下に配置
・自信を持った表情、前向きな笑顔

【ナレーション】
台本の「スライド9」の内容をそのまま読み上げ

---

### スライド10：まとめ（5:15-5:30）

【表示する文字】（最小限）
・「私の結論：様子見」
・✅ポイント（3つ）
・⚠️リスク（3つ）

【ビジュアル】
・Joby Aviation eVTOL（背景）
・✅と⚠️のアイコンで要点を整理（文字は最小限）
・「次回もお楽しみに！」のエンディングテキスト

【キャラクター：アイヒモ】
・中央やや右に配置（大きめ）
・笑顔、前向きな表情、手を振る様子

【ナレーション】
台本の「スライド10」の内容をそのまま読み上げ

---

## アニメーション・トランジション

・スライド切り替え：スムーズなフェードまたはスライド
・キャラクター（アイヒモ）：各スライドで表情が変わる
・重要な数字や文字：拡大表示やハイライト効果
・グラフ：アニメーションで徐々に表示

## BGMとSE

・BGM：テクノロジー系、未来的で前向きな曲（音量はナレーションの30%程度）
・SE：
  - スライド切り替え時：軽いサウンド
  - 重要ポイント表示時：チャイム音
  - リスク警告時：警告音（ピピッ）

## ナレーション設定

・声：女性の声（アイヒモのイメージに合う明るく親しみやすいトーン）
・スピード：やや速め（情報量が多いため）
・イントネーション：台本の内容に合わせて感情を込める
  - 驚き、興奮：トヨタ投資、市場成長のパート
  - 真剣、注意深く：財務状況、リスクのパート
  - 前向き、自信：投資判断、まとめのパート

## エンディング

・「チャンネル登録・高評価お願いします！」のテキスト表示
・アイヒモが手を振る
・動画終了

---

このプロンプトと台本、キャラクター画像を元に、YouTube動画を生成してください。
文字は最小限にし、ビジュアル（画像、グラフ、アイコン）を最大限活用してください。
キャラクター（アイヒモ）の表情を台本の内容に合わせて変化させ、視聴者が親しみを感じられる動画にしてください。
```

---

## Notebook LMへのプロンプト（英語版）

```
【Video Generation Instructions】

Please create a 5-minute YouTube video based on the provided script.

## Basic Video Settings

・Video Title: "Toyota Invests ¥25 Billion! Is Joby Aviation's Flying Taxi a Buy? [US Stock Analysis]"
・Duration: Approximately 5 minutes
・Number of Slides: 10
・Narration: Female voice (bright and friendly tone)
・BGM: Technology-themed, futuristic and positive music (low volume)

## Character Settings

・Use the uploaded character "aihimo_chara.png" (Aihimo)
・Character appears on every slide
・Change facial expressions to match the script content:
  - Opening (Slide 1): Bright smile, welcoming expression
  - eVTOL explanation (Slides 2-4): Curious, interested expression
  - Financial explanation (Slide 5): Serious expression, slightly concerned
  - FAA certification (Slide 6): Expectant and tense expression
  - Risk explanation (Slide 7): Alert, cautious expression
  - Evaluation (Slide 8): Analytical, thoughtful expression
  - Investment decision (Slide 9): Confident expression
  - Summary (Slide 10): Smile, positive expression

・Character placement: Bottom right or right side of each slide
・Character size: Approximately 20-25% of slide

## Slide Design Guidelines

【IMPORTANT】Minimize text on slides.

・Text amount: Maximum 3-5 bullet points per slide
・Text size: Large and readable
・Font: Gothic, bold
・Color scheme:
  - Background: White or light gray
  - Main color: Technology blue (#0066CC)
  - Accent colors: Green (#2ECC71), Yellow (#FFC107)
  - Warning color: Red (#FF0000)

・Visual-focused:
  - Prioritize images, graphs, and icons over text
  - Emphasize numbers with large display
  - Use ✅ and ⚠️ icons for key points

## Slide Content

[Include all 10 slides with minimal text, visual focus, and character expressions as described in the Japanese version above]

## Animations & Transitions

・Slide transitions: Smooth fade or slide
・Character (Aihimo): Change expressions on each slide
・Important numbers/text: Zoom in or highlight effects
・Graphs: Gradually display with animation

## BGM and Sound Effects

・BGM: Technology-themed, futuristic positive music (30% volume of narration)
・SE:
  - Slide transitions: Light sound
  - Key point display: Chime sound
  - Risk warnings: Alert sound (beep)

## Narration Settings

・Voice: Female voice (bright and friendly tone matching Aihimo)
・Speed: Slightly fast (due to high information density)
・Intonation: Match emotions to script content
  - Surprise, excitement: Toyota investment, market growth parts
  - Serious, careful: Financial status, risk parts
  - Positive, confident: Investment decision, summary parts

## Ending

・Display text: "Please subscribe and like!"
・Aihimo waves goodbye
・Video ends

---

Based on this prompt, the script, and the character image, please generate a YouTube video.
Minimize text and maximize visuals (images, graphs, icons).
Change Aihimo's facial expressions to match the script content and create a video that viewers can relate to.
```

---

## 補足：Notebook LMでの実行手順

### ステップ1：ソースをアップロード
1. `joby_aviation_notebook_lm_slides.md`（台本ファイル）をアップロード
2. `aihimo_chara.png`（キャラクター画像）をアップロード

### ステップ2：動画生成を指示
上記のプロンプト（日本語版または英語版）をNotebook LMに入力して、動画生成を依頼

### ステップ3：生成された動画を確認
- スライドの文字量が多すぎないか
- キャラクター（アイヒモ）の表情が変化しているか
- ビジュアル（画像、グラフ）が適切に使用されているか
- ナレーションのトーンが適切か

### ステップ4：微調整（必要に応じて）
生成された動画を確認し、必要に応じてプロンプトを修正して再生成

---

## 重要ポイント

### ✅ 文字は最小限
- 各スライド3-5個の箇条書きまで
- 文字よりもビジュアル（画像、グラフ、アイコン）を優先
- 重要な数字は大きく表示

### ✅ キャラクター表情の変化
- 台本の内容に合わせてアイヒモの表情を変える
- 笑顔、真剣、心配、自信など、感情豊かに
- 視聴者が親しみを感じられるように

### ✅ ビジュアル重視
- eVTOLの飛行イメージ
- 株価チャート
- 市場成長グラフ
- リスクアイコン
- レーダーチャート

### ✅ ナレーションのトーン
- 明るく親しみやすい女性の声
- 台本の内容に合わせて感情を込める
- やや速めのスピードで情報密度を高める

---

**作成日**: 2025-12-06
**用途**: Notebook LMでYouTube動画を自動生成するためのプロンプト
**動画時間**: 約5分
**スライド総数**: 10枚
**キャラクター**: アイヒモ（表情変化あり）
**文字量**: 最小限（ビジュアル重視）
