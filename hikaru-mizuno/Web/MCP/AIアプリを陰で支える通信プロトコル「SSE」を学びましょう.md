### Metadata

- Title: AIアプリを陰で支える通信プロトコル「SSE」を学びましょう
- URL: https://zenn.dev/team_soda/articles/c6f6e14e796da5
- Last Updated on: 2025-10-23



### Highlights & Notes

- AIからの回答をストリーム表示するのにSSEを使っています

## SSEとは
- サーバー送信イベント（Server-Sent Events、SSE）は、HTTPプロトコルを使用してサーバーからクライアント（ウェブブラウザなど）へ一方的にデータをストリーム配信するための標準技術
- SSEは、サーバーからクライアントへの単方向通信を提供

- WebSocketは、サーバーとクライアント間の双方向通信を提供

## MCPの通信プロトコルとしてのSSE
- 現在はSSEはStreamable HTTPという、独自のプロトコルに置き換えられています。
- SSEはStreamable HTTPで引き続き内部的に利用されています

## 入門 Streamable HTTP
- HTTPのPOSTとGETリクエストを利用し、必要に応じてServer-Sent Events (SSE)を使って複数のサーバーメッセージをストリーミングできます。
### 主要な特徴と仕組み
- 単一のMCPエンドポイント: サーバーは、POSTとGETの両方をサポートする単一のHTTPエンドポイントを提供する必要があります。
#### クライアントからサーバーへのメッセージ送信(HTTP+α)
- クライアントはJSON-RPCメッセージをHTTP POSTリクエストとしてMCPエンドポイントに送信
	- Acceptヘッダーにapplication/jsonとtext/event-streamの両方を指定する
	- リクエストボディには、単一のJSON-RPCリクエスト/通知/レスポンス、またはそれらのバッチを含めることができます
- サーバーは、リクエストにJSON-RPCリクエストが含まれている場合、SSEストリームを開始するか、JSONオブジェクトを返す
#### サーバーからクライアントへのメッセージ受信(HTTP SSE)
- クライアントはHTTP GETリクエストをMCPエンドポイントに発行してSSEストリームを開き、サーバーからの通知やリクエストを受信
- サーバーはこのGETリクエストに対してContent-Type: text/event-streamを返すか、405エラーを返します
#### セキュリティと堅牢性
- DNSリバインディング攻撃を防ぐため、サーバーはOriginヘッダーの検証、localhostへのバインド、および適切な認証の実装が必須または推奨
- 接続が切断された場合、サーバーはSSEイベントにIDを付与することで、クライアントがLast-Event-IDヘッダーを使用してストリームを再開し、未送信のメッセージを再配信できるようになります
- サーバーは初期化時にMcp-Session-IdヘッダーでセッションIDを割り当てることができます。クライアントは以降のリクエストでこのセッションIDを含める必要があります。クライアントは、不要になったセッションをHTTP DELETEリクエストで明示的に終了できます。
