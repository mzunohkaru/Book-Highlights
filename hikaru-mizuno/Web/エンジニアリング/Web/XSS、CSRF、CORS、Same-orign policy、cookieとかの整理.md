---
URL: https://zenn.dev/dove/articles/3dc0b8603db3fd
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/b67e3e5d23835095d546
Tags: []
Last updated: 2025-06-24
---
#### Highlights & Notes

> Cross Site Scripting(XSS)

> 「ユーザーから受け取った文字列をそのまま画面に表示するサイト」をねらった攻撃

> 画面にデータを出力する際は、常にHTMLをサニタイジング(エスケープ処理)をする

> 対応策

> まず覚えること

> ![](https://storage.googleapis.com/zenn-user-upload/d499e5eff7c6-20230115.png)

> ![](https://storage.googleapis.com/zenn-user-upload/20e5586eac63-20230115.png)

> cookieの振る舞い

> まず覚えること

> サーバー側がブラウザにHTTPレスポンスヘッダを通してcookieをセットし、次回以降のリクエストでcookieを要求するcookieはブラウザにドメイン単位で保存されるそのドメインにHTTPリクエストが送信される際に、他のドメインのサイトからのリクエストであっても、cookieが自動的に送信される。例外あり(SameSiteフラグ)。

> cookieを使ってセッション管理しているサイトに対しては、2種類の攻撃をすることができる

> 適当にcookieにセッション変数を入れることで、他の誰かのセッションに不法にあいのりする攻撃(セッションハイジャック攻撃)そのサイトに対してHTTPリクエストが送信される際に、他のドメインのサイトからのリクエストであっても、自動的にcookieが送信されることを悪用した攻撃(CSRF攻撃)

> セッションハイジャック攻撃について。

> ![](https://storage.googleapis.com/zenn-user-upload/9d23aef27655-20230115.png)

> Same-origin poilcy

> まず覚えること

> 異なるドメインに対してのGETリクエストを禁止するブラウザ側の挙動サーバー側のCORSの設定でこの挙動を上書きすることができるPOSTリクエストには効果がないレスポンスは失敗するがリクエストは成功してしまうプリフライトが飛ぶならCORSが適用されるため防ぐことはできるプリフライトが飛ぶかどうかはリクエストの条件次第

> CORSとプリフライトリクエスト

> まず覚えること

> CORSはSame-origin policyの挙動を上書きして許可する(ホワイトリスト形式)サーバー側で設定するレスポンスヘッダにAccess-Control-Allow-Originで許可ドメインを指定する異なるドメイン間のリクエストの場合、ブラウザはまずCORSプリフライトリクエストをHTTPメソッドOPTIONで送信して、CORSチェックをし、許可されていれば実際のリクエストを飛ばす。ただし、CORSプリフライトレクエストは単純なリクエストの場合は飛ばない。CORSチェックをスキップできる。単純なリクエストは基本的にapplication/jsonでやり取りしているAPIリクエストでは発生しないが、formを使ったリクエストの場合だと発生する。単純なリクエストはブラウザの古い仕様formとの互換性のため残ってる

> プリフライトリクエストという特殊なHTTPリクエスト

> ブラウザが自身とは異なるドメインにHTTPリクエストを送信する際に、前もって自動的に送信されます。HTTPメソッドはOPTIONSです。このプリフライトリクエストの結果、サーバーがこのドメインからのリクエストが許可しているかどうかを知ることができます。許可されていれば、ブラウザは本番のリクエストを送信します。

> formを使ったリクエストは単純なリクエストになりやすく、CORSチェックを回避してしまう
- POST リクエストで Content-Type: text/plain を使用 データをJSON形式ではなく、プレーンテキストで送信

> 具体的に

> CSRF

> まず覚えること

> CSRFはブラウザの挙動を利用しつつ、セキュリティの抜け道を巧みに使った攻撃Same-origin policyやCORSのぬけ穴を利用特にcookieは、異なるドメインからのリクエストであっても、自動的に送信されるため、これを悪用SameSiteフラグでこの挙動を制御できる。最近の最新のブラウザであれば、デフォルトで有効になっているはず。

> 対応策

> cookieを使ったセッションを利用するなら、formにはCSRFトークンをつけ、cookieのSameSiteフラグは有効にしておく使わないContent-Typeは弾くリスクの高い操作の前にはパスワード入力を求める
- 1. 目的の違い
- CSRFトークン
偽造されたリクエストを防ぐ
- アクセストークン
ユーザーの認証・認可

2. 生成タイミング
- CSRFトークン
フォーム表示時やページロード時に生成 リクエストごとに新しく生成されることが多い
- アクセストークン
ログイン成功時に生成 リフレッシュトークンで更新 認証プロセス完了後に発行

3. 有効期限
- CSRFトークン
短時間（数分〜数時間）
- アクセストークン
中〜長時間（数時間〜数日） リフレッシュ機能で延長可能

4. 送信方法
- CSRFトークン
HTTPヘッダー: X-CSRF-Token: abc123
- アクセストークン
Authorization ヘッダー: Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

5. 保存場所
- CSRFトークン
サーバー: セッションストレージ、メモリ、Redis等 クライアント: DOM（隠しフィールド）、メタタグ、JavaScript変数
- アクセストークン
サーバー: データベース、Redis、メモリ（JWTの場合は不要） クライアント: localStorage、sessionStorage、Cookie、メモリ
===
Content -Typeは弾く
JSONデータはapplication/jsonでのみ受け付ける

> ![](https://storage.googleapis.com/zenn-user-upload/aeaf41f14298-20230115.png)

> 原因

> Same-origin policyとCORSがformに対しては抜け穴があるcookieが異なるドメインからでも自動送信されてしまう

> 対応策

> cookieを使う場合は、CSRFトークンで対策するcookieが異なるドメインから自動送信されないようにSameSiteフラグを有効にしておく

> セッション管理 cookie vs LocalStrage vs InMemory

> cookieの強み

> JSからアクセスできないように設定できるので、XSSで侵入されたときにセッション変数を盗まれない追加コードなしで、有効期限を設定できるcookieの弱み異なるドメインから自動送信されてしまう。ただし最近は、ブラウザベンダがSameSiteフラグが設定されていなければデフォルトで有効にする方向へ方針を変えていっている。具体的には各ブラウザの対応を調査したほうがよさそう。

> LocalStorageの強み

> JSとの相性が良いLocalStorageの弱みおなじドメインであればJSからアクセスできる。XSSで侵入されたときにアクセストークンを盗まれる有効期限を設定できない。アクセストークン自体に期限を保持しておき、リクエスト時にコードで期限切れを判別する必要あり。


