# DevinにDevinが参照するドキュメントを整備してもらう

![](https://res.cloudinary.com/zenn/image/upload/s--a7MFCCyf--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Devin%25E3%2581%25ABDevin%25E3%2581%258C%25E5%258F%2582%25E7%2585%25A7%25E3%2581%2599%25E3%2582%258B%25E3%2583%2589%25E3%2582%25AD%25E3%2583%25A5%25E3%2583%25A1%25E3%2583%25B3%25E3%2583%2588%25E3%2582%2592%25E6%2595%25B4%25E5%2582%2599%25E3%2581%2597%25E3%2581%25A6%25E3%2582%2582%25E3%2582%2589%25E3%2581%2586%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:shown_it%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2NjNWMzMjhmOTYuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: DevinにDevinが参照するドキュメントを整備してもらう
- URL: https://zenn.dev/shown_it/articles/ae4f94d7ee0b1b
- Last Updated on: 2025-06-17



### Highlights & Notes

- プロジェクト毎のコンテキストをDevinが参照できる仕組み
- Gitリポジトリへの接続時のルールとして、docsディレクトリが存在する場合はその配下のファイル全ての内容を確認するようにという指示を追加
- 作成したドキュメントをDevinに読んでもらう
- タイトル: Gitリポジトリへの接続時のルール
	内容:
	Gitリポジトリへの接続時、最初に必ず以下の内容を確認した上で次のStepに移ってください。
	プロジェクト全体のディレクトリ構造の確認
	リポジトリ名に docs が含まれている場合、全てのファイルの内容を確認
	Readme.mdが存在する場合、内容を確認
	docs ディレクトリが存在する場合、docs ディレクトリ以下のファイル全ての内容を確認
- プロジェクトのコンテキストを理解した上でタスクに取り組むことが期待できます。
- プロジェクト単位のドキュメントを作成する
- プロジェクトの運用をドメイン知識のないエンジニア(Devin)に依頼することを想定し、用意しておくと良いドキュメントの作成をDevinに依頼
- - 既にコードに書かれている日本語のコメントは削除せず、残したまま整備してください。
	- 作業ブランチ名は devin/maintenance-phpdoc としてください。
	- 作業完了後、fugaブランチに対してプルリクエストを出してください。
