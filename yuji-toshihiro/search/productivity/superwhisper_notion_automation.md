# Superwhisper → Notion 完全自動化ガイド

## 🚀 自動化方法一覧（推奨順）

### 方法1: Zapier/Make（旧Integromat）を使用 ⭐最も簡単
```yaml
設定手順:
1. Superwhisperの出力をGoogleドキュメントやDropboxに自動保存
2. Zapierでファイル監視
3. 新規テキストを検知したらNotion APIで自動追加
4. 完了後、元ファイルをクリア

必要なもの:
- Zapierアカウント（無料プランでも可）
- Notion API トークン
- 中継用のクラウドストレージ
```

### 方法2: iOSショートカット + Notion API 📱
```yaml
設定手順:
1. Superwhisperで「ショートカットに送る」を設定
2. ショートカットアプリで以下を作成:
   - Superwhisperからテキストを受け取る
   - Notion APIにPOSTリクエスト
   - ToDoデータベースに直接追加

メリット:
- 完全無料
- iPhone/iPad完結
- ワンタップで実行
```

**ショートカットの具体的な設定:**
```javascript
// ショートカットアプリ内の設定
1. "テキストを取得" (Superwhisperから)
2. "Webリクエスト" アクション追加
   URL: https://api.notion.com/v1/pages
   メソッド: POST
   ヘッダー: 
     - Authorization: Bearer [YOUR_API_KEY]
     - Notion-Version: 2022-06-28
   本文: {
     "parent": {"database_id": "[YOUR_DATABASE_ID]"},
     "properties": {
       "Name": {"title": [{"text": {"content": "入力テキスト"}}]},
       "Status": {"select": {"name": "ToDo"}}
     }
   }
```

### 方法3: IFTTT連携 🔄
```yaml
トリガー: 
- Superwhisperが指定フォルダにファイル作成
- またはメール送信

アクション:
- Notion にページ作成
- ToDoデータベースに行追加

設定時間: 約10分
料金: 月3つまで無料
```

### 方法4: Androidの場合 - Tasker使用 🤖
```yaml
プロファイル:
- Superwhisperのテキスト共有を検知

タスク:
1. 変数にテキスト保存
2. HTTP PostでNotion APIを叩く
3. 成功通知を表示

必要なもの:
- Taskerアプリ（有料）
- AutoShare プラグイン
```

## 💡 最速セットアップ（15分で完了）

### ステップ1: Notion API設定
```bash
1. https://www.notion.so/my-integrations へアクセス
2. "New integration" をクリック
3. 名前を付けて作成（例: "Superwhisper連携"）
4. API キーをコピー
5. ToDoデータベースで統合を許可
```

### ステップ2: 自動化ツール選択
- **iOS**: ショートカットアプリ（無料・最速）
- **Android**: Tasker（有料だが高機能）
- **両OS**: Zapier（設定簡単）

### ステップ3: テンプレート適用
以下のテンプレートをコピーして使用：

**iOSショートカット用URL:**
```
https://www.icloud.com/shortcuts/[ショートカットID]
```

## 🎯 音声コマンドで完全自動化

### Siri連携（iOS）
```
"Hey Siri, タスク追加"
→ Superwhisper起動
→ 音声入力
→ 自動でNotionに追加
```

### Googleアシスタント連携（Android）
```
"OK Google, Notionにタスク"
→ Taskerプロファイル起動
→ Superwhisper経由で追加
```

## 📊 各方法の比較

| 方法 | 設定難易度 | 費用 | 信頼性 | 速度 |
|------|----------|------|--------|------|
| ショートカット | ★★☆ | 無料 | ★★★ | 高速 |
| Zapier | ★☆☆ | 無料〜 | ★★★ | 普通 |
| IFTTT | ★☆☆ | 無料〜 | ★★☆ | 遅い |
| Tasker | ★★★ | 有料 | ★★★ | 高速 |

## ⚡ トラブルシューティング

### API接続エラー
- APIキーの権限を確認
- データベースIDが正しいか確認
- インターネット接続を確認

### テキストが文字化けする
- UTF-8エンコーディングを指定
- 特殊文字をエスケープ

### 自動化が動作しない
- アプリの通知権限を確認
- バックグラウンド実行を許可
- 省電力モードを解除

## 🚦 今すぐ始めるなら

1. **最も簡単**: Zapier（5分で設定完了）
2. **最も高速**: iOSショートカット（即座に実行）
3. **最も柔軟**: Tasker（Android限定）

これで、音声入力から自動でNotionにタスクが追加されます！