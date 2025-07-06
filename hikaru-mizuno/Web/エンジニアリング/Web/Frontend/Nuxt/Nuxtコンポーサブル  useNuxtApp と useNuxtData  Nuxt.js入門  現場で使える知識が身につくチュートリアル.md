---
URL: https://tech-education-nav.com/contents/educational-materials/nuxt-js/nuxt-nuxtapp-nuxtdata-guide
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/02017047909746a08e5f
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> サーバーサイドとクライアントサイドの両方で利用可能

> アプリ全体で共通して扱いたい情報を保持

> プラグインやミドルウェア、コンポーネント内の setup 関数で useNuxtApp を呼び出す

> アプリケーション全体に影響する設定やヘルパー関数を利用したり、カスタムロジックを実装することが可能

> useNuxtApp

> nuxtApp.$hello('Nuxt User')

> nuxtApp.provide('hello', (name: string) => `Hello ${name}!`)

> 活用例

> hook メソッドを使うことで、Nuxt のレンダリングライフサイクルに介入することが可能

> nuxtApp.hook('page:start', () => {

> サーバーサイドレンダリング時のリクエスト情報や、データ取得結果を格納する payload にもアクセスできます

> ssrContext

> runWithContext
- コンテキストとは、アプリケーションの実行時に必要な状態や設定情報を格納する仕組み

具体例
-リクエスト固有の情報（HTTPリクエスト、レスポンス）
-現在のルート情報
-アプリケーションインスタンス
-プラグインによって注入された値
-SSRやSSG時のレンダリング状態

内部的にAsyncLocalStorageという仕組みを使って、各リクエストごとに独立したコンテキストを管理
===
コンテキストが失われると何が困るのか
1. Composablesが使用できない
2. サーバーサイドでの状態管理の問題
-リクエスト固有の情報にアクセスできない
-異なるリクエスト間で状態が混在する可能性
-SSRでのデータの整合性が保てない
3. プラグインで注入した値にアクセスできない

> キャッシュデータにアクセスする

> useNuxtData

>   // 親ルートでキャッシュした 'posts' のデータにアクセス  const { data: posts } = useNuxtData('posts')
- pages/posts/[id].vue

>   // /api/posts から取得したデータを 'posts' キーでキャッシュする  const { data } = await useFetch('/api/posts', { key: 'posts' })
- pages/posts.vue

> Optimistic Updates（楽観的更新）

> サーバーへのリクエストが完了する前に UI を更新し、成功した場合にバックグラウンドでデータを再取得する


