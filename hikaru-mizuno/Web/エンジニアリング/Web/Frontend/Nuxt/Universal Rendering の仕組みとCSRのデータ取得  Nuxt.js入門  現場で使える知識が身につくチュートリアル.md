---
URL: https://tech-education-nav.com/contents/educational-materials/nuxt-js/ssr-csr-data-fetching
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/d1623ac5b3366bcd60cc
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> Universal Rendering の仕組み

> ユーザーがページにアクセスしたとき、サーバー側でページの HTML を完全に生成し、その HTML と共に必要なデータをクライアントに送る仕組み

> ユーザーが Nuxt.js アプリケーションのページにアクセスすると、まずサーバーがそのリクエストを受け取り、ページを構築するための Vue コンポーネントをレンダリングします

> API にアクセスし、取得したデータを変数（Vue の ref として管理）に保存します。取得されたデータは、Nuxt Payload として HTML に埋め込まれ、クライアントに送信されます。

> 「Nuxt Payload」と呼ばれる特別な JavaScript オブジェクトにまとめられます

> クライアント側では同じデータを再度 API から取得する必要がなく、すでにサーバー側で取得されたデータをそのまま利用できる

> Hydration のプロセス

> サーバーが生成した静的な HTML を、クライアント側で Vue.js のリアクティブな状態に変換（「ハイドレート」）するプロセス

> CSR の場合のデータ取得

> すべてのデータ取得がクライアント側で行われ

> 問題点: データの二重取得

> server: false オプション

> 非SEO 対象の動的データ（例えば、ユーザー固有の情報や頻繁に変わるデータ）に適して


