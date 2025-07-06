---
URL: https://tech-education-nav.com/contents/educational-materials/nuxt-js/nuxt-middleware-guide
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/149a003e44dcf6e335c1
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> Middlewareとは、ルート遷移前に特定の処理を挟む仕組み

> 匿名ミドルウェア（Anonymous Middleware）ページ内に直接定義されます。名前付きミドルウェア（Named Middleware）middleware/ディレクトリ内に定義され、名前で呼び出します。グローバルミドルウェア（Global Middleware）middleware/ディレクトリ内に.globalサフィックスをつけて定義され、すべてのルート遷移で実行されます。

> 名前付きミドルウェアの作成

> middleware/ディレクトリにファイルを追加

> 実行順序

> middleware/内でアルファベット順に実行

> 必要に応じてファイル名に番号を付けて順序を制御します（例: 01.setup.global.ts）

> ページ定義のミドルウェア

> definePageMetaで指定された順序で実行

> ミドルウェアの目的を明確化認証、分析、エラーハンドリングなどの用途に応じてミドルウェアを分けます。順序を明示的に管理ファイル名に番号を付けて実行順序を制御します。グローバルミドルウェアの適量を維持必要以上に多くの処理を追加しないように注意します。エラーを適切にハンドリングabortNavigation(error)を活用して、エラー発生時にユーザーにフィードバックを提供します。

> ミドルウェアのベストプラクティス

> ミドルウェアの適用

> definePageMeta({  middleware: 'auth',})

> グローバルミドルウェア

> ファイル名に.globalを追加


