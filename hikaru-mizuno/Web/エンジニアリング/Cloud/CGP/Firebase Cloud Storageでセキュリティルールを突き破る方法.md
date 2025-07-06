---
URL: https://zenn.dev/akaboshinit/articles/c13b8d3085ac7d
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/430f9e507f2523e37210
Tags: []
Last updated: 2024-12-13
---
#### Highlights & Notes

> 取得したURLは以下のようになり、細部を見るとアクセストークンが付いていることが分かりますhttps://firebasestorage.googleapis.com/v0/b/xxxxx.appspot.com/o/xxxxxxxxxx?alt=media&token=xxxx-xxxx-xxxx-xxxxこの部分です token=xxxx-xxxx-xxxx-xxxx
- アクセストークンには弱点があります

> アクセストークンの弱点弱点とは、アクセストークンが含まれるURLにアクセスするユーザー自体には認証が行われないことです。結果として権限のないBさんでもアクセストークン付きURLでは画像を取得し閲覧ができてしまいますユーザー	権限有無	取得	アクセストークン付きURL閲覧Aさん	true	できる	できるBさん	false	できない	できるまた、そのアクセストークンが無期限の生存期間で一度アクセストークン付きURLが露出してしまえば、誰でも無期限に閲覧できてしまう状態となります。

> 画像URLをFirestoreに保存する
- ダメな使い方１

> 画像がCDN等でキャッシュされる・する
- ダメな使い方２

> もしStorageの画像をStorageのセキュリティルールを通して閲覧したい場合は、FirestoreにはファイルのRefを保存し、閲覧を行うクライアント内でgetDownloadURL()を行う必要があります
- 解決策


