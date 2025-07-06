---
URL: https://tech.ginco.io/post/ginco-engineer-meetup-2018-cloud-functions/
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/2c46dce1edce65aa3505
Tags: []
Last updated: 2024-12-15
---
#### Highlights & Notes

> Cloud Functionsの仕組み

> 1 リクエストに対し 1 インスタンスで処理する

> 新しくリクエストを受け取った場合空いているインスタンスがあればそのインスタンスにリクエストを渡す空いているインスタンスが無ければ新しくインスタンスを作成し、そのインスタンスにリクエストを渡す

> しばらくリクエストを処理していないインスタンスがある場合、そのインスタンスは削除される

> 実行対象の関数のモジュールのみをロードする

> Cloud FunctionsのCold Startの改善

> そこで index.js を下記のように修正しました。これにより、起動する関数の export 文のみが評価されます。Cloud Functions では、実行時に環境変数 FUNCTION_NAME に実行対象の関数名が入ります。そのため、firebase コマンドでデプロイする際には全ての関数が評価され、関数の実行時にはその関数のみが評価されます

> if (!process.env.FUNCTION_NAME || process.env.FUNCTION_NAME === 'Func1') {    exports.Func1 = require('./funcs/func1');}if (!process.env.FUNCTION_NAME || process.env.FUNCTION_NAME === 'Func2') {    exports.Func2 = require('./funcs/func2');}
- 環境変数は更新されています。
FUNCTION_NAMEは、使用できません。

https://qiita.com/_mogaming/items/edd6024ca0cdd8d3cd93

> 依存モジュールのバージョンアップ


