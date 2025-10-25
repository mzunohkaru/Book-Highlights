### Metadata

- Title: MCP Server の Streamable HTTP に関する技術要素
- URL: https://zenn.dev/demouth/articles/6f6b0a3f67938c
- Last Updated on: 2025-10-24



### Highlights & Notes

- 1リクエスト・1レスポンスという訳ではなく、お互いがお互いのタイミングで会話をしていくイメージ
- どちらか片方が連続で送信しているケースがあるのが重要なポイント
![[StreamableHTTP-シーケンス図.png]]

### stdio の場合はとてもシンプル
- 普通に標準入出力を使って相互に会話をすればよい
- ローカル上で実行されるので基本的には認証認可について考えることもありません

### 上記の仕様をHTTPを使って実現する
- 誰もが等しくMCPサーバーを使っていい場合もあれば、リクエストしてきたユーザー毎にレスポンスする内容を変えたい場合もあると思います。これを実現するために OAuth 2.0/2.1 を使い認証認可を行います。
#### SSE (Server-sent events) について
- SSE はHTTPサーバー側（つまりMCPサーバー側）が HTTP の Transfer-Encoding: chunked を使ってレスポンスすることで、サーバー側からデータを分割して送信できる仕組み
- サーバー側は任意のタイミングでクライアント側にデータを通知することができます
- 例ではサーバー側から別々のタイミングで２回データを送信しています。（この例では２回送信していますが、実際は何回でも送信できます）
- クライアントとサーバーがHTTP接続を継続した状態で待機することで、サーバー側から通知が必要になったタイミングで、サーバー側からPUSH通知を送信することができます
- SSEの重要な点としては、サーバー側からPUSHすることしかできない点
- 一番最初にクライアント側からリクエストを送信した以降、クライアントからの送信はできないので、クライアント側はデータを受け取るだけの状態になります。
```
Client -[POST/mcp]-> Server
Client <-[  data  ]- Server
Client <-[  data  ]- Server
```


#### 「Streamable HTTP」についてざっくりと説明
- 普段は一般的なHTTPを使う
	MCPサーバー側からPUSH通知をしたいときはSSEを使う
- MCPサーバー側からPUSH通知を送信する要件がなければ、MCPサーバー側はSSEを実装する必要はない

SSE
![[StreamableHTTP-SSE.png]]

SSEなし
![[StreamableHTTP-SSEなし.png]]

### Streamable HTTP についてもう少し深掘り
- ポイント１: Streamable HTTP とは 「HTTP または SSE」
	- 「Streamable HTTP」ではHTTPとSSEを使い分けます。
	- クライアント側は Accept: application/json, text/event-stream または Accept: text/event-stream とリクエストします
	- サーバー側でSSEが不要と判断したら Content-Type: application/json でレスポンスします
	- サーバー側でSSEが必要と判断したら Content-Type: text/event-stream でレスポンスします
- ポイント２： エンドポイントパスは1つ
	- エンドポイントのパスは https://example.com/mcp のような1つのみです。RESTのように複数のパスをついつい想像してしまいますが、そうではないです。
- ポイント３: GETメソッドとPOSTメソッドを使う
	- GETメソッド
		- 常に Server-Sent Events（SSE）を使います
		- MCPサーバーからの通知をMCPクライアントが受け取るため、MCPクライアント側からGETリクエストを送信してSSE接続を確立しておきます
	- POSTメソッド
		- MCPクライアント側からMCPサーバーに要求を伝えたい場合はPOSTを使います
		- SSEをつかってもいいし、使わなくてもいいです（SSEを使わない場合 Content-Type: application/json で応答する）
## 認可
- 認証認可にはOAuthを使う
※2025/08/28時点の仕様
- OAuth 2.1 IETF DRAFT (draft-ietf-oauth-v2-1-13)
- OAuth 2.0 Authorization Server Metadata (RFC8414)
- OAuth 2.0 Dynamic Client Registration Protocol (RFC7591)
- OAuth 2.0 Protected Resource Metadata (RFC9728)

### 認可の処理プロセス
1. MCPの初回起動じにDynamic Client RegistrationでClient情報を登録する
2. MCPサーバー（リソースサーバー）にMetadataを問い合わせ認可サーバーを発見する
3. ローカル上で認可サーバーを受け取るWebサーバーを起動する
4. ブラウザで認可サーバーを開き、ユーザーが認可をする
5. ローカルで立ち上げておいたWebサーバーで認可コードをうけとる
6. 受け取った認可コードと、事前に登録したクライアント情報を使ってアクセストークンを発行する
