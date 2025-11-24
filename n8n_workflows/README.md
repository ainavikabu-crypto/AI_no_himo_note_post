# n8n ワークフロー集

米国株の情報収集・分析を自動化するn8nワークフロー集です。

## ワークフロー一覧

| ファイル | 説明 |
|----------|------|
| `us_stock_analysis_wordpress.json` | 銘柄分析→WordPress投稿 |
| `tavily_summary_to_md.json` | Tavily検索→要約→MD保存 |

---

# 1. 米国株銘柄分析 → WordPress自動投稿

`us_stock_analysis_wordpress.json`

このワークフローは、米国企業の銘柄分析を自動化し、WordPressに記事を投稿します。

## ワークフロー概要

```
手動実行
    ↓
企業情報を設定（ティッカー、企業名）
    ↓
┌─────────────────────────────────────┐
│  Tavily - 決算情報検索（並列実行）    │
│  Tavily - 事業内容・展望検索          │
└─────────────────────────────────────┘
    ↓
データを統合
    ↓
┌─────────────────────────────────────┐
│  OpenAI - 記事生成（並列実行）        │
│  DALL-E - アイキャッチ生成            │
└─────────────────────────────────────┘
    ↓
WordPress投稿準備
    ↓
アイキャッチ画像をダウンロード
    ↓
WordPress - 画像アップロード
    ↓
WordPress - 記事投稿（下書き）
    ↓
完了通知
```

## 必要な認証情報（Credentials）

n8nで以下の認証情報を設定してください。

### 1. Tavily API

- **Type**: HTTP Header Auth
- **Name**: `Authorization`
- **Value**: `Bearer YOUR_TAVILY_API_KEY`

Tavily APIキーは [https://tavily.com/](https://tavily.com/) で取得できます。

### 2. OpenAI API

- **Type**: OpenAI API
- **API Key**: `YOUR_OPENAI_API_KEY`

OpenAI APIキーは [https://platform.openai.com/](https://platform.openai.com/) で取得できます。

### 3. WordPress API

- **Type**: WordPress API
- **URL**: `https://your-wordpress-site.com`
- **Username**: WordPressユーザー名
- **Password**: アプリケーションパスワード

※ WordPressのアプリケーションパスワードは、WordPress管理画面 > ユーザー > プロフィール > アプリケーションパスワード で生成できます。

## セットアップ手順

### 1. ワークフローのインポート

1. n8nを開く
2. 「Workflows」→「Import from File」を選択
3. `us_stock_analysis_wordpress.json` を選択してインポート

### 2. 認証情報の設定

1. 各ノードをクリックして認証情報を設定
2. Tavilyノード: HTTP Header Auth を設定
3. OpenAIノード: OpenAI API を設定
4. WordPressノード: WordPress API を設定

### 3. 企業情報の設定

「企業情報を設定」ノードで分析対象の企業を指定します。

```json
{
  "ticker": "BBAI",
  "companyName": "BigBear.ai",
  "companyNameJa": "ビッグベア・エーアイ"
}
```

### 4. WordPressカテゴリの確認

「WordPress - 記事投稿」ノードのカテゴリ設定を、WordPressに存在するカテゴリに合わせてください。

## 記事の出力内容

### 5段階評価項目

1. **成長性**: 売上・利益の伸び
2. **収益性**: 黒字化、利益率
3. **財務健全性**: 現金、負債
4. **競争優位性**: 技術力、市場ポジション
5. **株価の割安度**: PER、期待値との乖離

### アイヒモの視点

- 小型〜中型株としての魅力
- 日本語情報が少ない銘柄の先回り投資
- AI×特定分野の成長可能性

### アイキャッチ画像

DALL-E 3で自動生成。以下の要素を含みます：

- 企業名
- ティッカーシンボル
- 「銘柄分析」テキスト
- テクノロジー感のある背景

## カスタマイズ

### スケジュール実行

「手動実行」ノードを「Schedule Trigger」に変更すると、定期実行が可能です。

```json
{
  "rule": {
    "interval": [
      {
        "field": "weeks",
        "weeksInterval": 1
      }
    ]
  }
}
```

### 複数銘柄の分析

「企業情報を設定」ノードの前に「Spreadsheet」ノードを追加し、Google SheetsやExcelから銘柄リストを読み込むことができます。

### 通知の追加

「完了通知」ノードの後に「Slack」や「Discord」ノードを追加して、投稿完了を通知できます。

## トラブルシューティング

### Tavily APIエラー

- APIキーが正しく設定されているか確認
- APIの利用上限に達していないか確認

### OpenAI APIエラー

- APIキーが正しく設定されているか確認
- GPT-4oへのアクセス権があるか確認
- トークン上限に注意（max_tokens: 4000）

### WordPress投稿エラー

- アプリケーションパスワードが正しいか確認
- WordPressのREST APIが有効か確認
- カテゴリ・タグが存在するか確認

---

# 2. Tavily企業情報検索 → 要約 → MD保存

`tavily_summary_to_md.json`

Tavilyで企業情報を検索し、要約ライターで構造化した要約を生成、Markdownファイルとして保存します。

## ワークフロー概要

```
手動実行
    ↓
企業情報を設定（ティッカー、企業名）
    ↓
┌─────────────────────────────────────┐
│  Tavily - 事業内容検索（並列実行）   │
│  Tavily - 決算情報検索               │
└─────────────────────────────────────┘
    ↓
検索結果を統合（Merge）
    ↓
検索結果を結合（Code）
    ↓
OpenAI - 要約ライター
    ↓
Markdownフォーマット
    ↓
MDファイル保存
    ↓
完了通知
```

## 要約ライターの特徴

summaryプロンプトを使用して、以下の構造で要約を生成します：

- **概要**: 企業の概要と主要事業
- **事業内容**: 主要製品・サービス、技術・強み
- **最新決算情報**: 財務ハイライト（表形式）、決算のポイント
- **今後の展望**: 成長ドライバー、リスク
- **主要なインサイト**: ブログ記事に活用できる独自の視点

## 出力ファイル

### ファイル名形式
```
{ticker}_summary_{yyyy-MM-dd}.md
例: BBAI_summary_2024-11-24.md
```

### 出力例
```markdown
---
ticker: BBAI
company: BigBear.ai
generated_at: 2024-11-24T12:00:00.000Z
type: research_summary
---

# BigBear.ai（BBAI）企業情報要約

## 概要
...

## 参照ソース
1. [ソースタイトル](URL)
...

---
*この要約は 2024-11-24 に自動生成されました。*
```

## セットアップ

### 1. 企業情報の設定

「企業情報を設定」ノードで以下を指定：

```json
{
  "ticker": "BBAI",
  "companyName": "BigBear.ai",
  "companyNameJa": "ビッグベア・エーアイ",
  "outputDir": "/data/stock_research"
}
```

### 2. 出力先ディレクトリ

`outputDir`にMDファイルが保存されます。n8nのデータディレクトリ内のパスを指定してください。

## 活用方法

### WordPress投稿との連携

1. このワークフローで要約MDファイルを生成
2. `us_stock_analysis_wordpress.json`で記事を生成・投稿
3. 要約ファイルをブログ記事の下書きとして活用

### 複数銘柄の一括処理

「企業情報を設定」ノードの前に「Loop Over Items」や「Spreadsheet」ノードを追加して、複数銘柄を一括処理できます。

---

## ライセンス

このワークフローは自由に使用・改変できます。

---

作成: アイヒモ
更新日: 2024-11-24
