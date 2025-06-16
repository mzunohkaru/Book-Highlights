# Devinにコードレビューをさせ、コード品質と開発速度を同時に高める話

![](https://res.cloudinary.com/zenn/image/upload/s--scaNBXTi--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Devin%25E3%2581%25AB%25E3%2582%25B3%25E3%2583%25BC%25E3%2583%2589%25E3%2583%25AC%25E3%2583%2593%25E3%2583%25A5%25E3%2583%25BC%25E3%2582%2592%25E3%2581%2595%25E3%2581%259B%25E3%2580%2581%25E3%2582%25B3%25E3%2583%25BC%25E3%2583%2589%25E5%2593%2581%25E8%25B3%25AA%25E3%2581%25A8%25E9%2596%258B%25E7%2599%25BA%25E9%2580%259F%25E5%25BA%25A6%25E3%2582%2592%25E5%2590%258C%25E6%2599%2582%25E3%2581%25AB%25E9%25AB%2598%25E3%2582%2581%25E3%2582%258B%25E8%25A9%25B1%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:Developer%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzJkZmVkY2VkZTAuanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:GLOBIS%2520Tech%2Cx_220%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzMyYzY4NDk1Y2EuanBlZw==%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png)

### Metadata

- Title: Devinにコードレビューをさせ、コード品質と開発速度を同時に高める話
- URL: https://zenn.dev/globis/articles/28e47f8107c5b5
- Last Updated on: 2025-06-15



### Highlights & Notes

- チューニング
- レビュー時のお作法を守らせる（AIは細かい作業に分解することが苦手な場合が多いため、ファイル単位でのチェックや処理の追い方といった手順をインプットしています）
	Railsアプリケーションにおける理想的な設計パターンをレビュー時に定着させる
	グロービス特有のドメイン知識・運用ルールを活用させる
- Knowledgeとプロンプトの使い分け
- DevinのKnowledge
	Devinへのプロンプト (実際のGithub Actions用YAML付き)
- 管理のしやすさを重視し、変動が大きく頻繁にアップデートする必要のある内容（たとえばRailsの設計パターンや自社特有のドメイン知識など）は、DevinのKnowledgeに集約
- レビュー時のお作法やチェックリストの作成手順といった、あまり頻繁に変わらない指示事項や基本ルールはプロンプト側に記載する
- 基本的にKnowledgeの内容は、Devin自身が自動で提案したものをベースとして、加筆・修正をしています。
- Knowledge: Railsの理想的な設計パターンを学習させる
- 設計パターンの事例を多数集め、Devinに事前学習させることで、適切なリファクタリングポイント（「この処理はモデルに寄せるべき」「サービス層を使うべきではない」など）を指摘できるようにしました
- Knowledge: グロービス特有のドメイン知識の蓄積
- 特有の運用ルールやドメイン知識をDevinのKnowledgeに組み込む
- プロンプト: Plannerを意識したプロンプト設計
- （例：コードレビュー）を段階的・体系的に整理し、抜け漏れなく実行するための仕組みです。例えば「PRの変更内容をチェック → 問題点を洗い出す → 推奨事項をリストアップ → 必要があれば既存知識を参照して深掘り」という形で、タスクをいくつかのステップに分解し、チェックリストや実行計画を自動で作成してくれます。
- シンプルな変更の場合でも漏れなくレビューが行えるよう、ファイルごとに明確なチェックリストを自動作成させています。
- ![](https://storage.googleapis.com/zenn-user-upload/de4d6b502220-20250310.png)
- GitHub ActionsでDevinを使い始める
- PR本文に書かなくても、PRコメント欄でdevin-reviewと書くだけでレビューが開始されるような改善も予定
- PRが新規作成された時 (opened)
	PRに新しいコミットが追加された時 (synchronize)
	PR本文が編集され、かつ本文にdevin-reviewというキーワードが追加された時 (edited)
