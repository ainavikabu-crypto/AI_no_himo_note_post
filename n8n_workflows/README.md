# n8n ワークフロー: 米国株銘柄分析 → WordPress自動投稿

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

## ライセンス

このワークフローは自由に使用・改変できます。

---

作成: アイヒモ
更新日: 2024-11-24
