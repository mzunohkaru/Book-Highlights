---
URL: https://tech-education-nav.com/contents/educational-materials/vue/pinia-advanced-topics
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/c265c272e0e85df704fa
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> 1. 手動でローカルストレージを実装する

> const stateStr = localStorage.getItem('counter-store');

> localStorage.setItem('counter-store', JSON.stringify(state));

> 2. プラグインを使った状態永続化

> pinia-plugin-persistedstateというプラグイン

>   // 永続化の設定  persist: true

> // プラグインの設定pinia.use(piniaPluginPersistedstate);

> Piniaとプラグインの設定

> プラグインを使ったストアの定義

> ストアの状態が自動的にローカルストレージに保存される

> プラグインを使用したストアは、特別な処理なしに通常のストアと同じように使えます


