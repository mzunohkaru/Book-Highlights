# DevinのKnowledgeで共通ルールを整備する

![](https://res.cloudinary.com/zenn/image/upload/s--BwcNVb1N--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Devin%25E3%2581%25AEKnowledge%25E3%2581%25A7%25E5%2585%25B1%25E9%2580%259A%25E3%2583%25AB%25E3%2583%25BC%25E3%2583%25AB%25E3%2582%2592%25E6%2595%25B4%25E5%2582%2599%25E3%2581%2599%25E3%2582%258B%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:shown_it%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2NjNWMzMjhmOTYuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: DevinのKnowledgeで共通ルールを整備する
- URL: https://zenn.dev/shown_it/articles/c201edc90d207b
- Last Updated on: 2025-06-17



### Highlights & Notes

- DevinのKnowledgeとは
- Devinがあらゆるセッションで参照できる指示やアドバイスの集まりであり、組織・プロジェクトのコンテキストをDevinに教えるためのもの
- 例えば、コードの規約、デプロイのワークフロー、PRの命名規則、テストのワークフロー、独自のツールとの連携方法などを登録できます。
- セッション中にDevinがどのKnowledgeを使用したかは、チャットの「Accessed Knowledge」で確認できます。
- Knowledgeの設定
- セッション開始時のルール
- ![](https://storage.googleapis.com/zenn-user-upload/fd101be9aac6-20250411.png)
- タイトル: セッション開始時ルール
	Devin uses this when: when starting any new task or session as Devin
	Content:
	Languageに関する指示がない場合、セッション内のやり取りと出力は全て日本語で行なってください。
	指示外の変更を行う場合は、その意図と概要について説明した上チャットで許可を求めてください。
	コードに破壊的な変更を行う場合は、その意図と概要について説明した上でチャットで許可を求めてください。
	特別な指示がない場合、Devinの1セッションにつきGitHubへpushするブランチは1つだけにしてください。
	2つ以上のブランチのpushを行う必要がある場合はチャットで許可を求めてください。
	作業ブランチのブランチ名にはprefix devin/ を付けてください。
- Gitリポジトリへの接続時のルール
- ![](https://storage.googleapis.com/zenn-user-upload/011ad8fbd867-20250411.png)
- タイトル: Gitリポジトリへの接続時のルール
	Devin uses this when: when connecting to or cloning a Git repository
	Content:
	Gitリポジトリへの接続時、最初に必ず以下の内容を確認した上で次のStepに移ってください。
	プロジェクト全体のディレクトリ構造の確認
	リポジトリ名にdocs が含まれている場合、全てのファイルの内容を確認
	Readme.mdが存在する場合、内容を確認
	docs ディレクトリが存在する場合、docs ディレクトリ以下のファイル全ての内容を確認
- 全てのリポジトリにピン留め
