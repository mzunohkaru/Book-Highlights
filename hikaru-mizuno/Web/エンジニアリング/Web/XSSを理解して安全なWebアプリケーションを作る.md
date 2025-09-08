---
URL: https://zenn.dev/oreo2990/articles/d33a264b2d8b4c
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/110fea6921bc629f8d6f
Tags: []
Last updated: 2025-07-21
---
#### Highlights & Notes

> サニタイズ処理(エスケープ処理)

> 特別な意味を持つメタ文字を無効化し XSSを防ぐ

> 例えば<→&lt;、>→&gt;のようにタグを無害な文字列に置き換える

> DOM-based XSS(DOMベース型XSS)

> 動的にHTMLを生成する際に、悪意あるスクリプトが注入/実行される攻撃手法

> v-htmlなどでは、悪意あるスクリプトが注入され攻撃される可能性があり

> Reflected XSS(反射型XSS)

> 悪意あるコードをクエリパラメータとしてURLに埋め込み、脆弱性のあるサイトに誘導して攻撃を行います。

> Persistent XSS(持続型XSS)

> 攻撃者が悪意あるスクリプトを含むコメントを投稿し、そのデータがWebサーバーに格納されます。ユーザーが掲示板を開いた際に、ユーザーのブラウザ上でスクリプトが実行されてしまいます。

> 対策

> 下記3つの攻撃手法

> 属性値は引用符で囲む

> value="$data"

> 入力値のバリデーション

> CookieにHttpOnly属性を加える

> URLのスキームチェック

> URL生成前にスキーマがhttpまたはhttpsのみを受け付けるようにチェックする

> Set-CookieフィールドにHttpOnly属性を加えると、JavaScript がDocument.cookieなどでCookieへのアクセスを禁止することができ、Cookie情報漏洩のリスクが軽減


