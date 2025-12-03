# SOFI銘柄特化：n8nワークフロー用X投稿生成プロンプト

## n8nワークフローの構成

```
1. Schedule Trigger (毎日特定時刻)
   ↓
2. Tavily Search Node (SOFI最新情報検索)
   ↓
3. OpenAI ChatGPT Node (投稿生成)
   ↓
4. Code Node (文字数チェック)
   ↓
5. Twitter Node (投稿実行)
```

---

## Tavily Search Nodeの設定

### 検索クエリ（必ず英語）

**パターン1：総合ニュース**
```
SoFi Technologies SOFI stock news latest earnings revenue growth 2025
```

**パターン2：株価・業績重視**
```
SOFI stock price earnings revenue profit margin Q4 2024 Q1 2025
```

**パターン3：事業展開**
```
SoFi bank charter student loans fintech growth strategy partnership
```

**パターン4：競合比較**
```
SoFi vs Robinhood vs Upstart fintech comparison market share
```

**パターン5：アナリスト評価**
```
SOFI stock analyst rating price target upgrade downgrade
```

### Tavilyノード設定
- **Search Depth**: `advanced`（より詳細な情報）
- **Max Results**: `5`
- **Include Domains**: `finance.yahoo.com, seekingalpha.com, benzinga.com, investing.com, fool.com`（信頼できる金融サイト）

---

## OpenAI ChatGPT Node設定

### モデル
- **推奨**: `gpt-4o-mini`（コスパ重視、品質十分）
- **高品質**: `gpt-4o`（より洗練された文章）

### Temperature
- `0.7`（創造性とのバランス）

---

## システムプロンプト（System Message）

```
あなたは「アイヒモ」という20代の米国株投資家・個人開発者です。

【キャラクター設定】
- 口調：「です・ます調」で親しみやすく明るい
- 性格：正直で率直、適度に謙虚
- スタイル：体験談ベース、ポジティブな表現
- 絵文字：控えめ（0-1個程度）

【投稿対象】
SOFI（SoFi Technologies, Inc.）
- フィンテック企業
- デジタルバンキング、学生ローン、投資サービス
- ティッカー：SOFI
- 時価総額：中型株（2024-2025年時点）

【投稿ルール】
1. 必ず140文字以内（日本語、ハッシュタグ含む）
2. ハッシュタグを2-4個含める
   - #SOFI は必須
   - 他の候補：#米国株 #フィンテック #成長株 #個人投資家 #20代投資 #中型株
3. 具体的な数字や事実を必ず含める
   - 売上成長率、利益率、ユーザー数、株価変動など
4. 読者の興味を引く書き出し
5. 投資を煽らず、正直に情報を伝える
6. リスクがあれば正直に言及

【投稿の型】（1つ選択）
- 「発見型」：〜を見つけました、〜に注目しています
- 「数字型」：〜%上昇/下落、売上〜%増、ユーザー数〜万人
- 「疑問型」：〜って知ってました？
- 「体験型」：〜を調べてみたら、〜でした

【禁止事項】
- 「絶対上がる」「今買うべき」などの断定的表現
- リスクを無視した煽り
- 140文字超過
- SOFIに関係ない情報

【重要】
- 投稿は日本語で作成
- 出力は投稿テキストのみ（説明不要）
- 必ず140文字以内に収める
```

---

## ユーザープロンプト（User Message）

### パターンA：基本型（推奨）

```
以下はTavily APIで検索したSOFI（SoFi Technologies）の最新情報です。

【検索結果】
{{ $json.results }}

この情報を元に、SOFIに関する140文字以内のX投稿を1つ作成してください。

【必須条件】
✓ アイヒモの口調（です・ます調、親しみやすい）
✓ 具体的な数字や事実を含める（売上、利益、ユーザー数、株価など）
✓ #SOFI を必ず含む + 他のハッシュタグ1-3個
✓ 投稿の型（発見型、数字型、疑問型、体験型）のいずれかを使用
✓ 必ず140文字以内

出力は投稿テキストのみでお願いします。
```

### パターンB：要約済み情報を渡す（より高精度）

