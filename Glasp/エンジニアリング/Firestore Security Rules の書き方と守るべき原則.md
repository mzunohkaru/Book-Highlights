# Firestore Security Rules の書き方と守るべき原則

![](https://res.cloudinary.com/zenn/image/upload/s--4f52sOy4--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Firestore%2520Security%2520Rules%2520%25E3%2581%25AE%25E6%259B%25B8%25E3%2581%258D%25E6%2596%25B9%25E3%2581%25A8%25E5%25AE%2588%25E3%2582%258B%25E3%2581%25B9%25E3%2581%258D%25E5%258E%259F%25E5%2589%2587%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:Kosuke%2520Saigusa%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2dSTlpXdHdUaVdPVUl6MHlhN2Z5bFpPYmtoNTduVko4a2p5VjhhNGc9czI1MC1j%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Firestore Security Rules の書き方と守るべき原則
- URL: https://zenn.dev/kosukesaigusa/articles/efc2528898954d95a6ae
- Last Updated on: 2024-12-10



### Highlights & Notes

- rules_version = '2';
	service cloud.firestore {
	    match /databases/{database}/documents {
	        match /{document=**} {
	            allow read, write: if true;
	        }
	    }
	}
  - Notes: ドキュメント全てにルールを当てれる
- read
	get：単一のドキュメントの取得
	list：クエリによるコレクション・複数ドキュメントの取得
	write
	create：ドキュメントの生成
	update：ドキュメントの一部のフィールドの更新
	delete：ドキュメントの削除
- create と update すらも、ルールを区別することを怠って全く同一の条件で許可するケースはほぼない、と認識しておきましょう
- スキーマ検証とデータのバリデーションも、Firestore Security Rules でチェックすべき大切な要素
- ルール（allow <読み書きのオペレーション>: if <許可する条件>;）を書くということは、セキュリティに穴をひとつずつ開けていくことに等しいと認識しておく
  - Notes: 許可すること自体がセキュリティに穴を開ける行為である
- update オペレーションについては、create オペレーションと類似のルールとなることはすぐに分かりますが、必ずしもすべてのフィールドが更新されるわけではありません。
  - Notes: createdAtなど
