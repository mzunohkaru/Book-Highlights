---
URL: https://tech-education-nav.com/contents/educational-materials/vue/vuejs-watchers
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/c8def4a88b881c301316
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> ウォッチャーのオプション

> immediate：即時実行する

> deep：ネストされたプロパティを監視する

> flush：更新のタイミングを制御する

> デフォルトでは、ウォッチャーのコールバックはVue.jsの更新サイクルの「レンダリング後」に実行され

> 変更したい場合は、flushオプションを使い

> 'pre'：Vue.jsのレンダリング前にコールバックを実行'post'：レンダリング後に実行（デフォルト）'sync'：データが変更されたとき同期的に実行

> ウォッチャーでは、様々な種類のリアクティブなデータを監視する

> ウォッチャーの停止

> 通常はコンポーネントが破棄されるとき（ページから消えるときなど）に自動的に停止

> 特定の条件で手動で停止したい場合は、ウォッチャーが返す停止関数を使用
- stopWatching();