```
以下はSOFI（SoFi Technologies）の最新情報です。

【企業名】SoFi Technologies
【ティッカー】SOFI
【最新ニュース】{{ $json.news_summary }}
【株価情報】{{ $json.price_info }}
【業績情報】{{ $json.earnings_info }}
【検索日】{{ $json.search_date }}

この情報を元に、140文字以内のX投稿を作成してください。

【必須条件】
✓ アイヒモの口調（です・ます調、親しみやすく正直）
✓ 具体的な数字を含める
✓ #SOFI 必須 + 他ハッシュタグ1-3個
✓ 読者が興味を持つ内容
✓ 投資を煽らず、客観的に

必ず140文字以内に収めてください。出力は投稿テキストのみ。
```

### パターンC：ニュースがない場合の対応

```
以下はSOFI（SoFi Technologies）の最新検索結果です。

【検索結果】
{{ $json.results }}

【指示】
もし重要なニュースがない場合は、SOFIの基本情報（ビジネスモデル、強み、懸念点など）を交えた投稿を作成してください。

直近の業績や事業展開に焦点を当て、140文字以内のX投稿を作成してください。

【必須条件】
✓ アイヒモの口調
✓ 具体的な情報
✓ #SOFI + ハッシュタグ1-3個
✓ 140文字以内

出力は投稿テキストのみ。
```

---

## Code Node：Tavily結果整形（オプション、推奨）

Tavily検索結果を整形して、ChatGPTに渡しやすくします。

```javascript
// Tavilyの検索結果を取得
const tavilyResults = $input.item.json.results || [];

if (tavilyResults.length === 0) {
  return {
    json: {
      news_summary: "最新の重要ニュースは見つかりませんでした。",
      price_info: "株価情報なし",
      earnings_info: "業績情報なし",
      search_date: new Date().toISOString().split('T')[0],
      has_news: false
    }
  };
}

// 最初の3件の結果を要約
let newsSummary = "";
let priceInfo = "";
let earningsInfo = "";

for (let i = 0; i < Math.min(3, tavilyResults.length); i++) {
  const result = tavilyResults[i];
  newsSummary += `【ニュース${i + 1}】${result.title}\n`;
  newsSummary += `${result.content}\n\n`;

  // 株価情報を検出
  if (result.content.match(/stock price|share price|\$[0-9]+\.[0-9]+|up [0-9]+%|down [0-9]+%/i)) {
    priceInfo += result.content.substring(0, 200) + "\n";
  }

  // 業績情報を検出
  if (result.content.match(/revenue|earnings|profit|Q[1-4]|quarterly|annual/i)) {
    earningsInfo += result.content.substring(0, 200) + "\n";
  }
}

return {
  json: {
    news_summary: newsSummary || "関連ニュースなし",
    price_info: priceInfo || "株価情報なし",
    earnings_info: earningsInfo || "業績情報なし",
    search_date: new Date().toISOString().split('T')[0],
    result_count: tavilyResults.length,
    has_news: tavilyResults.length > 0
  }
};
```

---

## Code Node：文字数チェック（必須）

ChatGPT出力が140文字を超えた場合に切り詰めます。

```javascript
// ChatGPTの出力を取得
const tweet = $input.item.json.choices[0].message.content.trim();
const tweetLength = tweet.length;

// 140文字チェック
if (tweetLength > 140) {
  // ハッシュタグを保持しつつ切り詰め
  const hashtagMatch = tweet.match(/(#[^\s]+)/g);
  const hashtags = hashtagMatch ? hashtagMatch.join(' ') : '';
  const mainText = tweet.replace(/(#[^\s]+)/g, '').trim();

  // 本文を切り詰め
  const maxMainLength = 140 - hashtags.length - 1; // -1 for space
  const trimmedMain = mainText.substring(0, maxMainLength - 3) + '...';

  const finalTweet = trimmedMain + ' ' + hashtags;

  return {
    json: {
      tweet: finalTweet.substring(0, 140), // 安全のため再度確認
      original_length: tweetLength,
      is_trimmed: true,
      warning: "140文字を超えたため切り詰めました"
    }
  };
} else {
  return {
    json: {
      tweet: tweet,
      original_length: tweetLength,
      is_trimmed: false
    }
  };
}
```

---

## 実際の投稿例（アイヒモ口調、SOFI特化）

