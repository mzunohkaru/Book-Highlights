---
URL: https://tech-education-nav.com/contents/educational-materials/vue/vuejs-async-components
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/fa06d560771af0f676da
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> defineAsyncComponent 関数を使って非同期コンポーネントを簡単に定義する

> 非同期コンポーネントのユースケース

> アプリケーションの初期ロード時にすべてのコンポーネントを読み込まず、ユーザーが必要としたときにだけコンポーネントを読み込むために使われます。

> モーダルウィンドウやタブ切替など、ユーザーが特定の操作をしたときにのみ表示されるコンポーネントを非同期にすることで、不要なリソースの読み込みを避け

> コンポーネントの読み込み中にローディング状態を表示したり、読み込みに失敗した場合にエラー表示を行いたい場合

> defineAsyncComponent にはそのためのオプションが用意されて

> const ModalWindow = defineAsyncComponent(() =>
- defineAsyncComponent内に非同期コンポーネントのインポートをする。


