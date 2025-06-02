# Firebase Cloud Storageでセキュリティルールを突き破る方法

![](https://res.cloudinary.com/zenn/image/upload/s--3mj1CpjY--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Firebase%2520Cloud%2520Storage%25E3%2581%25A7%25E3%2582%25BB%25E3%2582%25AD%25E3%2583%25A5%25E3%2583%25AA%25E3%2583%2586%25E3%2582%25A3%25E3%2583%25AB%25E3%2583%25BC%25E3%2583%25AB%25E3%2582%2592%25E7%25AA%2581%25E3%2581%258D%25E7%25A0%25B4%25E3%2582%258B%25E6%2596%25B9%25E6%25B3%2595%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:akaboshinit%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2QzZGNiZGEzNTIuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Firebase Cloud Storageでセキュリティルールを突き破る方法
- URL: https://zenn.dev/akaboshinit/articles/c13b8d3085ac7d
- Last Updated on: 2024-12-13



### Highlights & Notes

- 取得したURLは以下のようになり、細部を見るとアクセストークンが付いていることが分かります
	https://firebasestorage.googleapis.com/v0/b/xxxxx.appspot.com/o/xxxxxxxxxx?alt=media&token=xxxx-xxxx-xxxx-xxxx
	この部分です token=xxxx-xxxx-xxxx-xxxx
  - Notes: アクセストークンには弱点があります
- アクセストークンの弱点
	弱点とは、アクセストークンが含まれるURLにアクセスするユーザー自体には認証が行われないことです。
	結果として権限のないBさんでもアクセストークン付きURLでは画像を取得し閲覧ができてしまいます
	ユーザー	権限有無	取得	アクセストークン付きURL閲覧
	Aさん	true	できる	できる
	Bさん	false	できない	できる
	また、そのアクセストークンが無期限の生存期間で一度アクセストークン付きURLが露出してしまえば、誰でも無期限に閲覧できてしまう状態となります。
- 画像URLをFirestoreに保存する
  - Notes: ダメな使い方１
- 画像がCDN等でキャッシュされる・する
  - Notes: ダメな使い方２
- もしStorageの画像をStorageのセキュリティルールを通して閲覧したい場合は、FirestoreにはファイルのRefを保存し、閲覧を行うクライアント内でgetDownloadURL()を行う必要があります
  - Notes: 解決策
