### Metadata

- Title: MCPのOAuth2認可フローをAmazon Bedrock AgentCore Gatewayで検証してみる
- URL: https://zenn.dev/aws_japan/articles/20373296118717
- Last Updated on: 2025-10-25



### Highlights & Notes
## Remote MCPの標準的な認可フロー

### ドキュメント
1. MCP ClientからMCP Serverにトークンなしでアクセスした際、HTTP401とともにヘッダーにResource metadata の問い合わせ先が入った状態で買ってくる（RFC9728）
2. 1で得たエンドポイントに、認可サーバーの情報を問い合わせ、取得する（RFC9728）
3. 認可サーバーに対してAuthorization Server Metadataを問い合わせ、取得する（RFC8414）
4. Authorization Server Metadataから取得した`/register` エンドポイントに、クライアント登録のリクエストを送り、認可サーバー側でクライアント登録が完了する(RFC7591)
5. Authorization Server Metadataから取得した`/authorize`エンドポイントに認可リクエストを送り、認証ページにリダイレクトする
6. 認証が完了し、アクセストークンを得た状態で元のページに遷移する

### 検証
- MCP Server側のログ
```
💡トークンを入れない状態でMCPにリクエストをかけると、401のUnauthorized responseとresource metadataが埋め込まれたヘッダーが返ってくる
[wrangler:info] GET /sse 401 Unauthorized (7ms)
💡resource metadataに付与されたエンドポイントに対して、具体的な認可サーバーの場所が記載されたmetadataを問い合わせ
[wrangler:info] GET /.well-known/oauth-protected-resource 404 Not Found (2ms)
💡issuer, authorization_endpoint, token_endpointなどが含まれたmetadataを認可サーバーにリクエスト
[wrangler:info] GET /.well-known/oauth-authorization-server 200 OK (1ms)
[wrangler:info] GET /.well-known/oauth-protected-resource/sse 404 Not Found (1ms)
[wrangler:info] GET /.well-known/oauth-protected-resource 404 Not Found (1ms)
[wrangler:info] GET /.well-known/oauth-authorization-server/sse 404 Not Found (1ms)
[wrangler:info] GET /.well-known/oauth-authorization-server 200 OK (2ms)
💡ClientをMCP側の認可サーバーに登録してもらうようにリクエスト
	- URLが返却されブラウザが遷移している
	- PKCEの形式にそってクライアント側で生成したcode_challengeを送っています
[wrangler:info] POST /register 201 Created (11ms)
[wrangler:info] GET /authorize 200 OK (9ms)
💡ブラウザ上では、`/authorize` のレスポンスとして(例)GitHubへのアクセス許可を確認するページを返してブラウザ上で表示されている
http://localhost:8788/authorize?response_type=code&client_id=xxxx&code_challenge=xxxx&code_challenge_method=S256&redirect_uri=http%3A%2F%2Flocalhost%3A6274%2Foauth%2Fcallback&state=xxxx
💡AprroveするとGithubの認証エンドポイントにリダイレクト
💡ログインすると、アクセストークンが付与された状態でもともと開いていたMCP Inspectorの画面にcallback
http://localhost:6274/oauth/callback?code=kishi-k%3A{client_id}%{secret}&state=xxxxxxxx
💡Toolsをアクセスすると、上記のトークン情報をヘッダーに入れてアクセスできている
POST /sse/message?sessionId=3fddd2a701911972a706df7959e7e331c8965a6342cb116f77c5218bbf768e28 HTTP/1.1
Accept: text/event-stream
Authorization: Bearer kishi-k:{client_id}:{secret}
```
