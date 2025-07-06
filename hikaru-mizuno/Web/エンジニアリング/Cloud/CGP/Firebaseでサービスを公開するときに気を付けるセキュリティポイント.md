---
URL: https://zenn.dev/singularity/articles/firebase-security
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/8de1ee6c8cadada6cd59
Tags: []
Last updated: 2024-12-15
---
#### Highlights & Notes

> 再帰ワイルドカード ({document=**})再帰ワイルドカードが定義されていると、そのルールが定義よりも深い階層全て適用されるため、想定をしていない権限が許可されている事があるので、利用には注意する

> ユーザが更新すべきでない、参照すべきでないデータは、Functionsで読み書きすることも考える

> １つのdocumentに、ユーザが書き換え可能なものと、システムが更新するものは極力混ぜない

> 混ざった場合には security ruleでユーザがその項目を更新したり、create時にその項目を勝手に設定できないように気を付ける

> 自社システムをアタックするハックデイを開催してもよい。

> uploadされた画像は、resizeなどをして、そのときにexifを削除して他のユーザへ表示するようにして、オリジナルの画像は表示させない
- 写真を撮影した際に、位置情報などが画像ファイルのメタデータに含まれている。
このメタデータの流出を避ける必要がある。
https://zenn.dev/oubakiou/articles/ee01b03d0c24e8

https://firebase.google.com/docs/functions/gcp-storage-events?hl=ja&gen=2nd#download-transform

> Storage.rulesでwrite権限やファイルサイズ、ファイル種別の制限をして、ユーザが不法なファイルをアップロードできないようにする。

> HTTP Headerを設定して、不正なリクエストを防ぐ(Functionsでexpressを使っている場合は、そちらも忘れずに)

> APIのDoS対策にrate limitを導入する
- https://firebase.google.com/docs/functions/quotas?hl=ja

> 例えば特権ユーザを作るなら、customClaimを使うなどして、他のユーザがFunctionsを叩いても動作しないようにする

> DoS攻撃で課金が増える可能性があるので、インスタンスの最大数の上限を設定しておくmaxInstancesを指定する

> インフラ周りはCloud Logging

> functionsのログは、functions.loggerを活用

> FirestoreやStorageのTriggerでFunctionsを呼び出すときは、Loopしていないか確認をする無限課金される


