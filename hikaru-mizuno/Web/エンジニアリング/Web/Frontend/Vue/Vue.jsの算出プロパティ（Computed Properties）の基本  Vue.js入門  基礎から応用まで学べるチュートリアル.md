---
URL: https://tech-education-nav.com/contents/educational-materials/vue/vuejs-computed-properties
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/4f30e772734d412643bd
Tags: []
Last updated: 2025-06-30
---
#### Highlights & Notes

> 依存するデータが変更された場合にのみ再計算される
- computed → 派生値の計算（値を返す、リアクティブ、キャッシュされる）
watch → 副作用の実行（値を返さない、変更を監視して処理を実行）

> 多くの場合、計算結果を表示したいだけなら算出プロパティが適して

> 算出プロパティとメソッドの使い分け

> 通常、算出プロパティは読み取り専用ですが、必要に応じて書き込み可能にすることもできます

> 書き込み可能な算出プロパティ

> 算出プロパティを使う際の注意点

> 算出プロパティ内でデータを変更したり、非同期処理を行ったりすべきではありません。そのような操作はメソッドやウォッチャーを使用しましょう。

> 一つの算出プロパティは一つの計算に集中すべき

> 複雑な計算は、複数の算出プロパティに分解する


