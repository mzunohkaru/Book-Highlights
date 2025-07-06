---
URL: https://tech-education-nav.com/contents/educational-materials/vue/vuejs-reactivity-basics
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/39a7c33ef217ed39ab73
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> リアクティビティの落とし穴と解決策

> reactiveオブジェクトの分割代入

> reactiveオブジェクトからプロパティを分割代入すると、リアクティビティが失われます。

> 解決策：toRefsを使用することで、リアクティビティを保ったまま分割代入できます。

> 配列の要素を直接変更する

> 解決策：配列のメソッドを使うか、新しい配列を代入します。

> 配列の要素を直接インデックスで変更すると、変更が検知されない場合があります。

> 新しいプロパティの追加

> リアクティブオブジェクトに新しいプロパティを後から追加しても、自動的にはリアクティブにならないことがあります。

> 解決策：初期化時に全てのプロパティを定義しておくか、Object.assignを使用します。

> テンプレートで{{ counter }}と書いた場合、Vueは「このコンポーネントはcounterに依存している」と記録します。後でcounter.valueが変更されると、そのコンポーネントだけが再レンダリングされます。これにより、必要な部分だけを効率よく更新できる


