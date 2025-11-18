# note用SEOブログ記事自動生成ワークフロー

## 概要

このn8nワークフローは、Google検索の上位結果を分析し、SEO最適化されたnote投稿用のブログ記事を自動生成します。

## 主な機能

- ✅ Google検索APIで上位3件の結果を取得
- ✅ 各ページのコンテンツを自動収集
- ✅ AI（GPT-4o）で内容を分析・要約
- ✅ SEO最適化されたnote用記事を自動生成
- ✅ **マークダウン形式**で出力（すぐにnoteに投稿可能）
- ✅ **タイトルにハッシュタグを自動挿入**（3〜5個）
- ✅ Google Driveに自動保存

## 出力形式

生成される記事は以下の構造になります：

```markdown
# [魅力的なタイトル]

#タグ1 #タグ2 #タグ3 #タグ4 #タグ5

[導入部分]

## [見出し1]

[本文]

### [小見出し]

[詳細な内容]

## [見出し2]

[本文]

## まとめ

[結論と行動喚起]
```

## セットアップ手順

### 1. 必要な認証情報の設定

#### RapidAPI（Google Search API）
1. [RapidAPI](https://rapidapi.com/)でアカウント作成
2. [Google Search API](https://rapidapi.com/rphrp1985/api/google-search74)を購読
3. API Keyを取得
4. n8nで「Header Auth account」認証情報を作成
   - Header Name: `x-rapidapi-key`
   - Value: `取得したAPI Key`

#### OpenAI API
1. [OpenAI Platform](https://platform.openai.com/)でAPI Keyを取得
2. n8nで「OpenAI API」認証情報を作成
   - API Key: `取得したAPI Key`

#### Google Drive
1. n8nで「Google Drive OAuth2」認証情報を作成
2. Googleアカウントで認証

### 2. ワークフローのインポート

1. n8nを開く
2. 右上の「…」メニューから「Import from file」を選択
3. `note_seo_blog_workflow.json`をアップロード
4. 各ノードの認証情報を設定

### 3. 認証情報の紐付け

以下のノードで認証情報を設定してください：

- **HTTP Request**: Header Auth account
- **Message a model**: OpenAI API
- **Message a model1**: OpenAI API
- **Create file from text**: Google Drive OAuth2

## 使い方

1. ワークフローを有効化
2. チャットトリガーにアクセス
3. 検索したいキーワードを入力（例：「SEO対策 初心者」）
4. ワークフローが自動実行され、記事が生成される
5. Google Driveに「note投稿_[日時].md」というファイル名で保存される

## ワークフローの流れ

```
チャット入力
    ↓
Google検索（上位3件取得）
    ↓
URL抽出
    ↓
各ページのコンテンツ取得（ループ処理）
    ↓
HTMLからテキスト抽出
    ↓
コンテンツ統合
    ↓
AI要約（GPT-4o）
    ↓
SEO記事生成（GPT-4o + マークダウン形式 + タグ付き）
    ↓
Google Driveに保存
```

## カスタマイズ方法

### 検索結果の件数を変更

「HTTP Request」ノードの`limit`パラメータを変更：
```json
{
  "name": "limit",
  "value": "5"  // 3から5に変更
}
```

### タグの数を変更

「Message a model1」ノードのプロンプト内で、タグの数を指定：
```
・タグは記事のテーマに沿った、検索されやすいキーワードを3〜7個選ぶ
```

### 保存先フォルダを変更

「Create file from text」ノードの`folderId`を変更：
1. Google Driveで目的のフォルダを開く
2. URLからフォルダIDをコピー
3. n8nのノードで設定

## 変更点（元のワークフローからの改善）

### 1. マークダウン形式対応
- note投稿に最適化されたマークダウン記法で出力
- 見出し、箇条書き、番号付きリスト、太字などを適切に使用

### 2. タグ自動生成
- タイトルの直後に3〜5個のハッシュタグを自動挿入
- SEOを意識したキーワード選定

### 3. ファイル名改善
- 元: `for Blog Post_ {{$now}}`
- 新: `note投稿_{{$now.format('yyyy-MM-dd_HHmmss')}}.md`
- `.md`拡張子を追加してマークダウンファイルとして保存

### 4. プロンプト最適化
- note投稿に特化したトーン・文体
- 日本語圏の読者に適した表現
- スマホでも読みやすい構成を意識

## トラブルシューティング

### エラー: "No HTML content provided"
- HTTPリクエストがタイムアウトしている可能性があります
- 「HTTP Request1」ノードの`timeout`を増やしてください（デフォルト: 10000ms）

### 記事が生成されない
- OpenAI APIの利用制限を確認してください
- API Keyが正しく設定されているか確認してください

### Google Driveに保存されない
- OAuth2認証が期限切れの可能性があります
- 認証情報を再設定してください

## ライセンス

MIT License

## サポート

問題が発生した場合は、Issueを作成してください。