### 例1：発見型（決算ニュース）
```
SOFIのQ4決算を調べてみました。売上は前年比25%増の6.3億ドルで、8四半期連続の黒字達成。デジタルバンクのユーザー数も前年比44%増だそうです。手堅い成長してますね。#SOFI #米国株 #フィンテック
```
（98文字）

### 例2：数字型（株価動向）
```
SOFIの株価、今週7%上昇してます。Q4の好決算を受けて投資家の評価が上がったみたいです。時価総額は約90億ドル。フィンテック中型株として注目してます。#SOFI #米国株 #成長株 #20代投資
```
（97文字）

### 例3：疑問型（事業展開）
```
SOFIが銀行ライセンス取得したって知ってました？これで預金サービスも提供できるようになって、利益率が改善してるそうです。フィンテックから銀行への進化、面白いです。#SOFI #フィンテック #米国株
```
（96文字）

### 例4：体験型（業績分析）
```
SOFIの業績を調べてみたら、Q3で初めて通期黒字を達成してました。売上成長率は年20%超で、学生ローン事業も回復傾向。まだ割安感あるかもです。#SOFI #米国株 #成長株 #個人投資家
```
（92文字）

### 例5：リスク言及型（正直な投稿）
```
SOFIを調べてます。売上成長は良いんですが、まだPER高めで、金利動向に左右されやすいのが懸念点です。長期目線なら面白いけど、リスクも理解してます。#SOFI #米国株 #20代投資
```
（87文字）

---

## SOFIに関する重要キーワード（検索・分析用）

### ビジネス関連
- SoFi Technologies
- Digital banking / Neobank
- Student loan refinancing
- Personal loans
- Investment platform
- Bank charter（銀行ライセンス）
- Galileo（子会社、決済プラットフォーム）
- Technisys（子会社、バンキングプラットフォーム）

### 業績指標
- Revenue growth（売上成長率）
- Net income / Profitability（純利益・収益性）
- Member growth（会員数増加）
- ARPU (Average Revenue Per User)
- Loan origination（ローン実行額）
- Deposit growth（預金増加）

### 投資家が注目するポイント
- Path to profitability（黒字化への道筋）
- Federal Reserve rate impact（金利影響）
- Student loan moratorium end（学生ローン返済猶予終了）
- Competition with traditional banks
- Fintech sector trends

---

## Tavily検索クエリの最適化（SOFI特化）

### 時期別のクエリ

**決算シーズン（1月、4月、7月、10月）**
```
SoFi SOFI Q4 2024 earnings revenue profit guidance analyst reaction
```

**通常時（最新ニュース）**
```
SoFi SOFI stock news latest announcement partnership product launch
```

**金利変動時**
```
SoFi SOFI interest rate impact Fed rate student loan refinancing
```

**競合比較時**
```
SoFi vs Upstart vs LendingClub fintech lending comparison 2025
```

### より具体的なクエリ例

```
# 株価パフォーマンス
SOFI stock price performance YTD 2025 chart analysis

# アナリスト評価
SOFI stock analyst rating consensus price target buy sell

# 事業成長
SoFi member growth deposit growth loan origination Q4 2024

# 規制・ライセンス
SoFi bank charter OCC FDIC regulatory approval impact

# 新サービス
SoFi new product launch credit card checking account innovation
```

---

## エラー対応・例外処理

### Tavily検索結果が空の場合

**Code Nodeで対応**
```javascript
const results = $input.item.json.results || [];

if (results.length === 0) {
  // 検索結果がない場合、SOFIの基本情報を使用
  return {
    json: {
      fallback_mode: true,
      company_info: "SoFi Technologies (SOFI) はデジタルバンキング、学生ローン、投資サービスを提供するフィンテック企業。2024年に8四半期連続黒字を達成。",
      search_date: new Date().toISOString().split('T')[0]
    }
  };
}
```

**ChatGPTへのフォールバックプロンプト**
```
最新のSOFI関連ニュースが見つかりませんでした。

以下の情報を元に、SOFIについての140文字以内の投稿を作成してください：
- SoFi Technologiesはデジタルバンキング・フィンテック企業
- 学生ローン、個人ローン、投資サービスを提供
- 2024年に8四半期連続黒字達成
- 銀行ライセンス取得済み

アイヒモの口調で、#SOFI を含む投稿を作成してください。140文字以内。
```

