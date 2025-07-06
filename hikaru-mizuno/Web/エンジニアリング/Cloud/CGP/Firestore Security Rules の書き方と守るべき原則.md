---
URL: https://zenn.dev/kosukesaigusa/articles/efc2528898954d95a6ae
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/95d3b24afe3bd52d1ca1
Tags: []
Last updated: 2024-12-10
---
#### Highlights & Notes

> rules_version = '2';service cloud.firestore {    match /databases/{database}/documents {        match /{document=**} {            allow read, write: if true;        }    }}
- ドキュメント全てにルールを当てれる

> readget：単一のドキュメントの取得list：クエリによるコレクション・複数ドキュメントの取得writecreate：ドキュメントの生成update：ドキュメントの一部のフィールドの更新delete：ドキュメントの削除

> create と update すらも、ルールを区別することを怠って全く同一の条件で許可するケースはほぼない、と認識しておきましょう

> スキーマ検証とデータのバリデーションも、Firestore Security Rules でチェックすべき大切な要素

> ルール（allow <読み書きのオペレーション>: if <許可する条件>;）を書くということは、セキュリティに穴をひとつずつ開けていくことに等しいと認識しておく
- 許可すること自体がセキュリティに穴を開ける行為である

> update オペレーションについては、create オペレーションと類似のルールとなることはすぐに分かりますが、必ずしもすべてのフィールドが更新されるわけではありません。
- createdAtなど


