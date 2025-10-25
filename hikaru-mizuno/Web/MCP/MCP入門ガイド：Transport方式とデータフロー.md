### Metadata

- Title: MCP入門ガイド：Transport方式とデータフロー
- URL: https://zenn.dev/acntechjp/articles/d0ae655d68b82a
- Last Updated on: 2025-10-24



### Highlights & Notes

- stdio方式ならローカル処理だから安全」という誤解が広まっていますが、これは正しくありません
- ローカル処理ならstdio、外部サービス連携ならHTTP

### 動作の流れ
- 初回実行時
	1. コードをダウンロード
		- npm/PyPI からパッケージ取得
		- ⚠️ コードのみ（データは送信されない）
	2. ローカルにキャッシュ
- 2回目以降
	- キャッシュから即座に実行

### 実装言語
- npx: JavaScript/TypeScript製（例：@modelcontextprotocol/server-filesystem）
- uvx: Python製（例：markitdown-mcp）⚡ 10倍高速

## stdio方式
```
STEP 1: あなたの入力
「このPDFをMarkdownに変換して」
    ↓
STEP 2: MCPホスト（Claude Code等）
- ローカルで動作
    ↓ LLM API（インターネット経由）
STEP 3: ⚠️ LLMプロバイダー（Anthropic等）
- 入力を解析し「MCPサーバーを使え」と判断
    ↓ 指示を返す
STEP 4: MCPホスト
- MCPサーバーを起動
STEP 5: ✅ MCPサーバー（ローカル）
- PDFを読み取り、Markdownに変換
- ※この段階では外部送信なし
    ↓ 処理結果を返す
STEP 6: MCPホスト
- 結果を受け取る
    ↓ LLM API（処理結果を含む）
STEP 7: ⚠️ LLMプロバイダー
- 変換されたMarkdownテキストを受信
- 解析して応答を生成
    ↓
STEP 8: あなたに結果表示
```

## HTTP方式（リモート）
```
STEP 1: あなたの入力
「Notionにページを作成して」
    ↓
STEP 2-3: ⚠️ LLMプロバイダー
- 「Notion MCPを使え」と判断
    ↓ HTTPS（インターネット経由）
STEP 4-5: ⚠️ Notion社のMCPサーバー
- リクエストを受信、Notionデータベースに保存
    ↓ 結果を返す
STEP 6-7: ⚠️ LLMプロバイダー
- Notionの処理結果を受信
```