---

## 投稿の多様性を持たせる工夫

### ランダムで投稿の型を選択

**システムプロンプトに追加**
```
【投稿の型】
今回は以下のいずれかの型を使用してください（ランダムで1つ選択）：
1. 発見型：「SOFIの〜を見つけました」
2. 数字型：「SOFIの売上が〜%増」「株価が〜%上昇」
3. 疑問型：「SOFIの〜って知ってました？」
4. 体験型：「SOFIを調べてみたら、〜でした」
5. 比較型：「SOFIと他のフィンテック株を比べると」
```

### 週次・月次での振り返り投稿

**月曜日：週の展望**
```
今週のSOFI、注目ポイントは〜
```

**金曜日：週の振り返り**
```
今週のSOFI、株価は〜%変動
```

---

## モニタリング・改善サイクル

### Notionやスプレッドシートに記録

**記録項目**
- 投稿日時
- 投稿内容
- 使用したニュース（Tavilyソース）
- 投稿の型（発見型、数字型など）
- エンゲージメント（いいね、リポスト、返信数）
- 株価（投稿時のSOFI株価）

### 分析ポイント
- どの型の投稿がエンゲージメント高いか
- 決算発表後の投稿は反応が良いか
- 数字を含む投稿 vs ストーリー系投稿
- 最適な投稿時間帯

---

## 完全なワークフロー例

```
ノード1: Schedule Trigger
  - 月〜金の午前9時、午後3時（市場前後）

ノード2: Tavily Search
  - Query: "SoFi SOFI stock news latest earnings Q1 2025"
  - Max Results: 5
  - Search Depth: advanced

ノード3: Code (結果整形)
  - Tavilyの結果から重要情報抽出
  - 株価、業績、ニュースを分類

ノード4: OpenAI ChatGPT
  - Model: gpt-4o-mini
  - System: 上記システムプロンプト
  - User: 上記ユーザープロンプト（パターンB）
  - Temperature: 0.7

ノード5: Code (文字数チェック)
  - 140文字以内確認
  - 超過の場合は切り詰め

ノード6: IF条件分岐（オプション）
  - 重要ニュースの場合のみ投稿
  - または人間の承認待ち

ノード7: Twitter Node
  - 投稿実行

ノード8: Notion / Google Sheets
  - 投稿履歴を保存
  - エンゲージメント追跡
```

---

## トラブルシューティング

### 問題1: 同じような投稿が続く

**解決策**:
- Temperature を 0.8 に上げる
- システムプロンプトに「前回と異なる型を使用」と追加
- Tavily検索クエリを日替わりで変える

### 問題2: SOFIに関係ない情報が含まれる

**解決策**:
- Tavily検索クエリに `SOFI OR "SoFi Technologies"` を必ず含める
- システムプロンプトに「SOFIに直接関係する情報のみ」を強調
- Code Nodeで結果をフィルタリング

### 問題3: 数字が古い、不正確

**解決策**:
- Tavilyの検索結果に日付を含める
- `news published last 7 days` などの期間指定
- 信頼できるドメインのみに絞る（Include Domains設定）

---

## まとめ：SOFI特化型プロンプトの特徴

✅ **Tavily検索クエリ最適化**：SOFI関連の英語クエリ集
✅ **アイヒモ口調維持**：親しみやすく正直な投稿
✅ **140文字厳守**：文字数チェック機能付き
✅ **#SOFI必須**：企業特化ハッシュタグ
✅ **具体的な数字**：売上、株価、ユーザー数など
✅ **投資を煽らない**：リスクも正直に言及
✅ **多様な投稿の型**：発見型、数字型、疑問型、体験型
✅ **エラー対応**：検索結果がない場合のフォールバック

このプロンプトをn8nに設定すれば、毎日自動的にSOFI銘柄の最新情報を、アイヒモの口調で魅力的なX投稿に変換できます！

---

**作成日**: 2025-12-02
**対象銘柄**: SOFI (SoFi Technologies, Inc.)
**用途**: n8nワークフローでの自動X投稿生成
**推奨モデル**: gpt-4o-mini（コスパ◎）
**重要**: Tavily検索は必ず英語クエリ、投稿は日本語140文字以内
