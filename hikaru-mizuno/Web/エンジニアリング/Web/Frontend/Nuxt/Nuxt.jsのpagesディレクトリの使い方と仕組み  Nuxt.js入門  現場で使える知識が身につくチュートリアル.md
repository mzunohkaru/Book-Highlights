---
URL: https://tech-education-nav.com/contents/educational-materials/nuxt-js/nuxt-pages-directory
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/1d3f1de6c85fca55e9f7
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> ファイル名に角括弧（[]）を使用することで、パスの一部をパラメータとして抽出

> $route.params.id によってURLからidの値を取得

> キャッチオールルート

> 動的ルート

> ファイル名に三点リーダー（...）を使用して作成

> 使用する場面

> 404ページ: 存在しないURLをキャッチしてカスタム404ページを表示。ブログやCMSの動的ルーティング: /blog/some-post や /blog/category/some-post のような階層的なパスを一括処理。多言語対応: /en/hello や /ja/こんにちは などのパスを一括キャッチして処理

> ネストされたルート

> 親ページの中に子ページを表示する構造

> 親ページに<NuxtPage />を挿入し、子ルートの内容をレンダリング

> 使用する場面

> 管理画面の構築:/admin (ダッシュボード)/admin/users (ユーザー管理)/admin/settings (設定)階層的なナビゲーション:/products (商品一覧)/products/category (カテゴリ別商品)/products/category/item (個別商品詳細)

> ページメタデータの設定

> definePageMetaを使用して各ページごとのメタデータを設定

> navigateTo

> .client.vueを使用して、クライアントサイドのみでレンダリングされるページ

> .server.vueを使用して、サーバーサイドでのみレンダリングされるページ


