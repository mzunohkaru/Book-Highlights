# Devin の良い感じの使い方を模索する：草創編

![](https://res.cloudinary.com/zenn/image/upload/s--f80rSYLT--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Devin%2520%25E3%2581%25AE%25E8%2589%25AF%25E3%2581%2584%25E6%2584%259F%25E3%2581%2598%25E3%2581%25AE%25E4%25BD%25BF%25E3%2581%2584%25E6%2596%25B9%25E3%2582%2592%25E6%25A8%25A1%25E7%25B4%25A2%25E3%2581%2599%25E3%2582%258B%25EF%25BC%259A%25E8%258D%2589%25E5%2589%25B5%25E7%25B7%25A8%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:%25E3%2582%25AF%25E3%2583%25A9%25E3%2582%25A6%25E3%2583%2589%25E3%2582%25A8%25E3%2583%25BC%25E3%2582%25B9%25E6%25A0%25AA%25E5%25BC%258F%25E4%25BC%259A%25E7%25A4%25BE%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzE2OGMwYmM4MTIuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Devin の良い感じの使い方を模索する：草創編
- URL: https://zenn.dev/cloud_ace/articles/5f7036fb07c2bd
- Last Updated on: 2025-06-15



### Highlights & Notes

- Devin のライフサイクル
- ![](https://res.cloudinary.com/zenn/image/fetch/s--mnOtz-v8--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/0cab76a033a21b1e5900b507.jpg%3Fsha%3D784b2eafd341a4cb62a7bcddd858c584e8e646e2)
- 「特定のコンポーネントファイル内の関数の責務を分離したい」というタスクを与えてますが、一口に責務を分離したいといっても、どの関数をどのような観点で分離するのかを具体的な指示を与えていません。つまりは曖昧なタスク
- よくないタスク指示
- いいタスク指示
- サブタスクが提案されることは無く PR まで一貫して自律的にタスクが遂行され
- 何を以て成果物の品質を充足し得るかを明確にし、plan の実行可否を自律的に判断できる程度に Devin の認知負荷を排した上でお願いすることが肝要である
- Playbook
- 共有・再利用可能なシステムプロンプトで、タスクの精度を高めるためにユーザが定義できるシステムプロンプト
- Procedure: タスクの具体的な手順を記述
	Specifications: タスク完了時に満たすべき条件
	Advice: Devin へのアドバイスや注意点
	Forbidden Actions: 実行してはいけない操作等の制約事項
	Required from User: タスクを遂行するにあたり必要なユーザーから提供されているべき情報
- タスクを遂行する手順と成果物として必要なものを具体的に定義している他、変更されているテーブルに対してアプリケーションで操作が実行されている場合、その操作のクエリも変更する必要がある旨、タスクを実行する際には静的解析やテストの実行は必要ない旨を記載
- 定型化可能な作業タスクはどんどん Playbook 化してチーム内で共有化すると諸々捗る
- Knowledge
- Devin がタスクを遂行する上で重要なルールや技術的背景を登録できる機能
- 例えば、プロジェクトで採用しているブランチ戦略に関するルールなど
- コミットメッセージの規約として Conventional Commits を採用している等のルールや、インフラリソースや依存関係を勝手に追加してはならない等の制約事項も有効
- 「アプリケーションの特定の関数で大きいレイテンシが発生しているので原因箇所を特定して解消してほしい」という指示を与えたところ(これも Devin に想像力を要求している点でアンチパターンな指示
- Devian's Workspace
- Devin はタスクを進行していく際に仮想マシン内で指定されたリポジトリからソースコードを取得しますが、それぞれリポジトリ単位でリポジトリを clone する時の振る舞い、依存関係の取得、静的解析、テストやローカルアプリケーションの実行手順まで任意に指定
- タスクの達成条件として、テスト結果やローカルでの検証等を指定する場合にコンテクストとして重要になって
