---
URL: https://zenn.dev/comm_vue_nuxt/articles/nuxt-use-fetch-guide?redirected=1
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/a1f0ff74fc842be288df
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> useFetch は、おもに次のようなシーンで利用する

> コンポーネント内の描画に使用するデータを取得するときUniversal Rendering (SSR) におけるデータ取得を行うとき（サーバーサイドでデータを取得した結果をクライアントでも扱えるようにするなど）リアクティブな値に連動して、自動的に再取得を行うとき一度取得したキャッシュを使用したり、必要に応じて再取得したいとき

> $fetch

> リクエスト時のパラメーター等をよしなに処理してくれたり、取得したレスポンスを（JSON等で）パースしてくれたりする

> $fetch のほうがふさわしい場面

> サーバー側 (server/api など) でデータを取得するページ内の button をクリックしたときに、サーバー側に何らかのデータを POST し、その後別のページに遷移するアプリケーションで扱う値とは別のデータ取得やPOSTの処理

> ユーザーのアクションに応じた処理を行う場合は $fetch が適しています（取得したデータを引き続きコンポーネント内で扱う場合は useFetch で構いません）

> たとえば何かしらのログを一回だけ送信するといった処理を行う場合


